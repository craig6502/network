Java Code setup BEFORE SSL socket connection must cover :

This code decouples the implementation of the SSL protocol from:

    1. the source of your key material (KeyStore)
    2. certificate algorithm choice and key management (KeyManager)
    3. management of peer trust rules (TrustManager) - not used here
    4. secure random algorithm (SecureRandom)
    5. NIO or socket implementation (SSLServerSocketFactory) - could use SSLEngine for NIO

In other words, you set the JSSE system properties first, so that you can call the relevant Java commands that rely upon them.

Is the trust manager essential?
see here:
https://access.redhat.com/documentation/en-US/Fuse_MQ_Enterprise/7.1/html/Security_Guide/files/SSL-SysProps.html

If not specified it will be looked for in some general locations - see below). Better to set it up locally.

    `$JAVA_HOME/lib/security/jssecacerts`

    `$JAVA_HOME/lib/security/cacerts`


Are all of these essential?

Clients communicating with server (implementation)

Algorithm:
 1. Determine the SSL Server Name and port in which the SSL server is listening
 2. Register the JSSE provider
 3. Create an instance of SSLSocketFactory
 4. Create an instance of SSLSocket
 5. Create an OutputStream object to write to the SSL Server
 6. Create an InputStream object to receive messages back from the SSL Server
 
 For certificate creation this is a good reference:
 
 `https://www.sslshopper.com/article-most-common-java-keytool-keystore-commands.html`
 
 and
 
 `https://access.redhat.com/documentation/en-US/Fuse_MQ_Enterprise/7.1/html/Security_Guide/files/SSL-SysProps.html`
 
 For Java client security see
 
 `https://access.redhat.com/documentation/en-US/Fuse_MQ_Enterprise/7.1/html/Security_Guide/files/FMQSecuritySSLClients.html`
