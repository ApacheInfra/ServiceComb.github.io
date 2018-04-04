---
title: "Development Style-Transparent RPC"
lang: en
ref: develop-with-transparent-rpc
permalink: /users/develop-with-transparent-rpc/
excerpt: "Development Style-RPC"
last_modified_at: 2017-08-15T15:01:43-04:00
redirect_from:
  - /theme-setup/
---

{% include toc %}
## Concept Description

The transparent remote procedure call(RPC) development mode is a development mode based on API and API implementation. The service developer does not need to use the description of Spring MVC and JAX-RS.

## Development Example

The transparent RPC development mode supports two service release mode: Spring XML configuration and annotation configuration. The Spring XML configuration mode is as follows:

* **Step 1** Define a service API. Compile the Java API definition based on the API definition defined before development. The code is as follows:

   ```java
   public interface Hello {
     String sayHi(String name);
     String sayHello(Person person);
   }
   ```

   > **NOTE**：
   > The location of the API must be the same as the path specified by x-java-interface in the API definition.

* **Step 2** implement the service. The implementation of the Hello service is as follows:

   ```java
   import org.apache.servicecomb.samples.common.schema.Hello;
   import org.apache.servicecomb.samples.common.schema.models.Person;

   public class HelloImpl implements Hello {
     @Override
     public String sayHi(String name) {
       return "Hello " + name;
     }

     @Override
     public String sayHello(Person person) {
       return "Hello person " + person.getName();
     }
   }
   ```

* **Step 3** Release the service. Create the pojoHello.bean.xml file in the resources/META-INF/spring directory and declare the schema in the file. The content of the file is as follows:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns=" http://www.springframework.org/schema/beans " xmlns:xsi=" http://www.w3.org/2001/XMLSchema-instance "
          xmlns:p=" http://www.springframework.org/schema/p " xmlns:util=" http://www.springframework.org/schema/util "
          xmlns:cse=" http://www.huawei.com/schema/paas/cse/rpc "
          xmlns:context=" http://www.springframework.org/schema/context "
          xsi:schemaLocation=" http://www.springframework.org/schema/beans classpath:org/springframework/beans/factory/xml/spring-beans-3.0.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd http://www.huawei.com/schema/paas/cse/rpc classpath:META-INF/spring/spring-paas-cse-rpc.xsd">

       <cse:rpc-schema schema-id="pojoHello" implementation="org.apache.servicecomb.samples.pojo.provider.PojoHelloImpl"/>
   </beans>
   ```

   > **NOTE**：
   > A schema statement must be defined for each service API.

## The Develop Method by Configure Remarks

1. Define a service API, which is the same as the Spring XML mode.

2. Implement the service in the same way as using Spring XML.

3. Release the service. @RpcSchema is used to define schema during the API Hello implementation. The code is as followss:

   ```java
   import org.apache.servicecomb.provider.pojo.RpcSchema;
   // other code omitted
   @RpcSchema(schemaId = "pojoHello")
   public class HelloImpl implements Hello {
     // other code omitted
   }
   ```

   In the pojoHello.bean.xml file of resources/META-INF/spring directory, configure base-package that performs scanning. The content of the file is as follows:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns=" http://www.springframework.org/schema/beans " xmlns:xsi=" http://www.w3.org/2001/XMLSchema-instance "
          xmlns:p=" http://www.springframework.org/schema/p " xmlns:util=" http://www.springframework.org/schema/util "
          xmlns:cse=" http://www.huawei.com/schema/paas/cse/rpc "
          xmlns:context=" http://www.springframework.org/schema/context "
          xsi:schemaLocation=" http://www.springframework.org/schema/beans classpath:org/springframework/beans/factory/xml/spring-beans-3.0.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd http://www.huawei.com/schema/paas/cse/rpc classpath:META-INF/spring/spring-paas-cse-rpc.xsd">

       <context:component-scan base-package="org.apache.servicecomb.samples.pojo.provider"/>
   </beans>
   ```

> **NOTE**:
> Different from the Spring MVC and JAX-RS development modes, the transparent RPC development mode used `@RpcSchema` instead of `@RestSchema`.
