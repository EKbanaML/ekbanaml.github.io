# Apache Kafka Security

Apache Kafka is an internal middle layer enabling your back-end systems to share real-time data feeds with each other through Kafka topics. With a standard 
Kafka setup, any user or application can write any messages to any topic, as well as read data from any topics.

### Kafka Security has three components:

- **Encryption of data in-flight using SSL / TLS:** This allows your data to be encrypted between your producers and Kafka and your consumers and Kafka. This is 
a very common pattern everyone has used when going on the web. That’s the “S” of HTTPS (that beautiful green lock you see everywhere on the web).
- **Authentication using SSL or SASL:** This allows your producers and your consumers to authenticate to your Kafka cluster, which verifies their identity. 
It’s also a secure way to enable your clients to endorse an identity. Why would you want that? Well, for authorization!
- **Authorization using ACLs:** Once your clients are authenticated, your Kafka brokers can run them against access control lists (ACL) to determine whether or 
not a particular client would be authorised to write or read to some topic.

---

Before Enabling Security in Kafka make sure you have setup the followings:

* [SSL](../sasl_ssl/ssl-encryption.md)
* [SASL_Kerberos](../sasl_ssl/sasl_kerberos.md)

---

Now, you can refer to the following documentation for enabling security in kafka cluster.

* [Kafka Server](kafka_server.md)
* [Kafka Schema Registry](kafka_schema_registry.md)
* [Kafka Connect](kafka_connect.md)
* [Kafka Rest Proxy](kafka_rest_proxy.md)
* [Kafka KSQL](kafka_ksql.md)
