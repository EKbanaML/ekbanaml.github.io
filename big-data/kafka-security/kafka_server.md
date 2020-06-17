# Kafka Server Security

## Configuring Kafka Brokers

Kafka Brokers support listening for connections on multiple ports. We need to configure `listeners` and optionally `advertised.listeners` in `server.properties`, 
each of which contains one or more comma-separated values.

Before Configuring Brokers, make sure you have configured [Zookeeper](zookeeper.md) for authenticating Brokers.

## Enabling SSL Logging

You can enable SSL debug logging at the JVM level by starting the Kafka broker and/or clients with the javax.net.debug system property. For example:

```
-Djavax.net.debug=all
```

### Config (server.properties)

###### For node: nn1

```
listeners=SASL_SSL://nn1.ekbana.com:9092
advertised.listeners=SASL_SSL://nn1.ekbana.com:9092
```

###### For node: dn1

```
listeners=SASL_SSL://dn1.ekbana.com:9092
advertised.listeners=SASL_SSL://dn1.ekbana.com:9092
```

###### For each node

You can refer [SSL-Encryption](../sasl_ssl/ssl-encryption.md) for generating certificates.

```
ssl.truststore.location=/etc/security/ssl/kafka.server.truststore.jks
ssl.truststore.password=Truststore-password
ssl.keystore.location=/etc/security/ssl/kafka.server.keystore.jks
ssl.keystore.password=Keystore-password
ssl.key.password=Key-password

ssl.protocol=SSL
security.inter.broker.protocol=SSL
ssl.client.auth=required

security.protocol=SASL_SSL
security.inter.broker.protocol=SASL_SSL
sasl.enabled.mechanisms=GSSAPI
sasl.mechanism.inter.broker.protocol=GSSAPI
sasl.kerberos.service.name=kafka
```
## Create Keytabs for Kafka Server/Client

Refer [SASL_Kerberos](../sasl_ssl/sasl_kerberos.md) for more details.

```
sudo kadmin.local

kadmin: addprinc -randkey kafka/nn1.ekbana.com@EKBANA.COM
kadmin: addprinc -randkey kafka/nn2.ekbana.com@EKBANA.COM
kadmin: addprinc -randkey kafka/dn1.ekbana.com@EKBANA.COM

kadmin: xst -norandkey -k /etc/security/keytabs/kafka.keytab kafka/nn1.ekbana.com kafka/nn2.ekbana.com kafka/dn1.ekbana.com
```

## Create a jaas file for Authentication

###### For node: nn1 (kServer.conf)

```
// Unique keytab and principal name for broker
KafkaServer {
    com.sun.security.auth.module.Krb5LoginModule required
    useKeyTab=true
    storeKey=true
    keyTab="/etc/security/keytabs/kafka.keytab"
    principal="kafka/nn1.ekbana.com@EKBANA.COM";
};
```

###### For node: dn1 (kServer.conf)

```
// Unique keytab and principal name for broker
KafkaServer {
    com.sun.security.auth.module.Krb5LoginModule required
    useKeyTab=true
    storeKey=true
    keyTab="/etc/security/keytabs/kafka.keytab"
    principal="kafka/dn1.ekbana.com@EKBANA.COM";
};
```

To start Kafka Server:

```
export KAFKA_OPTS=-Djava.security.auth.login.config=/etc/security/conf/kServer.conf
/usr/share/kafka-confluent/bin/kafka-server-start /usr/share/kafka-confluent/etc/kafka/server.properties
```
