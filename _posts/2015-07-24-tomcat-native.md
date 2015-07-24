---
layout: post
title: ISSUE : Tomcat 7.0.59-1 require a new version of tomcat-native on centos6
---

## ISSUE : Tomcat 7.0.59-1 require a new version of tomcat-native on centos 6

After a minor upgrade of tomcat 7.0.54 to 7.0.59 we run in the following message:

```
SEVERE: An incompatible version 1.1.30 of the APR based Apache Tomcat Native library is installed, while Tomcat requires version 1.1.32
```

This is due to the fact that our tomcat setup is using the tomcat native.
After looking at the changelog of tomcat, it seems to be there since Tomcat 7.0.57. 
Unfortunately we didn't saw it directly, since tomcat start anyway (but has no listening socket).
Unfortunately centos6 epel only provides tomcat-native-1.1.30-1.el6.x86_64
So no toher option than rebuilding the rpm for centos6.

## Rebuild the RPM from the src rpm of fedora
 
We download the source rpm of fedora 23 at : https://kojipkgs.fedoraproject.org//packages/tomcat-native/1.1.33/2.fc23/src/tomcat-native-1.1.33-2.fc23.src.rpm
We install it on a centos6 X86_64 server
The spec file has some dependencies:

```
yum install -y java-devel jpackage-utils apr-devel openssl-devel
```

Next run

```
cd /root/rpmbuild/SPECS
rpmbuild tomcat-native.spec
```

Push the rpm in your local repo --> update and enjoy
