# Kafka clients

The configs for SSL will be the same for both producer and consumer.

## Create Keytabs for Kafka Client

Refer [SASL_Kerberos](../sasl_ssl/sasl_kerberos.md) for more details.

```
sudo kadmin.local

kadmin: addprinc -randkey kafkaclient@EKBANA.COM

kadmin: xst -norandkey -k /etc/security/keytabs/kafkaclient.keytab kafkaclient@EKBANA.COM

```

## Configure (client-ssl.properties)

You can refer [SSL-Encryption](../sasl_ssl/ssl-encryption.md) for generating certificates.

```
ssl.truststore.location=/etc/security/ssl/kafka.client.truststore.jks
ssl.truststore.password=Truststore-password
ssl.keystore.location=/etc/security/ssl/kafka.client.keystore.jks
ssl.keystore.password=Keystore-password
ssl.key.password=Key-password

sasl.mechanism=GSSAPI
security.protocol=SASL_SSL
sasl.kerberos.service.name=kafka

sasl.jaas.config=com.sun.security.auth.module.Krb5LoginModule required \
    useKeyTab=true \
    storeKey=true \
    keyTab="/etc/security/keytabs/kafkaclient.keytab" \
    principal="kafkaclient/@EKBANA.COM";
```

## Console Producer/Consumer

```
bin/kafka-console-producer --broker-list localhost:9092 --topic test --producer.config client-ssl.properties
bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic test --new-consumer --consumer.config client-ssl.properties
```

## Java Producer/Consumer

```
Properties properties = new Properties();

properties.setProperty(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SASL_SSL");
properties.setProperty(SslConfigs.SSL_TRUSTSTORE_LOCATION_CONFIG, "/etc/security/ssl/kafka.client.truststore.jks");
properties.setProperty(SslConfigs.SSL_TRUSTSTORE_PASSWORD_CONFIG, "Truststore-password");
properties.setProperty(SslConfigs.SSL_KEYSTORE_LOCATION_CONFIG, "/etc/security/ssl/kafka.client.keystore.jks");
properties.setProperty(SslConfigs.SSL_KEYSTORE_PASSWORD_CONFIG, "Keystore-password");
properties.setProperty(SslConfigs.SSL_KEY_PASSWORD_CONFIG, "Key-password");

properties.setProperty(SaslConfigs.SASL_KERBEROS_SERVICE_NAME, "kafka");
properties.setProperty(SaslConfigs.SASL_JAAS_CONFIG, "com.sun.security.auth.module.Krb5LoginModule required " +
        "useKeyTab=true " +
        "storeKey=true " +
        "keyTab=\"/etc/security/keytabs/kafkaclient.keytab\" " +
        "principal=\"kafkaclient@EKBANA.COM\";");
```