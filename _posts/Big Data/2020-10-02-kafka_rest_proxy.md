---
title: "Kafka Rest Proxy"
excerpt_separator: "<!--more-->"
last_modified_at: 2020-10-02T16:20:02-05:00
categories:
  - Big Data
tags:
  - Kafka
---

# Kafka Rest Proxy Security

The Kafka REST Proxy provides a RESTful interface to a Kafka cluster. It makes it easy to produce and consume messages, view the state of the cluster, and 
perform administrative actions without using the native Kafka protocol or clients. Examples of use cases include reporting data to Kafka from any frontend app 
built in any language, ingesting messages into a stream processing framework that doesn’t yet support Kafka, and scripting administrative actions.

## Create Keytabs for Kafka Rest Proxy

Refer [SASL_Kerberos]({% post_url /Big\ Data/2020-10-02-sasl_kerberos %}) for more details.

```
sudo kadmin.local

kadmin: addprinc -randkey rest@EKBANA.COM

kadmin: xst -norandkey -k /etc/security/keytabs/restProxy.keytab rest@EKBANA.COM
```

## Configure (kafka-rest.properties)

You can refer [SSL-Encryption]({% post_url /Big\ Data/2020-10-02-ssl-encryption %}) for generating certificates.

```
listeners=http://nn1.ekbana.com:8082

schema.registry.url=https://nn1.ekbana.com:8081
zookeeper.connect=10.10.5.20:2181,10.10.5.21:2181,10.10.5.22:2181
bootstrap.servers=SASL_SSL://nn1.ekbana.com:9092,SASL_SSL://nn2.ekbana.com:9092,SASL_SSL://dn1.ekbana.com:9092

# ssl security

ssl.truststore.location=/etc/security/ssl/restProxy.client.truststore.jks
ssl.truststore.password=Truststore-password
ssl.keystore.location=/etc/security/ssl/restProxy.client.keystore.jks
ssl.keystore.password=Keystore-password
ssl.key.password=Key-password

ssl.protocol=SSL
inter.instance.protocol=http
ssl.client.auth=true

sasl.mechanism=GSSAPI
sasl.kerberos.service.name=kafka
security.protocol=SASL_SSL

sasl.jaas.config=com.sun.security.auth.module.Krb5LoginModule required \
   useKeyTab=true \
   storeKey=true \
   keyTab="/etc/security/keytabs/restProxy.keytab" \
   principal="rest@EKBANA.COM";


# Rest Proxy Client

client.bootstrap.servers=SASL_SSL://nn1.ekbana.com:9092,SASL_SSL://nn2.ekbana.com:9092,SASL_SSL://dn1.ekbana.com:9092

client.ssl.protocol=SSL
client.ssl.truststore.location=/etc/security/ssl/restProxy.client.truststore.jks
client.ssl.truststore.password=Truststore-password
client.ssl.keystore.location=/etc/security/ssl/restProxy.client.keystore.jks
client.ssl.keystore.password=Keystore-password
client.ssl.key.password=Key-password

client.security.protocol=SASL_SSL
client.sasl.mechanism=GSSAPI
client.sasl.kerberos.service.name=kafka

client.sasl.jaas.config=com.sun.security.auth.module.Krb5LoginModule required \
   useKeyTab=true \
   storeKey=true \
   keyTab="/etc/security/keytabs/restProxy.keytab" \
   principal="rest@EKBANA.COM";
```

#### Start Kafka Rest Proxy

```
/usr/share/kafka-confluent/bin/kafka-rest-start /usr/share/kafka-confluent/etc/kafka-rest/kafka-rest.properties
```