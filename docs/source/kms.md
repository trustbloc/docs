# Key Management System (KMS)

## What is KMS?

KMS lets to manage cryptographic keys and use them to perform crypto operations. [TrustBloc KMS][trustbloc-kms] is a
server implementation of [KMS][kms-api] and [Crypto][crypto-api] APIs from the [Aries Framework Go][aries-framework-go].

It can be used as a [remote KMS][remote-kms] and a [remote Crypto][remote-crypto] for the framework's `webkms`
implementation.

## Running KMS server

KMS server can be run as a standalone binary or a docker container. Refer [here][running-kms-server] for the instructions
on how to run a server and supported startup flags and options.

## KMS in a TrustBloc environment

KMS server can play a different role depending on the concrete setup.

In the TrustBloc environment, the KMS server is used as an authorization KMS for supporting [ZCAP-LD][zcap-ld] scheme
and as an operational KMS for regular user's crypto operations.

### Authorization KMS

Authorization KMS allows creating Controller identity (identified by a cryptographic key pair) and then using it for
signing requests in a ZCAP-LD authorization model.

The key store for the Controller uses the server's database for storing keys. These keys are encrypted with a
[Shamir secret lock][shamir-secret-lock].

To unlock a key store, the controller's secret share needs to be present in a `Secret-Share` header for every request.
The other share comes from the Auth server. The endpoint for fetching that share is configured with `KMS_AUTH_SERVER_URL`
environment variable.

[Server Key Manager][kms-architecture] is protected with a [Local secret lock][local-secret-lock].

### Operational KMS

Operational KMS manages users' working keys and supports crypto operations with those keys.
 
Keys are protected with a [Local secret lock][local-secret-lock] and saved to the [EDV server][edv-server] provided by
the user. EDV parameters are specified in the request upon creating a key store. Refer to the [Storage][kms-storage]
section for the details.

Recipient and MAC keys for accessing an EDV server are stored in [Server Key Manager][kms-architecture]. A primary key
for the user's key store lock is also saved here.

Operational KMS uses [ZCAP-LD][zcap-ld] authorization scheme. Requests are expected to be signed with the Authorization
KMS. 

[Server Key Manager][kms-architecture] is protected with an [AWS secret lock][aws-secret-lock].

## Scenario: User Onboarding

```{image} _static/onboard_user_flow.png
```

[aries-framework-go]: https://github.com/hyperledger/aries-framework-go
[kms-api]: https://github.com/hyperledger/aries-framework-go/blob/main/pkg/kms/api.go
[crypto-api]: https://github.com/hyperledger/aries-framework-go/blob/main/pkg/crypto/api.go
[trustbloc-kms]: https://github.com/trustbloc/kms
[remote-kms]: https://github.com/hyperledger/aries-framework-go/blob/main/pkg/kms/webkms/remotekms.go
[remote-crypto]: https://github.com/hyperledger/aries-framework-go/blob/main/pkg/crypto/webkms/remotecrypto.go
[running-kms-server]: https://github.com/trustbloc/kms#running-kms-server
[zcap-ld]: https://w3c-ccg.github.io/zcap-ld/
[shamir-secret-lock]: https://github.com/trustbloc/kms#shamir-secret-lock
[local-secret-lock]: https://github.com/trustbloc/kms#local-secret-lock
[aws-secret-lock]: https://github.com/trustbloc/kms#aws-secret-lock
[kms-architecture]: https://github.com/trustbloc/kms#architecture-overview
[kms-storage]: https://github.com/trustbloc/kms#storage
[edv-server]: https://github.com/trustbloc/edv
