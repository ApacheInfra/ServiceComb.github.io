---
title: "Communication Between Service-Center and MicroServices"
lang: cn
ref: communication-btw-sc-ms
permalink: /cn/docs/communication-btw-sc-ms/
excerpt: "An introduction of how service-center and microservices work together"
last_modified_at: 2017-08-09T09:18:43+08:00
author: Asif Siddiqui
tags: [服务中心]
redirect_from:
  - /theme-setup/
---

Service-Center([SC](https://github.com/ServiceComb/service-center)) is a service registry that allows services to register their instance information and to discover providers of a given service.
SC uses etcd to store all the information of micro-service and its instances. Below is the diagram stating the working principles and flow of SC.

#### On StartUp
Here we assume that micro-services are written using [java-chassis](https://github.com/ServiceComb/java-chassis) sdk. So when micro-service boots up then java-chassis sdk does the following list of tasks.

1. On startup provider registers the micro-service to SC if not registered earlier and also register its instance information like its Ip and Port on which instance is running.

2. SC stores the provider information in etcd.

3. On startup consumer retrieves the list of all provider instance from SC using the micro-service name of the provider.

4. Consumer sdk stores all the information of provider instances in its cache.

5. Consumer sdk creates a web socket connection to SC to watch all the provider instance information, if there is any change in the provider then sdk updates it's cache information.

![OnStartup]({{ site.url }}{{ site.baseurl }}/assets/images/onStartup.PNG){: .align-center}

#### Communication between Consumer -> Provider
Once the bootup is successful then the consumer can communicate with providers flawlessly, below is the diagram illustrating the communication between provider and consumer.

![Communication]({{ site.url }}{{ site.baseurl }}/assets/images/communication.PNG){: .align-center}

Provider instance regularly sends heartbeat signal every 30 seconds to SC, if SC does not recieve the heartbeat for particular instance then the information in etcd expires and the provider instance information is removed.  
Consumer watches the information of provider instances from SC and if there is any change then the cache is updated.  
When Consumer needs to communicate to Provider then consumer reads endpoints of the provider instances from cache and do loadbalancing to communicate to Provider.
