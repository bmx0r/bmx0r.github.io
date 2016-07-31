---
layout: post
title: OpenSSL generates different hash values for each OpenSSL version
---

## ISSUE : OpenSSL generates different hash values for each OpenSSL version

Recently while adding a root CA in /etc/openldap/cacerts under centos/rhel systems, the path where you can drop CAs and a link to them named with the hash.0
We discover that openssl generate different hashes depending of the version.

In practice what does it mean….
 
When you need to add a new CA in /etc/openldap/cacerts (the directory containing the CA to allow us to recognize the ldap’s server certificate)
We need two things :

* The Root CA
* A link who point to that root with a hashe.0 as name

 
Ie I add two CA in the directory:

* /etc/openldap/cacerts/rootCA.pem
* /etc/openldap/cacerts/server_ssl_subCA.pem

Now I have to generate the link :

* ln -s rootCA.pem `openssl x509 -hash -noout -in rootCA.pem`.0
* ln -s server_ssl_subCA.pem `openssl x509 -hash -noout -in server_ssl_subCA.pem`.0

 
The output on el 6 :

* 1c94294a.0 -> /etc/openldap/cacerts/rootCA.pem
* c14013e2.0 -> /etc/openldap/cacerts/server_ssl_subCA.pem

The output on el5:

* 8f257ea1.0 -> rootCA.pem
* 5f8afcd9.0 -> server_ssl_subCA.pem

So indeed depending on the openssl version hashes might get different à Think about it when you need to add a CA and a hashes somewhere (ie with apache, or when you use directive like CAPATH=)
