# Kafka Schema Registry Security

Schema Registry provides a serving layer for your metadata. It provides a RESTful interface for storing and retrieving Avro schemas. It stores a versioned 
history of all schemas, provides multiple compatibility settings and allows evolution of schemas according to the configured compatibility setting. It provides 
serializers that plug into Kafka clients that handle schema storage and retrieval for Kafka messages that are sent in the Avro format.

## Create Keytabs for Schema Registry

Refer [SASL_Kerberos](../sasl_ssl/sasl_kerberos.md) for more details.

```
sudo kadmin.local

kadmin: addprinc -randkey schemaregistry@EKBANA.COM

kadmin: xst -norandkey -k /etc/security/keytabs/schemaRegistry.keytab schemaregistry@EKBANA.COM
```

## Create jaas file for Schema Registry

###### Create (schemaRegistry.conf)

```
// SchemaRegistry client authentication
KafkaClient {
    com.sun.security.auth.module.Krb5LoginModule required
    useKeyTab=true
    storeKey=true
    keyTab="/etc/security/keytabs/schemaRegistry.keytab"
    principal="schemaregistry@EKBANA.COM";
};
```

## Configure (schema-registry.properties)

You can refer [SSL-Encryption](../sasl_ssl/ssl-encryption.md) for generating certificates.

```
listeners=https://nn1.ekbana.com:8081
kafkastore.bootstrap.servers=SASL_SSL://nn1.ekbana.com:9092,SASL_SSL://nn2.ekbana.com:9092,SASL_SSL://dn1.ekbana.com:9092

kafkastore.ssl.truststore.location=/etc/security/ssl/schemaRegistry.client.truststore.jks
kafkastore.ssl.truststore.password=Truststore-password
kafkastore.ssl.keystore.location=/etc/security/ssl/schemaRegistry.client.keystore.jks
kafkastore.ssl.keystore.password=Keystore-password
kafkastore.ssl.key.password=Key-password

inter.instance.protocol=https
ssl.client.auth=true

kafkastore.sasl.mechanism=GSSAPI
kafkastore.sasl.kerberos.service.name=kafka
kafkastore.security.protocol=SASL_SSL
```

Start by running the Schema Registry and the services it depends on:

- [ZooKeeper](zookeeper.md#to-start-zookeeper) 
- [Kafka](kafka_server.md#to-start-kafka-server)

#### Start Schema Registry

```
export SCHEMA_REGISTRY_OPTS=-Djava.security.auth.login.config=/etc/security/conf/schemaRegistry.conf
/usr/share/kafka-confluent/bin/schema-registry-start /usr/share/kafka-confluent/etc/schema-registry/schema-registry.properties
```
