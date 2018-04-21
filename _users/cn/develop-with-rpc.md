---
title: "使用RPC方式开发"
lang: cn
ref: develop-with-rpc
permalink: /cn/users/develop-with-rpc/
excerpt: "使用RPC方式开发"
last_modified_at: 2017-08-15T15:01:43-04:00
redirect_from:
  - /theme-setup/
---

{% include toc %}
## 概念阐述

RPC开发方式允许用户通过在服务接口上标注注解来生成服务提供者代理，从而进行服务的调用。

## 示例代码

只需要声明一个服务接口类型的成员，并且使用`@RpcReference`标注该成员，声明依赖的微服务及schemaId，即可进行服务调用，示例代码如下：

```java
import org.springframework.stereotype.Component;

import org.apache.servicecomb.foundation.common.utils.BeanUtils;
import org.apache.servicecomb.foundation.common.utils.Log4jUtils;
import org.apache.servicecomb.provider.pojo.RpcReference;
import org.apache.servicecomb.samples.common.schema.Hello;
import org.apache.servicecomb.samples.common.schema.models.Person;

@Component
public class CodeFirstConsumerMain {
    @RpcReference(microserviceName = "codefirst", schemaId = "codeFirstHello")
    private static Hello hello;

    public static void main(String[] args) throws Exception {
        init();
        System.out.println(hello.sayHi("Java Chassis"));
        Person person = new Person();
        person.setName("ServiceComb/Java Chassis");
        System.out.println(hello.sayHello(person));
    }

    public static void init() throws Exception {
        Log4jUtils.init();
        BeanUtils.init();
    }
}
```

在以上代码中，服务消费者已经取得了服务提供者的服务接口`Hello`，并在代码中声明一个`Hello`类型的成员。通过在`hello`上使用`@RPCReference`注解指明微服务名称和schemaId，ServiceComb框架可以在程序启动时从服务中心获取到对应的服务提供者实例信息，并且生成一个代理注入到hello中，用户可以像调用本地类一样调用远程服务。

### 调用方式的补充说明
上面示例代码中，为了能在main函数中直接使用hello变量，我们将它标记为`static`。作为CodeFirstConsumerMain这个Bean的本地变量，我们更推荐下面两种做法：
#### 方式1：通过cse:rpc-reference定义
在你的bean.xml中添加cse:rpc-reference的配置项：

```xml
<cse:rpc-reference id="hello" microservice-name="codefirst"
    schema-id="codeFirstHello" interface="org.apache.servicecomb.samples.common.schema.Hello"></cse:rpc-reference>
```

然后就可以使用`BeanUtils.getBean`直接获取服务提供者的服务接口`Hello`：

```java
Hello hello = BeanUtils.getBean("hello");
```

#### 方式2：获取Bean，再获取接口
先使用`BeanUtils.getBean`获取到CodeFirstConsumerMain这个Bean：

```java
//Spring Bean 实例默认名为类名的小写
CodeFirstConsumerMain consumer = BeanUtils.getBean("codeFirstConsumerMain");
```

然后按Getter的方式获取hello：

```java
public Hello getHello() {
    return hello;
}
```

```java
Hello hello = consumer.getHello()
```

> 说明：
> `BeanUtils.getBean`有锁，因此有性能问题，无论是哪种方式，推荐一次调用（例如在构造函数中）获取缓存起来作为一个本地变量反复使用。