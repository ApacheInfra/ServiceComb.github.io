---
title: "ServiceComb����ָ��"
lang: cn
ref: release_guide
permalink: /cn/developers/release-guide/
excerpt: "ServiceComb����ָ�� ���������Apache����"
last_modified_at:  2018-05-08T09:55:44+08:00
author: Asif Siddiqui
tags: [����]
redirect_from:
  - /theme-setup/
---

�������ҽ��������Apache�Ͻ���ServiceComb��Ŀ����.

## ǰ��׼��

1. ��ĿCIӦ���������ģ���ɫ�ģ���
2. ȷ����ص���Ŀ�汾�š�
3. ��Ϊ����Ĺ�������Ҫǩ������ȷ��ǩ������Կ��Ӧ��Կ�Ѿ�������������Կ��������
4. ��ϤPOM�ļ��а汾������ص����á�

## ����Maven
ServiceComb Java-Chassis��Sagaʹ��Maven���а汾������������Ҫ�ڷ���ǰ��Maven����һЩ���á�

��ʹ��Maven�ѷ��а��������ֿ�֮ǰ������Ӧ����`~/.m2/settings.xml`�ļ���������ƷΪ��Ⱥ��д�ģ���������������Ա���޷��ύ��ͬ��Ʒ����SNAPSHOT�汾������Ŀ�ο���Maven��Ŀ���趨[ָ��](http://maven.apache.org/developers/committer-settings.html)�����ر�ע��[��������](http://maven.apache.org/guides/mini/guide-encryption.html)��

```
<settings>
  ...
  <servers>
    <!-- Per http://maven.apache.org/developers/committer-settings.html -->

    <!-- To publish a snapshot of some part of Maven -->
    <server>
      <id>apache.snapshots.https</id>
      <username> <!-- YOUR APACHE LDAP USERNAME --> </username>
      <password> <!-- YOUR APACHE LDAP PASSWORD --> </password>
    </server>
    <!-- To publish a website of some part of Maven -->
    <server>
      <id>apache.website</id>
      <username> <!-- YOUR APACHE LDAP USERNAME --> </username>
      <filePermissions>664</filePermissions>
      <directoryPermissions>775</directoryPermissions>
    </server>
    <!-- To stage a release of some part of Maven -->
    <server>
      <id>apache.releases.https</id>
      <username> <!-- YOUR APACHE LDAP USERNAME --> </username>
      <password> <!-- YOUR APACHE LDAP PASSWORD --> </password>
    </server>
    <!-- To stage a website of some part of Maven -->
    <server>
      <id>stagingSite</id> <!-- must match hard-coded repository identifier in site:stage-deploy -->
      <username> <!-- YOUR APACHE LDAP USERNAME --> </username>
      <filePermissions>664</filePermissions>
      <directoryPermissions>775</directoryPermissions>
    </server>

  </servers>
  ...
  <profiles>
    <profile>
      <id>apache-release</id>
      <properties>
        <gpg.useagent>false</gpg.useagent>
        <gpg.passphrase><!-- YOUR GPG PASSPHRASE --></gpg.passphrase>
        <test>false</test>
      </properties>
    </profile>

  </profiles>
...
</settings>
```

## ����Service-Center

***׼����У�鷢�а�***

1. ��¡service-center���롣
```
git@github.com:apache/incubator-servicecomb-service-center.git
cd incubator-servicecomb-service-center
gvt restore
```

2. ��master��֧�ϴ���׼�������汾�ı�ǩ��

3. ����RAT���ߣ��������Դ�ļ�ͷ���кϷ���ASF����, ��ο�[���ĵ�](https://github.com/apache/incubator-servicecomb-service-center/tree/master/docs/release)��

4. ����`make_release.sh`�ű�����ο�[���ĵ�](https://github.com/apache/incubator-servicecomb-service-center/tree/master/scripts/release)��

5. ��һ�������ڸ�Ŀ¼�����ɷ��а���

6. ��Linux��Windows����������ǰ����service-center��

7. ����[���ɲ���](https://github.com/apache/incubator-servicecomb-service-center/tree/master/integration)��

8. �������ȫ�����Զ�ͨ���ˣ������а��ַ���ͬ���ڲ�ͬ�����Ͻ�����֤��

9. ����ǩ���͵����ֿ⡣

***�����а�ǩ��***

10. ��Github����Ҫ���а汾[��ǩ](https://github.com/apache/incubator-servicecomb-service-center/tags)��Դ�����

11. ����Linux���а���Windows���а���Դ�����ǩ����У��͡�

12. �ϴ����а浽[Apache���п����ֿ�](https://dist.apache.org/repos/dist/dev/incubator/servicecomb/incubator-servicecomb-service-center/).

13. ��SVN���ط��а�����֤ǩ����У�顣

***PPMC��׼***

14. ����ͶƱ�ʼ��� ***dev@servicecomb.apache.org***�� ����PPMC��׼.

15. �ȴ�72Сʱ�����߻��3Ʊ+1����û��-1�������-1Ʊ���������Ⲣ��***��1��***���¿�ʼ��

16. ��ͶƱ���������dev@servicecomb.apache.org��

***IPMC��׼***

17. ����ͶƱ�ʼ���***general@incubator.apache.org***������IPMC��׼��

18. �ȴ�72Сʱ�����߻��3Ʊ+1����û��-1�������-1Ʊ���������Ⲣ��***��1��***���¿�ʼ��

19. ��ͶƱ���������general@incubator.apache.org��

***ͨ��***

20. �ϴ����а���[Apache���вֿ�](https://dist.apache.org/repos/dist/release/incubator/servicecomb/incubator-servicecomb-service-center/)��

21. �ȴ�24Сʱ�������о���ͬ����

22. �ϴ�����ҳ����ServiceComb��վ��

23. ���ͷ���ͨ���ʼ���dev@servicecomb.apache.org�� general@incubator.apache.org�� announce@apache.org��




## ����Java-Chassis

***׼����У�鷢�а�***

1. ��¡java-chassis���롣
```
git clone git@github.com:apache/incubator-servicecomb-java-chassis.git
```

2. ʹ������perl����滻����pom.xml�ļ��еİ汾�Ų��ύ�Ķ������ء�
```
find . -name 'pom.xml'|xargs perl -pi -e 's/1.0.0-m2-SNAPSHOT/1.0.0-m2/g'
```

3. ��master��֧�ϴ���׼�������汾�ı�ǩ��

4. ����repository.apache.org����������ķ��а档

5. ��GPG��Կ�ļ��������ļ��б��á�

6. ����`~/.m2/settings.xml`�ļ��е�GPG��Կ�ļ�·��������.

7. ����������Apache�ʻ��û��������롣

8. �����������
```
mvn deploy -DskipTests -Prelease -Pdistribution -Ppassphrase
```

9. ��������ִ�гɹ������е�jar�����ɹ��ϴ�����ʱ�ֿ������Company Workshop�������Ĺ�����֤��

10. ����ʱ�ֿ⹲������ˣ��ڲ�ͬ�Ļ����ͻ����Ͻ�����֤��

11. �����֤ȫ��ͨ��������ǩ�ύ�����ֿ⡣

12. ����Apache��ʱ�ֿ⡣

***�����а�ǩ��***

13. ����ʱ�ֿ����ض����ư���Դ�����

14. ���ɶ����ư���Դ�����ǩ����У��͡�

15. �ϴ����а���[Apache���п����ֿ�](https://dist.apache.org/repos/dist/dev/incubator/servicecomb/incubator-servicecomb-java-chassis/).
.

16. ��SVN���ط��а�����֤ǩ����У�顣

***PPMC��׼***

17. ����ͶƱ�ʼ��� ***dev@servicecomb.apache.org***�� ����PPMC��׼.

18. �ȴ�72Сʱ�����߻��3Ʊ+1����û��-1�������-1Ʊ���������Ⲣ��***��1��***���¿�ʼ��

19. ��ͶƱ���������dev@servicecomb.apache.org��

***IPMC��׼***

20. ����ͶƱ�ʼ���***general@incubator.apache.org***������IPMC��׼��

21. �ȴ�72Сʱ�����߻��3Ʊ+1����û��-1�������-1Ʊ���������Ⲣ��***��1��***���¿�ʼ��

22. ��ͶƱ���������general@incubator.apache.org��

***ͨ��***

23. �ϴ����а���[Apache���вֿ�](https://dist.apache.org/repos/dist/release/incubator/servicecomb/incubator-servicecomb-java-chassis/)��

24. �ȴ�24Сʱ�������о���ͬ����

25. �ϴ�����ҳ����ServiceComb��վ��

26. ���ͷ���ͨ���ʼ���dev@servicecomb.apache.org�� general@incubator.apache.org�� announce@apache.org��




## ����Saga

***׼����У�鷢�а�***

1. ��¡Saga���롣
```
git@github.com:apache/incubator-servicecomb-saga.git
```

2. ʹ������perl����滻����pom.xml�ļ��еİ汾�Ų��ύ�Ķ������ء�
```
find . -name 'pom.xml'|xargs perl -pi -e 's/1.0.0-m2-SNAPSHOT/1.0.0-m2/g'
```

3. ��master��֧�ϴ���׼�������汾�ı�ǩ��

4. ����repository.apache.org����������ķ��а档

5. ��GPG��Կ�ļ��������ļ��б��á�

6. ����`~/.m2/settings.xml`�ļ��е�GPG��Կ�ļ�·��������.

7. ����������Apache�ʻ��û��������롣

8. �����������
```
mvn deploy -DskipTests -Prelease -Pdistribution -Ppassphrase
```

9. ��������ִ�гɹ������е�jar�����ɹ��ϴ�����ʱ�ֿ�������ż���������֤�������ܡ�

10. ����ʱ�ֿ⹲������ˣ��ڲ�ͬ�Ļ����ͻ����Ͻ�����֤��

11. �����֤ȫ��ͨ��������ǩ�ύ�����ֿ⡣

12. ����Apache��ʱ�ֿ⡣

***�����а�ǩ��***

13. ����ʱ�ֿ����ض����ư���Դ�����

14. ���ɶ����ư���Դ�����ǩ����У��͡�

15. �ϴ����а���[Apache���п����ֿ�](https://dist.apache.org/repos/dist/dev/incubator/servicecomb/incubator-servicecomb-java-chassis/).
.

16. ��SVN���ط��а�����֤ǩ����У�顣

***PPMC��׼***

17. ����ͶƱ�ʼ��� ***dev@servicecomb.apache.org***�� ����PPMC��׼.

18. �ȴ�72Сʱ�����߻��3Ʊ+1����û��-1�������-1Ʊ���������Ⲣ��***��1��***���¿�ʼ��

19. ��ͶƱ���������dev@servicecomb.apache.org��

***IPMC��׼***

20. ����ͶƱ�ʼ���***general@incubator.apache.org***������IPMC��׼��

21. �ȴ�72Сʱ�����߻��3Ʊ+1����û��-1�������-1Ʊ���������Ⲣ��***��1��***���¿�ʼ��

22. ��ͶƱ���������general@incubator.apache.org��

***ͨ��***

23. �ϴ����а���[Apache���вֿ�](https://dist.apache.org/repos/dist/release/incubator/servicecomb/incubator-servicecomb-saga/)��

24. �ȴ�24Сʱ�������о���ͬ����

25. �ϴ�����ҳ����ServiceComb��վ��

26. ���ͷ���ͨ���ʼ���dev@servicecomb.apache.org�� general@incubator.apache.org�� announce@apache.org��


**ע��**
�������й���ͨ����Ҫ2��ʱ�䣬���PPMCͶƱ��IPMCȫ����һ����ͨ�����������ǰ׼�����л��
