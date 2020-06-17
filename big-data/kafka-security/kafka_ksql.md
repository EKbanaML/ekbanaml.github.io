# Kafka KSQL Security

Kafka enables companies to transform their business with event-driven architectures. They can deploy real-time data pipelines that organize all of an 
enterprise’s data around a single source of truth, and then use stream processing to enable new business opportunities and new methods of real-time analysis and 
decision-making. KSQL is the streaming SQL engine for Apache Kafka that makes it very easy to read, write, and process streaming data in real time, at scale, 
using a SQL-like syntax. There’s no need to write any code in a programming language like Java or Scala.

## Create Keytabs for KSQL

Refer [SASL_Kerberos](../sasl_ssl/sasl_kerberos.md) for more details.

```
sudo kadmin.local

kadmin: addprinc -randkey ksql@EKBANA.COM

kadmin: xst -norandkey -k /etc/security/keytabs/ksql.keytab ksql@EKBANA.COM
```

## Configure (ksql-server.properties)

You can refer [SSL-Encryption](../sasl_ssl/ssl-encryption.md) for generating certificates.

```
listeners=http://localhost:8088
bootstrap.servers=nn1.ekbana.com:9092,nn2.ekbana.com:9092,dn1.ekbana.com:9092
ksql.schema.registry.url=https://nn1.ekbana.com:8081

ssl.truststore.location=/etc/security/ssl/ksql.client.truststore.jks
ssl.truststore.password=Truststore-password
ssl.keystore.location=/etc/security/ssl/ksql.client.keystore.jks
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
   keyTab="/etc/security/keytabs/ksql.keytab" \
   principal="ksql@EKBANA.COM";
```

#### Start KSQL

```
/usr/share/kafka-confluent/bin/ksql
```