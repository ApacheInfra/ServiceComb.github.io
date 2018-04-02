---
title: "ServiceComb Saga Downloads"
lang: en
ref: release
permalink: /release/saga-downloads/
excerpt: "ServiceComb Saga Downloads"
last_modified_at: 2018-03-28T00:50:43-55:00
---

## Releases

* Apache ServiceComb (incubating) Saga 0.1.0
    - Source [[src]](https://apache.org/dyn/closer.cgi/incubator/servicecomb/incubator-servicecomb-saga/0.1.0/apache-servicecomb-incubating-saga-distribution-0.1.0-src.zip) [[asc]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-saga/0.1.0/apache-servicecomb-incubating-saga-distribution-0.1.0-src.zip.asc) [[sha512]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-saga/0.1.0/apache-servicecomb-incubating-saga-distribution-0.1.0-src.zip.sha512)
    - Distribution [[Binary]](https://apache.org/dyn/closer.cgi/incubator/servicecomb/incubator-servicecomb-saga/0.1.0/apache-servicecomb-incubating-saga-distribution-0.1.0-bin.zip) [[asc]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-saga/0.1.0/apache-servicecomb-incubating-saga-distribution-0.1.0-bin.zip.asc) [[sha512]](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-saga/0.1.0/apache-servicecomb-incubating-saga-distribution-0.1.0-bin.zip.sha512)


**Verifying the release**

It is essential that you verify the integrity of the downloaded files using the PGP or SHA signatures.
 The PGP signatures can  be verified using GPG or PGP. 
 Please download the [KEYS](https://www.apache.org/dist/incubator/servicecomb/KEYS){:target="_blank"} as well as the asc signature files for relevant distribution. It is recommended to get these files from the main distribution [directory](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-saga/){:target="_blank"} and not from the mirrors.
 ```
 gpg -i KEYS
 
 or
 
 pgpk -a KEYS
 
 or
 
 pgp -ka KEYS

```

To verify the binaries/sources you can download the relevant asc files for it from main distribution directory and follow the below guide.

```
gpg --verify apache-servicecomb-incubating-saga********.asc apache-servicecomb-incubating-saga-*********

or

pgpv apache-servicecomb-incubating-saga-********.asc

or 

pgp apache-servicecomb-incubating-saga-********.asc


```

Alternatively you can download the SHA signatures from main distribution [repo](https://www.apache.org/dist/incubator/servicecomb/incubator-servicecomb-saga/){:target="_blank"} and verify the downloads using sha512sum.
