---
title: "Consul Service Mesh实战"
lang: cn
ref: consul-servicemesh
permalink: /cn/docs/consul-servicemesh
excerpt: "Consul Service Mesh介绍与相关实践"
last_modified_at: 2018-07-21T10:26:28+08:00
author: Zhen Ju
tags: [microservice, servicemesh, consul]
redirect_from:
  - /theme-setup/
---

## Consul Service Mesh实战

在最近发布的[Consul1.2](https://github.com/hashicorp/consul/releases)中，[HashiCorp](https://www.hashicorp.com/)宣布支持Service Mesh。作为一个优秀的分布式服务发现解决方案，Consul是如何支持Service Mesh的呢？本文将带读者一探究竟。



在Consul 1.2的[release blog](https://www.hashicorp.com/blog/consul-1-2-service-mesh)中，Consul定义了Service Mesh的3个要素：

1. **Discovery**：服务可以互相找到
2. **Configuration**：服务在运行时，可以从一个集中的源头获取配置
3. **Segmentation**：服务之间的通信必须是加密并通过认证的

实际上，Consul本身已经支持服务发现，并且实现了一个集群共享的KV存储方案，可以实现服务配置的存储。而最新的Consul 1.2中，新发布的Connect功能，可以为服务创建一个网络层的代理，这成为Consul Service Mesh方案中，数据面代理的基础。接下来，我们从这三个方面一一对Consul进行解析。

#### Discovery

Consul是一个跨数据中心的分布式服务发现解决方案。在Consul集群的每一个节点上，都运行一个Consul进程，成为一个Consul agent，一个agent可以以server或client两种模式运行。Client agent非常轻量，只需与Server agent交互，并维护Client自身很少的状态。而Server agent除了Client agent的所有功能，还提供额外的能力来完成基于Raft的一致性方案，因此Server agent的负载会更高，也需要更多的资源，运行在更加稳定的节点上。Consul agent可以通过join命令，指定集群中任意一个agent的IP，以加入集群，形成一个Consul cluster。如下图所示：

![consul-cluster](https://github.com/crystaldust/imagestorage/raw/master/consul-servicemesh/consul-cluster.jpg)

在单个节点上，Consul agent读取该节点服务的配置文件，获取该节点上服务的信息，并与cluster中其他agent通过gossip协议互相通信，完成集群内的服务注册与发现。当Consul cluster中的服务需要调用另一个服务时，可以通过API和DNS解析两种方式，向所在节点的Consul agent获取目标服务的具体地址，然后调用目标服务。



#### Configuration

除了服务注册与发现，Consul还内置了一个KV存储系统，用于Cluster之间共享数据。KV的Key是分层的，通过斜线`/`分隔，这样，可以在概念上对key进行组织和管理，用来保存不同的配置，并把配置分配给集群内的所有服务。例如，在一个集群内，运行了`webfront`服务，`webfront`服务需要获知当前是开发环境还是测试环境，以提供不同的页面表现。那么，我们在可以在节点A上用KV存储保存环境信息：

```bash
nodeA$ consul kv put services/webapi/metadata/env dev #假设env=dev表示测试环境
```

服务运行过程中，假设任务调度器将`webfront`服务的实例调度到节点B和节点C，那么在节点B和C上可以通过Consul agent访问到环境信息：

```bash
nodeB$ consul kv get services/webapi/metadata/env
nodeB$ dev
```

当然，在实际的使用中，我们不仅可以通过命令行来获取KV数据，也可以通过RESTful API，向所在节点的Consul agent发送HTTP请求来获取数据。

#### Segmentation

按照Consul的说明，Segmentation的意义在于使服务之间的网络请求都是经过TLS加密并认证的。那么，Consul是如何做到请求的自动TLS加密和认证的呢？

Consul 1.2新增了Connect功能，只需要在服务中增加一个`connect`配置，即可为该服务启动一个代理：

```json
{
    "service": {
        "name": "webfront",
        "connect": {
            "proxy": {}
        }
    }
}
```

这样，Consul agent就会为`webfront`服务启动一个内置的proxy，proxy启动时，向所在节点的Consul agent（为了便于说明，假设为Client agent，其实Client/Server都是可能的）请求`webfront`所需要的root证书和leaf证书。Client agent检查本地的缓存，若已有所需证书，则直接将证书和私钥返回。否则就会生成一个新的私钥，并向Server agent发送CSR。Server对CSR进行验证，并返回签名的证书，Client agent拿到证书，把证书和私钥返回给proxy。这样，proxy就可以接受TLS请求了。这个过程如下：

![consul-tls](https://github.com/crystaldust/imagestorage/raw/master/consul-servicemesh/consul-tls.jpg)

并且，Consul中还有一个自动更新的机制，当Server中的根证书变化时，Client重新生成私钥，向Server发送CSR，获取签名的证书，并把签名的证书和私钥再发给proxy，完成证书的更新。

了解了Consul Connect，我们会发现，其实Connect功能帮每个服务启动了一个Proxy，对服务的inbound/outbound请求进行了TLS加密和认证，这个场景是否似曾相识？没错，就是Service Mesh中数据面sidecar的概念。只是这个Consul内建的sidecar目前能实现的功能还比较单一，只有TLS加密和认证。

现在，我们对Consul Service Mesh的三个要素都有了较为具体了解，那么，相比于一般意义上的Service Mesh方案，例如Istio，Conduit等，Consul的Service Mesh有何不同呢？我们不妨以Istio为例，看看Istio中是如何实现这三个要素的：

**Discovery**：由Pilot结合服务发现机制（kubernetes，eureka，consul等）完成

**Configuration**：由Pilot结合Kubernetes完成，用户使用kubectl定义Istio所需的Kubernetes资源，Pilot读取到这些资源后，转换为自己的Model，并通过xDS API将配置信息下发给Envoy

**Segmentation**：Citadel结合Envoy完成，Citadel将证书颁发给每个服务的Envoy

因此，Istio的结构是这样的：

![istio-architecture](https://github.com/crystaldust/imagestorage/raw/master/consul-servicemesh/istio-architecture.jpg)

可以看到Discovery和Configuration都是在Istio控制面完成的。Segmentation是由控制面的Citadel+数据面的Envoy代理完成的。

根据HashiCorp的说明，在Consul的Service Mesh方案中，Consul作为控制面运行，Consul agent实现了服务发现和配置存储。同时，Consul agent再结合Connect启动的Proxy，完成了网络流量的加密与认证。如下图所示：

![consul-proxy-certs](https://github.com/crystaldust/imagestorage/raw/master/consul-servicemesh/consul-proxy-certs.jpg)

请注意，Consul的KV系统只完成了配置的存储，并没有像Istio一样，可以对配置进行动态修改并下发到数据面，目前Proxy的配置也是通过读取文件，然后在启动或重启代理时传入的，因此我们认为Consul的配置管理能力还需要完善。此外，因为Consul Connect启动的Proxy是一个4层代理，无法提供路由、熔断、重试等更高级的微服务治理能力，因此也不需要获取服务治理的配置。这是Consul Service Mesh的方案中治理能力欠缺的地方。不过，HashiCorp也表示Connect在1.2版本中只是Beta状态，未来会继续围绕Connect开发新的功能，包括UI的增强，支持Envoy作为proxy，与Nomad和Kubernetes的集成等。

#### 等等，听说Consul已经可以集成Envoy了？

没错！其实，在Consul1.2中，Connect已经可以通过指定一个可执行文件的路径来启动第三方代理了。Consul会以daemon模式启动第三方代理，在后台运行：

```json
{
    "service": {
        "name": "webfront",
        "connect": {
            "proxy": {
                "exec_mode": "daemon",
                "command": ["/usr/bin/my-proxy", "--listen", "8800"]
            }
        }
    }
}
```

那么，按照通常的思路，是不是可以直接指定Envoy，让Consul启动Envoy来实现集成呢？理论上确实可行，但是Consul是无法管理第三方代理的配置的，Envoy的配置只能由用户自行定义，增加了运维成本，也不便于管理。

这时，就轮到HashiCorp好基友[solo.io](https://www.solo.io/)出品的[Gloo Connect](https://github.com/solo-io/gloo-connect)闪亮登场了！solo.io是一家提供Cloud Native工具和集成方案的公司，例如支持Kubernetes和Istio的微服务debugger [squash](https://github.com/solo-io/squash)，基于Envoy的function gateway解决方案[Gloo](https://github.com/solo-io/gloo)。在刚刚过去的[HashiDay 2018](https://www.hashidays.com/)上，solo.io和HashiCorp联合发布了Gloo Connect。Gloo产品线本身很有意思，Gloo与英文单词"胶水"(Glue)的发音一样，暗示着Gloo会作为"胶水"，将Cloud Native中的各种服务粘合在一起，那么，Gloo Connect是如何把Consul和Envoy粘合在一起的呢？

Gloo Connect作为Consul的第三方代理，在启动时会把Consul Connect proxy的配置和Gloo的配置转换成Envoy的配置，并启动Envoy实例。同时，Gloo Connect也可以通过命令行对Envoy的配置进行动态修改。下面，我们根据Gloo Connect的官方示例来演示如何将Consul与Envoy集成。

###### Tips

根据Gloo Connect的[Getting Started文档](https://github.com/solo-io/gloo-connect/blob/master/docs/getting-started/README.md)，Consul 1.2.0中，存在一个已知bug，运行示例需要下载一个[预先编译的Consul版本](https://github.com/solo-io/gloo-connect/releases/download/v0.1.0/consul)，或者使用1.2.1版本，并把Consul配置到系统路径，或者当前目录。

首先，我们将gloo-connect项目clone到本地，进入getting-started目录，并编译运行`service.go`：

```bash
$ git clone https://github.com/solo-io/gloo-connect/
$ cd gloo-connect/docs/getting-started
$ go run service.go &
```

`service.go`运行后监听8080端口，交替返回200和500状态码。接下来，我们执行当前目录下的`run-consul.sh`脚本，该脚本会在`./run-data/consul-config`目录下生成两个配置信息`connect.json`和`service.json`，前者将gloo-connect指定为Consul Connect默认的proxy，后者定义了两个服务`microsvc1`（即我们运行的service.go） 和`test`，并定义了两者之间的代理：`microsvc1`配置了一个空规则的代理，`test`配置了一个监听1234端口的代理，任何访问1234端口的请求，都将被当作`test`向`microsvc1`的访问。最后，脚本启动consul，并设定consul的配置目录为`./run-data/consul-config`。此外，脚本还会运行gloo-connect和envoy，需要确保两者在系统路径或当前目录下并具有执行权限，gloo-connect项目的发布页面可以[下载编译好的gloo-connect和envoy](https://github.com/solo-io/gloo-connect/releases)

脚本启动后，我们通过进程管理工具，可以看到Consul为每个服务启动了第三方代理gloo-connect，而每个gloo-connect又启动了一个Envoy进程，并把转换好的配置传给Envoy：

![gloo-connect](https://github.com/crystaldust/imagestorage/raw/master/consul-servicemesh/gloo-connect.png)

此时，真正监听1234端口的就是Envoy进程了。下面，我们访问1234端口，会看到服务交替返回状态码200和500：

![req-microsvc1-by-proxy](https://github.com/crystaldust/imagestorage/raw/master/consul-servicemesh/req-microsvc1-by-proxy.png)

我们用gloo-connect命令为`microsvc1`的Envoy sidecar配置一条retry规则：

```bash
$ ./gloo-connect set service microsvc1 --http --retries=3
```

这样，当通过Envoy代理访问`microsvc1`时，如果遇到500，会重试3次，其实在第一次重试时就会返回200了，因此我们访问1234端口，每次都会返回200：

![gloo-connect-retries](https://github.com/crystaldust/imagestorage/raw/master/consul-servicemesh/gloo-connect-retries.png)

这样，我们就借助一个第三方工具gloo-connect，实现了Consul和Envoy的集成！需要说明的是，gloo-connect也处于早期阶段，目前支持的配置还比较少，除了`retries`，只支持`http`（是否使用http模式，默认为false）和`timeout`。

#### 总结

目前来看，Consul Connect虽然还刚刚起步，但是已经让我们非常惊喜的看到了一个相对完整、简单易用的Service Mesh方案，通过第三方的集成，未来也许会有更多优秀的代理，甚至是用户自己定义的代理，不断加入到Consul的阵营中。随着Consul Connect的不断演进和HashiCorp的持续投入，我们相信，Consul会在未来的Service Mesh竞争中占有一席之地，也希望业界不断涌现Consul这样优秀的方案，给用户更多选择，让Service Mesh的世界越来越精彩。

#### 后记

不同于Istio、Conduit等“天生Service Mesh”的微服务方案，Consul本身是一个成熟的服务发现工具，其实现Service Mesh的思路非常有意思。在笔者看来，除去“控制面”“数据面”这种基于大的框架角度的划分，Service Mesh从功能上要完成三件事情：服务注册与发现，配置管理（存储、下发），服务治理。Consul在第一点已经做得非常好了，配置方面，Consul的集群内共享KV为实现配置管理奠定了坚实的基础，完全可以通过第三方工具、或者Consul自身增强的特性，基于KV实现配置的下发。服务治理方面与Consul本身关系不大，但是通过Connect，Consul具备了为服务配置代理或者集成第三方代理的能力，完全可以用已有的优秀代理来实现Service Mesh数据面，补足服务治理的能力。

Consul的方案，也给已经在业界流行并经过生产环境验证的“上一代”微服务框架带来很多启发。在Service Mesh概念流行之前，已有很多基于SDK的侵入式微服务框架。这些框架往往提供服务发现机制或者或者兼容开源的服务发现机制，有一些也提供配置的管理，只是需要在应用程序中引入SDK来实现服务治理，因此对于不同的语言，都要提供相应的SDK。但是相对于Consul，毕竟这些框架已经具备了微服务治理的能力，它们向Service Mesh转型的过程中，更需要的是将治理能力从SDK转移到网络层面的代理中。在这一点上，华为开源的微服务框架ServiceComb的做法非常值得参考。在ServiceComb中，已有Service Center实现服务的注册与发现，基于archaius的配置更新机制，以及提供治理能力的SDK（支持Java和Go两种语言），为了实现数据面的代理，ServiceComb团队将SDK中实现治理能力的代码剥离出来，基于此开发了网络代理[Mesher](https://github.com/go-chassis/mesher)，Mesher的治理能力与SDK是基本对等的，并且共享同一套配置。这样，已经使用SDK的服务，不需要做任何修改，而使用其他语言开发的新应用，则可以通过Mesher获得ServiceComb的服务治理能力。最终，ServiceComb成为同时支持SDK和网络代理的混合式微服务方案，也是一个真正意义上的Service Mesh方案。
