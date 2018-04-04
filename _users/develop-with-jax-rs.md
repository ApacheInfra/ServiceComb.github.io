---
title: "Development Style-JAX-RS"
lang: en
ref: develop-with-jax-rs
permalink: /users/develop-with-jax-rs/
excerpt: "Development Style-JAX-RS"
last_modified_at: 2017-08-15T15:01:43-04:00
redirect_from:
  - /theme-setup/
---

{% include toc %}

## Concept Description

ServiceComb supports developers in developing services in JAX-RS mode by using JAX-RS.

## Development Example

* **Steps 1** Define a service API. Compile the Java API definition based on the API definition defined before development. The code is as follows:

   ```java
   public interface Hello {
    　String sayHi(String name);
    　String sayHello(Person person);
   }
   ```

   > **NOTE**：
   > The location of the API must be the same as the path specified by x-java-interface in the API definition.

* **Step 2** Implement the service. JAX-RS is used to describe the development of service code. The implementation of the Hello service is as follows:

   ```java
   import javax.ws.rs.POST;
   import javax.ws.rs.Path;
   import javax.ws.rs.Produces;
   import javax.ws.rs.core.MediaType;
   import org.apache.servicecomb.samples.common.schema.Hello;
   import org.apache.servicecomb.samples.common.schema.models.Person;

   @Path("/jaxrshello")
   @Produces(MediaType.APPLICATION_JSON)
   public class JaxrsHelloImpl implements Hello {
     @Path("/sayhi")
     @POST
     @Override
     public String sayHi(String name) {
     　return "Hello " + name;
     }

     @Path("/sayhello")
     @POST
     @Override
     public String sayHello(Person person) {
       return "Hello person " + person.getName();
     }
   }
   ```

* **Step 3** Release the service. Add ```@RestSchema``` as the annotation of the service implementation class and specify schemaID, which indicates that the implementation is released as a schema of the current microservice. The code is as follows:

   ```java
   import org.apache.servicecomb.provider.rest.common.RestSchema;
   // other code omitted
   @RestSchema(schemaId = "jaxrsHello")
   public class JaxrsHelloImpl implements Hello {
     // other code omitted
   }
   ```

   Create the jaxrsHello.bean.xml file in the resources/META-INF/spring directory and configure base-package that performs scanning. The content of the file is as follows:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns=" http://www.springframework.org/schema/beans " xmlns:xsi=" http://www.w3.org/2001/XMLSchema-instance "
          xmlns:p=" http://www.springframework.org/schema/p " xmlns:util=" http://www.springframework.org/schema/util "
          xmlns:cse=" http://www.huawei.com/schema/paas/cse/rpc "
          xmlns:context=" http://www.springframework.org/schema/context "
          xsi:schemaLocation=" http://www.springframework.org/schema/beans classpath:org/springframework/beans/factory/xml/spring-beans-3.0.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd http://www.huawei.com/schema/paas/cse/rpc classpath:META-INF/spring/spring-paas-cse-rpc.xsd">

       <context:component-scan base-package="org.apache.servicecomb.samples.jaxrs.provider"/>
   </beans>
   ```

## Involved APIs

Currently, the JAX-RS development mode supports the following annotation. For details about how to use JAX-RS, see [JAX-RS official documentation](https://jax-rs-spec.java.net/nonav/2.0-rev-a/apidocs/index.html)。

| Remarks                 | Location         | Description                              |
| :---------------------- | :--------------- | :--------------------------------------- |
| javax.ws.rs.Path        | schema/operation | URL path                                 |
| javax.ws.rs.Produces    | schema/operation | Coding/decoding capability supported by the method |
| javax.ws.rs.DELETE      | operation        | http method                              |
| javax.ws.rs.GET         | operation        | http method                              |
| javax.ws.rs.POST        | operation        | http method                              |
| javax.ws.rs.PUT         | operation        | http method                              |
| javax.ws.rs.QueryParam  | parameter        | Obtain parameters from query string      |
| javax.ws.rs.PathParam   | parameter        | Obtain parameters from path, This parameter must be defined in path. |
| javax.ws.rs.HeaderParam | parameter        | Obtain parameters from header.           |
| javax.ws.rs.CookieParam | parameter        | Obtain parameters from cookie.           |

> **NOTE**：
- When the method parameter has no annotation and is not an HttpServletRequest parameter, the parameter is of the body type by default. One method supports a maximum of one body-typed parameter.
- You are advised to explicitly define the value of the parameter. Otherwise, the parameter name in the API definition is used, such as `@QueryParam\("name"\) String name` String name instead of `@QueryParam String name`.
