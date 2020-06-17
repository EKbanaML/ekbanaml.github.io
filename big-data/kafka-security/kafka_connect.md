# Kafka Connect Security


## Create Keytabs for Kafka Connect

Refer [SASL_Kerberos](../sasl_ssl/sasl_kerberos.md) for more details.

```
sudo kadmin.local

kadmin: addprinc -randkey connect@EKBANA.COM

kadmin: xst -norandkey -k /etc/security/keytabs/connect.keytab connect@EKBANA.COM
```

## Configure (connect-distributed.properties)

You can refer [SSL-Encryption](../sasl_ssl/ssl-encryption.md) for generating certificates.

```
listeners=https://nn1.ekbana.com:8083
bootstrap.servers=SASL_SSL://nn1.ekbana.com:9092,SASL_SSL://nn2.ekbana.com:9092,SASL_SSL://dn1.ekbana.com:9092

rest.host.name=https://nn1.ekbana.com
rest.port=8083

rest.advertised.host.name=nn1.ekbana.com
rest.advertised.listeners=https

ssl.truststore.location=/etc/security/ssl/connect.client.truststore.jks
ssl.truststore.password=Truststore-password
ssl.keystore.location=/etc/security/ssl/connect.client.keystore.jks
ssl.keystore.password=Keystore-password
ssl.key.password=Key-password

inter.instance.protocol=https
ssl.client.auth=true

sasl.mechanism=GSSAPI
sasl.kerberos.service.name=kafka
security.protocol=SASL_SSL

sasl.jaas.config=com.sun.security.auth.module.Krb5LoginModule required \
   useKeyTab=true \
   storeKey=true \
   keyTab="/etc/security/keytabs/connect.keytab" \
   principal="connect@EKBANA.COM";
```

#### For Producer

```
producer.bootstrap.servers=SASL_SSL://nn1.ekbana.com:9092,SASL_SSL://nn2.ekbana.com:9092,SASL_SSL://dn1.ekbana.com:9092
producer.security.protocol=SASL_SSL

producer.ssl.truststore.location=/etc/security/ssl/connect.client.truststore.jks
producer.ssl.truststore.password=Truststore-password
producer.ssl.keystore.location=/etc/security/ssl/connect.client.keystore.jks
producer.ssl.keystore.password=Keystore-password
producer.ssl.key.password=Key-password

producer.sasl.mechanism=GSSAPI
producer.sasl.kerberos.service.name=kafka
producer.security.protocol=SASL_SSL

producer.sasl.jaas.config=com.sun.security.auth.module.Krb5LoginModule required \
   useKeyTab=true \
   storeKey=true \
   keyTab="/etc/security/keytabs/connect.keytab" \
   principal="connect@EKBANA.COM";
```

#### For Consumer

```
consumer.bootstrap.servers=SASL_SSL://nn1.ekbana.com:9092,SASL_SSL://nn2.ekbana.com:9092,SASL_SSL://dn1.ekbana.com:9092
consumer.security.protocol=SASL_SSL

consumer.ssl.truststore.location=/etc/security/ssl/connect.client.truststore.jks
consumer.ssl.truststore.password=Truststore-password
consumer.ssl.keystore.location=/etc/security/ssl/connect.client.keystore.jks
consumer.ssl.keystore.password=Keystore-password
consumer.ssl.key.password=Key-password

consumer.sasl.mechanism=GSSAPI
consumer.sasl.kerberos.service.name=kafka
consumer.security.protocol=SASL_SSL

consumer.sasl.jaas.config=com.sun.security.auth.module.Krb5LoginModule required \
   useKeyTab=true \
   storeKey=true \
   keyTab="/etc/security/keytabs/connect.keytab" \
   principal="connect@EKBANA.COM";
```

#### Start Kafka Connect

```
/usr/share/kafka-confluent/bin/connect-distributed /usr/share/kafka-confluent/etc/kafka/connect-distributed.properties
```
