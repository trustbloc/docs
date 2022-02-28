# User Onboarding and Recovery

In the TrustBloc environment, KMS server is used as an **authorization KMS** for supporting [ZCAP-LD][zcap-ld] scheme
and as an **operational KMS** for regular user's crypto operations.

Both are part of the user onboarding flow in the Wallet.

```{image} ../_static/onboard_user_flow.png
```

## Authorization KMS

Authorization KMS allows creating Controller identity (identified by a cryptographic key pair) and then using it for
signing requests in a [ZCAP-LD][zcap-ld] authorization model.

The key store for the Controller uses the server's database for storing keys. These keys are encrypted with a
[Shamir secret lock][shamir-secret-lock].

To unlock a key store, the controller's secret share needs to be present in a `Secret-Share` header for every request.
The other share comes from the Auth server. The URL of the server is configured with `KMS_AUTH_SERVER_URL` environment
variable (or `--auth-server-url` flag).

[Server Key Manager][kms-architecture] is protected with a [Local secret lock][local-secret-lock].

## Operational KMS

Operational KMS manages users' working keys and supports crypto operations with those keys.

Keys are protected with a [Local secret lock][local-secret-lock] and saved to the [EDV server][edv-server] provided by
the user. EDV parameters are specified in the request upon creating a key store. Refer to the [Storage][kms-storage]
section for the details.

Recipient and MAC keys for accessing an EDV server are stored in [Server Key Manager][kms-architecture]. A primary key
for the user's key store lock is also saved here.

Operational KMS uses [ZCAP-LD][zcap-ld] authorization scheme. Requests are expected to be signed with the Authorization
KMS.

[Server Key Manager][kms-architecture] is protected with an [AWS secret lock][aws-secret-lock].

## Oathkeeper

Certain KMS endpoints are protected with the OAuth scheme. TrustBloc uses [Oathkeeper][oathkeeper] as an authorization
proxy in front of KMS services to protect the following endpoints:

* `POST /v1/keystores` for creating a key store;
* `POST /v1/keystores/did` for creating a DID (e.g. controller for EDV).

An example configuration for Authorization KMS can be found [here][oathkeeper-authz-kms].

## Recovery

Planned functionality.


[zcap-ld]: https://w3c-ccg.github.io/zcap-ld/
[shamir-secret-lock]: https://github.com/trustbloc/kms#shamir-secret-lock
[local-secret-lock]: https://github.com/trustbloc/kms#local-secret-lock
[aws-secret-lock]: https://github.com/trustbloc/kms#aws-secret-lock
[kms-architecture]: https://github.com/trustbloc/kms#architecture-overview
[kms-storage]: https://github.com/trustbloc/kms#storage
[edv-server]: https://github.com/trustbloc/edv
[oathkeeper]: https://github.com/ory/oathkeeper
[oathkeeper-authz-kms]: https://github.com/trustbloc/kms/blob/main/test/bdd/fixtures/oathkeeper-config/auth-keyserver/config.yaml
