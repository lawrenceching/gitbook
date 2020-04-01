---
description: >-
  Enable mutual authentication/mTLS in Jetty server, Java mTLS, Jetty client
  authentication
---

# How to enable mutual authentication in Jetty server

mutual authentication is a common way to authentication your client or integrate with upstream/downstream services. This post will demostrate how to enable mutual authentication, or so-call mTLS in SSL, in Jetty server.

The overall source code is stored at [https://github.com/lawrenceching/jetty-mtls-example](https://github.com/lawrenceching/jetty-mtls-example).

### Prepare Certificates

{% hint style="info" %}
Change the password "changeit" in below commands if you generate a real certificate in production.
{% endhint %}

Run below commans to generate server and client certificates. It's notable that when terminal prompts your to input your first name and last name, input your domain name or ip address. It's mandatory that your hostname match your certificate.

I assume that you generate certs in `src/main/resources` where we normally do.

```text
# Generate server keystore
keytool -genkey -alias server -keyalg RSA -keypass changeit -storepass changeit -keystore server.jks

# Export server certificate from server keystore and import it into truststore.
# Client need it to validate server certificate in mTLS.
keytool -export -alias server -storepass changeit -file server.cer -keystore server.jks
keytool -import -file ./server.cer -alias server -keystore client_truststore.jks

# ---

# Generate client keystore
keytool -genkey -alias client -keyalg RSA -keypass changeit -storepass changeit -keystore client.jks

# Export client certificate and import in into truststore.
# Server need it to validate client certificate in mTLS.
keytool -export -alias client -storepass changeit -file client.cer -keystore client.jks
keytool -import -file ./client.cer -alias client -keystore server_truststore.jks

# You you can convert them to PKCS12 or PEM format
keytool -importkeystore -srckeystore client.jks -destkeystore client.p12 -srcalias client -srcstoretype jks -deststoretype pkcs12
openssl pkcs12 -in client.p12 -nokeys -out client.crt
openssl pkcs12 -in client.p12 -nocerts -nodes -out client.key


```

### Configurate Client Certificate in Jetty Server

The key to Jetty is to configurate the SslContextFactory.  Things are pretty straightforward. You set the KeyStore and KeyStore password, and then set the TrustStore and TrustStore password. You need to call `setNeedClientAuth(true)` to enable mTLS.

```java
// https://github.com/lawrenceching/jetty-mtls-example/blob/master/src/main/java/me/imlc/JettyMtlsServer.java#L25-L32
SslContextFactory.Server sslContextFactory = new SslContextFactory.Server();
sslContextFactory.setKeyStorePath(this.getClass().getResource(
        "/server.jks").toExternalForm());
sslContextFactory.setKeyStorePassword("changeit");

sslContextFactory.setTrustStorePath(this.getClass().getResource("/server_truststore.jks").toExternalForm());
sslContextFactory.setTrustStorePassword("changeit");
sslContextFactory.setNeedClientAuth(true);
```

### Verification

#### CURL

We usually use `curl` to be a test client. However `curl` doesn't know .jks file. So we need some convertion.

```text
# Convert KeyStore to pkcs12 format
keytool -importkeystore -srckeystore client.jks -destkeystore client.p12 -srcalias client -srcstoretype jks -deststoretype pkcs12

# Convert private key and certificate to .key and .crt in PEM format
openssl pkcs12 -in client.p12 -nokeys -out client.crt
openssl pkcs12 -in client.p12 -nocerts -nodes -out client.key

# curl it!
curl -v -k https://localhost:8443 \
  --cert ./client.crt \
  --key ./client.key
  
# Or
curl -v -k --cert-type=P12 --cert ./client.p12 \
   https://localhost:8443
```

#### Java HTTP Client

Or you may want to call mTLS server in Java client. Here is an example using Jetty http client. You can find similar configuration in OkHttp or Apacha Http Client.

```text
https://github.com/lawrenceching/jetty-mtls-example/blob/master/src/test/java/me/imlc/JettyMtlsServerTest.java#L26-L46
@Test
public void canAuthenticateClient() throws Exception {
        SslContextFactory sslContextFactory = new SslContextFactory.Client();
        sslContextFactory.setKeyStorePath(this.getClass().getResource(
                "/client.jks").toExternalForm());
        sslContextFactory.setKeyStorePassword("changeit");
        
        sslContextFactory.setTrustAll(true);
        
        sslContextFactory.setTrustStorePath(this.getClass().getResource(
                "/client_truststore.jks").toExternalForm());
        sslContextFactory.setTrustStorePassword("changeit");
        
        HttpClient httpClient = new HttpClient(sslContextFactory);
        
        
        httpClient.start();
        
        ContentResponse response = httpClient.newRequest("https://localhost:8443")
                .send();
        
        assertThat(200, equalTo(response.getStatus()));
        assertThat("Hello, world", equalTo(response.getContentAsString()));
        
        httpClient.stop();
}
```

