# Zookeeper Security

The metadata stored in ZooKeeper is such that only brokers will be able to modify the corresponding znodes, but znodes are world readable. 

The rationale behind this decision is that the data stored in ZooKeeper is not sensitive, but inappropriate manipulation of znodes can cause cluster disruption. 
We also recommend limiting the access to ZooKeeper via network segmentation (only brokers and some admin tools need access to ZooKeeper if the new consumer and 
new producer are used).

## Create keytabs for Zookeeper

Refer [SASL_Kerberos](../sasl_ssl/sasl_kerberos.md) for more details.

```
sudo kadmin.local

kadmin: addprinc -randkey zookeeper/nn1.ekbana.com@EKBANA.COM
kadmin: addprinc -randkey zookeeper/nn2.ekbana.com@EKBANA.COM
kadmin: addprinc -randkey zookeeper/dn1.ekbana.com@EKBANA.COM

kadmin: xst -norandkey -k /etc/security/keytabs/zookeeper.keytab zookeeper/nn1.ekbana.com zookeeper/nn2.ekbana.com zookeeper/dn1.ekbana.com
```

## Create a jaas file for Authentication

###### Zookeeper Server for node: nn1 (zkServer.conf)

```
Server {
	com.sun.security.auth.module.Krb5LoginModule required
	useKeyTab=true
	useTicketCache=false
	storeKey=true
	keyTab="/etc/security/keytabs/zookeeper.keytab"
	principal="zookeeper/nn1.ekbana.com@EKBANA.COM";
};
```

###### Zookeeper Server for node: dn1 (zkServer.conf)

```
// Zookeeper server authentication
Server {
	com.sun.security.auth.module.Krb5LoginModule required
	useKeyTab=true
	useTicketCache=false
	storeKey=true
	keyTab="/etc/security/keytabs/zookeeper.keytab"
	principal="zookeeper/dn1.ekbana.com@EKBANA.COM";
};
```

###### Zookeeper client for node: nn1 (client.conf)

It will be used to connect kafka brokers

```
// ZooKeeper client authentication
Client {
    com.sun.security.auth.module.Krb5LoginModule required
    useKeyTab=true
    storeKey=true
    keyTab="/etc/security/keytabs/kafka.keytab"
    principal="kafka/nn1.ekbana.com@EKBANA.COM";
};
```

###### Zookeeper client for node: dn1 (client.conf)

It will be used to connect kafka brokers

```
// ZooKeeper client authentication
Client {
    com.sun.security.auth.module.Krb5LoginModule required
    useKeyTab=true
    storeKey=true
    keyTab="/etc/security/keytabs/kafka.keytab"
    principal="kafka/dn1.ekbana.com@EKBANA.COM";
};
```

## Configure (zoo.cfg)

```
authProvider.1=org.apache.zookeeper.server.auth.SASLAuthenticationProvider
jaasLoginRenew=3600000
kerberos.removeHostFromPrincipal=true
kerberos.removeRealmFromPrincipal=true
```
## Configure (java.env)

```
SERVER_JVMFLAGS="-Djava.security.auth.login.config=/etc/security/conf/zkServer.conf"
CLIENT_JVMFLAGS="-Djava.security.auth.login.config=/etc/security/conf/client.conf"
```

#### To start Zookeeper

```
/usr/share/zookeeper/bin/zkServer.sh start
```