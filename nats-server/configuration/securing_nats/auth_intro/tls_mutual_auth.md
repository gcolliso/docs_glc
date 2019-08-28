# TLS Authentication

The server can require TLS certificates from a client. When needed, you can use the certificates to:

* Validate the client certificate matches a known or trusted CA
* Extract information from a trusted certificate to provide authentication

## Validating a Client Certificate

The server can verify a client certificate using a CA certificate. To require verification, add the option `verify` the TLS configuration section as follows:

```text
tls {
  cert_file: "./configs/certs/server-cert.pem"
  key_file:  "./configs/certs/server-key.pem"
  ca_file:   "./configs/certs/ca.pem"
  verify:    true
}
```

Or via the command line:

```bash
> ./nats-server --tlsverify --tlscert=./test/configs/certs/server-cert.pem --tlskey=./test/configs/certs/server-key.pem --tlscacert=./test/configs/certs/ca.pem
```

This option verifies the client's certificate is signed by the CA specified in the `ca_file` option.

## Mapping Client Certificates To A User

In addition to verifying that a specified CA issued a client certificate, you can use information encoded in the certificate to authenticate a client. The client wouldn't have to provide or track usernames or passwords.

To have TLS Mutual Authentication map certificate attributes to the user's identity use `verify_and_map` as shown as follows:

```text
tls {
  cert_file: "./configs/certs/server-cert.pem"
  key_file:  "./configs/certs/server-key.pem"
  ca_file:   "./configs/certs/ca.pem"
  # Require a client certificate and map user id from certificate
  verify_and_map: true
}
```

> Note that `verify` was changed to `verify_and_map`.

There are two options for certificate attributes that can be mapped to user names. The first is the email address in the Subject Alternative Name \(SAN\) field of the certificate. While generating a certificate with this attribute is outside the scope of this document, you can view one with `openssl`:

```text
$ openssl x509 -noout -text -in  test/configs/certs/client-id-auth-cert.pem
Certificate:
...
        X509v3 extensions:
            X509v3 Subject Alternative Name:
                DNS:localhost, IP Address:127.0.0.1, email:derek@nats.io
            X509v3 Extended Key Usage:
                TLS Web Client Authentication
...
```

The configuration to authorize this user would be as follows:

```text
authorization {
  users = [
    {user: "derek@nats.io"}
  ]
}
```

> Note: This configuration only works for the first email address if there are multiple emails in the SAN field.

The second option is to use the RFC 2253 Distinguished Names syntax from the certificate subject as follows:

```text
$ openssl x509 -noout -text -in  test/configs/certs/tlsauth/client2.pem
Certificate:
    Data:
...
        Subject: OU=CNCF, CN=example.com
...
```

The configuration to authorize this user would be as follows:

```text
authorization {
  users = [
    {user: "CN=example.com,OU=CNCF"}
  ]
}
```

## TLS Timeout

[TLS timeout](../tls.md#tls-timeout) is described here.

