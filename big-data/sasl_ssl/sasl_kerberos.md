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

For Installing Kerberos in Linux you can refer to [kerberos-installation-guide](https://help.ubuntu.com/community/Kerberos)


