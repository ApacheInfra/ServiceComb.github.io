---
title: "ServiceComb Service-Center下载"
lang: cn
ref: release
permalink: /cn/release/service-center-downloads/
excerpt: "ServiceComb Service-Center Downloads"
last_modified_at: 2018-03-28T00:50:43-55:00
---

## 发布包

* Apache ServiceComb (incubating) Service-Center 1.0.0-m2 (Latest)
    - 源码 [[src]](https://apache.org/dyn/closer.cgi/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m2/apache-servicecomb-incubating-service-center-1.0.0-m2-src.zip) [[asc]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m2/apache-servicecomb-incubating-service-center-1.0.0-m2-src.zip.asc) [[sha512]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m2/apache-servicecomb-incubating-service-center-1.0.0-m2-src.zip.sha512) 
    - Windows版本 [[Binary]](https://apache.org/dyn/closer.cgi/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m2/apache-servicecomb-incubating-service-center-1.0.0-m2-windows-amd64.tar.gz) [[asc]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m2/apache-servicecomb-incubating-service-center-1.0.0-m2-windows-amd64.tar.gz.asc) [[sha512]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m2/apache-servicecomb-incubating-service-center-1.0.0-m2-windows-amd64.tar.gz.sha512)
    - Linux版本 [[Binary]](https://apache.org/dyn/closer.cgi/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m2/apache-servicecomb-incubating-service-center-1.0.0-m2-linux-amd64.tar.gz) [[asc]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m2/apache-servicecomb-incubating-service-center-1.0.0-m2-linux-amd64.tar.gz.asc) [[sha512]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m2/apache-servicecomb-incubating-service-center-1.0.0-m2-linux-amd64.tar.gz.sha512)

* Apache ServiceComb (incubating) Service-Center 1.0.0-m1
    - 源码 [[src]](https://apache.org/dyn/closer.cgi/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-src.zip) [[asc]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-src.zip.asc) [[sha512]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-src.zip.sha512) 
    - Windows版本 [[Binary]](https://apache.org/dyn/closer.cgi/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-windows-amd64.tar.gz) [[asc]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-windows-amd64.tar.gz.asc) [[sha512]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-windows-amd64.tar.gz.sha512)
    - Linux版本 [[Binary]](https://apache.org/dyn/closer.cgi/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-linux-amd64.tar.gz) [[asc]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-linux-amd64.tar.gz.asc) [[sha512]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-linux-amd64.tar.gz.sha512)

**发布包验证**

使用PGP或SHA签名验证下载文件的完整性是很有必要的。PGP签名可以使用GPG或PGP进行验证，请下载 [KEYS](https://www.apache.org/dist/incubator/servicecomb/KEYS)以及相关发行的asc签名文件。
建议从主发行[目录](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/){:target="_blank"} 中获取这些文件，而不是从镜像中获取这些文件。.
 ```
 gpg -i KEYS
 
 or
 
 pgpk -a KEYS
 
 or
 
 pgp -ka KEYS

 ```

要验证二进制文件或源代码，您可以从主发行目录下载相关的asc文件并按照以下指南进行操作。

```
gpg --verify apache-servicecomb-incubating-service-center-********.asc apache-servicecomb-incubating-service-center-*********

or

pgpv apache-servicecomb-incubating-service-center-********.asc

or 

pgp apache-servicecomb-incubating-service-center-********.asc


```

另外，您也可以从主发行[仓库](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/){:target="_blank"}下载SHA签名并使用sha512sum验证。
