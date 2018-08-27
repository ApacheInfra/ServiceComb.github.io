---
title: "ServiceComb Service-Center Binaries"
lang: cn
ref: release
permalink: /cn/release/service-center-binary/
excerpt: "ServiceComb Service-Center Binaries"
last_modified_at: 2018-03-28T00:50:43-55:00
---

## Releases

* Apache ServiceComb (incubating) Service-Center 1.0.0-m1
    - Source [[src]](https://apache.org/dyn/closer.cgi/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-src.zip) [[asc]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-src.zip.asc) [[sha512]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-src.zip.sha512) 
    - Windows [[Binary]](https://apache.org/dyn/closer.cgi/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-windows-amd64.tar.gz) [[asc]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-windows-amd64.tar.gz.asc) [[sha512]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-windows-amd64.tar.gz.sha512)
    - Linux [[Binary]](https://apache.org/dyn/closer.cgi/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-linux-amd64.tar.gz) [[asc]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-linux-amd64.tar.gz.asc) [[sha512]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/1.0.0-m1/apache-servicecomb-incubating-service-center-1.0.0-m1-linux-amd64.tar.gz.sha512)

**Verifying the release**

It is essential that you verify the integrity of the downloaded files using the PGP or SHA signatures.
 The PGP signatures can  be verified using GPG or PGP. 
 Please download the [KEYS](https://www.apache.org/dist/incubator/servicecomb/KEYS){:target="_blank"} as well as the asc signature files for relevant distribution. It is recommended to get these files from the main distribution [directory](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/){:target="_blank"} and not from the mirrors.
 ```
 gpg -i KEYS
 
 or
 
 pgpk -a KEYS
 
 or
 
 pgp -ka KEYS

```

To verify the binaries/sources you can download the relevant asc files for it from main distribution directory and follow the below guide.

```
gpg --verify apache-servicecomb-incubating-service-center-********.asc apache-servicecomb-incubating-service-center-*********

or

pgpv apache-servicecomb-incubating-service-center-********.asc

or 

pgp apache-servicecomb-incubating-service-center-********.asc


```

Alternatively you can download the SHA signatures from main distribution [repo](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-service-center/){:target="_blank"} and verify the downloads using sha512sum.
