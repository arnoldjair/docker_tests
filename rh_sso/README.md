# Instructions

## Login with docker to the redhat repository [1]

```bash
docker login registry.redhat.io
```
## Pull the RH SSO image
```bash
docker pull registry.redhat.io/rh-sso-7/sso76-openshift-rhel8:7.6-35
```

## Create HTTPS and JGroups Keystores, and Truststore
```bash
mkdir keystore
openssl req -new -newkey rsa:2048 -x509 -keyout keystore/xpaas.key -out keystore/xpaas.crt -days 365 -subj "/CN=localhost" -nodes
keytool -genkeypair -keyalg RSA -keysize 2048 -dname "CN=localhost" -alias jboss -keystore keystore/keystore.jks -storepass secret -keypass secret
keytool -certreq -keyalg rsa -alias jboss -keystore keystore/keystore.jks -file keystore/sso.csr -storepass secret
openssl x509 -req -CA keystore/xpaas.crt -CAkey keystore/xpaas.key -in keystore/sso.csr -out keystore/sso.crt -days 365 -CAcreateserial
keytool -import -file keystore/xpaas.crt -alias xpaas.ca -keystore keystore/keystore.jks -storepass secret -trustcacerts -noprompt
keytool -import -file keystore/sso.crt -alias jboss -keystore keystore/keystore.jks -storepass secret
mkdir jgroups
keytool -genseckey -alias secret-key -storetype JCEKS -keystore jgroups/jgroups.jceks -storepass secret -keypass secret -keyalg DES
mkdir truststore
keytool -import -file keystore/xpaas.crt -alias xpaas.ca -keystore truststore/truststore.jks -storepass secret -trustcacert
```
## Follow the instructions to create the certificates for postgresql (root repo), but in the postgresql folder inside this.

[Postgresql instructions](../postgresql/README.md)

## Execute docker-compose.yml

```bash
docker-compose up -d
```

# Bibliography
[1] 	R. Hat, "Red Hat SSO 7.6," Red Hat, [Online]. Available: https://catalog.redhat.com/software/containers/rh-sso-7/sso76-openshift-rhel8/629651e2cddbbde600c0a2ec?architecture=amd64&image=653bb75db2675c19e6bdc14e&container-tabs=gti&gti-tabs=red-hat-login.