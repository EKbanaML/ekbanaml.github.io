# SASL and GSSAPI

SASL and GSSAPI are frameworks that various authentication providers can be plugged into. People wishing to use Kerberos authentication in an app that supports
SASL or GSSAPI need only to provide the appropriate Kerberos plugin, rather than rewrite the app with Kerberos-specific code.

SASL stands for Simple Authentication and Security Layer; it's a framework that allows developers to implement different authentication mechanisms, and allows 
clients and servers to negotiate a mutually acceptable mechanism for each connection (rather than hard-coding or pre-configuring them).

GSSAPI stands for Generic Security Services Application Program Interface; it is usually made available as one of the mechanisms that SASL can use. It is itself 
another framework for developing and implementing various authentication mechanisms. These mechanisms include Kerberos, NTLM, and SPNEGO 
(Simple and Protected GSSAPI Negotiation Mechanism): a GSSAPI pseudo-mechanism which allows GSSAPI-compatible clients to negotiate which GSSAPI mechanism they 
want to use.

Here's an example to help make this a little clearer (brutally simplified for clarity's sake):

- Client connects to server and says, "I support SASL! How should I authenticate myself?"
- Server receives the connection and responds, "I also support SASL, and can use these mechanisms, in descending order of preference: GSSAPI, CRAM-MD5, PLAIN."
- Client responds, "Of the choices, I'd like to use GSSAPI."
- Server responds "GSSAPI? Capital. I support Kerberos and NTLM."
- Client responds "Let's use Kerberos. Here's my encrypted ticket etc. etc."

For Installing Kerberos in Linux you can also refer to [kerberos-installation-guide](https://help.ubuntu.com/community/Kerberos)

## Server Side Installation

Add a domain name in your machine if not added.

```
Edit: /etc/hosts

ip-address          domain-name         host-name

10.10.10.251        nn1.ekbana.com      nn1
10.10.10.252        nn2.ekbana.com      nn2
10.10.10.253        dn1.ekbana.com      dn1
```

Command to install kerberos server

```
sudo apt install krb5-kdc krb5-admin-server
```

You will be asked at the end of the install to supply the hostname for the Kerberos and Admin servers, which may or may not be the same server, for the realm.

```
Realm: EKBANA.COM
Kerberos servers : nn1.ekbana.com
Admin servers: nn1.ekbana.com
```

You can create a database for kerberos by using following command

```
sudo krb5_newrealm
```

If you need to adjust the Key Distribution Center (KDC) settings simply edit the file and restart the krb5-kdc daemon. If you need to reconfigure Kerberos from 
scratch, perhaps to change the realm name, you can do so by typing:

```
sudo dpkg-reconfigure krb5-kdc
```

Now create an admin principal to get the ability to perform any operation on all principals in the realm.

```
sudo kadmin.local
kadmin.local: addprinc ekbana/admin (To add a principal)
kadmin.local: quit
```

The new admin user needs to have the appropriate Access Control List (ACL) permissions.

```
Edit: /etc/krb5kdc/kadm5.acl

ekbana/admin@EKBANA.COM
        or
*/admin@EKBANA.COM


Also edit krb5.conf file in /etc/krb5.conf [Add ekbana.com domain]
[domain_realm]
        .toronto.edu = UTORONTO.CA
        .utoronto.ca = UTORONTO.CA
        .ekbana.com = EKBANA.COM
         ekbana.com = EKBANA.COM
```

Now restart the krb5-admin-server for the new ACL to take effect

```
sudo systemctl restart krb5-admin-server.service
```

The new user principal can be tested using the kinit utility: 

```
kinit ekbana/admin
ekbana/admin@EKBANA.COM's Password: Enter the password
```

After entering the password, use the klist utility to view information about the Ticket Granting Ticket (TGT): 

```
Ticket cache: FILE:/tmp/krb5cc_1000
Default principal: ekbana/admin@EKBANA.COM

Valid starting			Expires              	       Service principal
2020-02-15T16:11:35		2020-02-16T02:11:35	       krbtgt/EKBANA.COM@EKBANA.COM
				
			renew until 2020-02-16T16:11:33
```

With kerberos authentication setup we need to edit SSH configuration to allow kerberos authentication to be used

```
Edit: /etc/ssh/sshd_config

# Kerberos options
KerberosAuthentication no
KerberosGetAFSToken no
KerberosOrLocalPasswd yes
KerberosTicketCleanup yes
```

## Client Side Installation

```
sudo apt install krb5-user libpam-krb5 libpam-ccreds auth-client-config
```

Configure Realm, Kerberos server and Admin server same as Server side Installation.
* Edit krb5.conf in /etc/krb5.conf file same as Server side Installation.

*[Note: You need to add the ip-address and domain names of all the client and server in each machines]*

Login to the admin principal created in Server side Installation. 

```
kinit ekbana/admin
password for ekbana@EKBANA.COM: Enter the password
```

* Check if you have a ticket or not using “klist” command. 

## Creating Keytabs

First Create principals for any service such as zookeeper.

```
sudo kadmin.local

kadmin: addprinc -randkey zookeeper/nn1.ekbana.com@EKBANA.COM
kadmin: addprinc -randkey zookeeper/nn2.ekbana.com@EKBANA.COM
kadmin: addprinc -randkey zookeeper/dn1.ekbana.com@EKBANA.COM
```

Now Create keytabs using the above principals.

```
kadmin: xst -norandkey -k /etc/security/keytabs/zookeeper.keytab zookeeper/nn1.ekbana.com zookeeper/nn2.ekbana.com zookeeper/dn1.ekbana.com
```

Change the keytabs to appropriate owner and group and permission as 444, you can refer to the above installation link for more details.

---

##### You can list the contents of keytabs using the command:

```
klist -e -t -k /etc/security/keytabs/zookeeper.keytab
```







