# User Onboarding and Recovery

KMS server can play a different role depending on the concrete setup.

In the TrustBloc environment, the KMS server is used as an authorization KMS for supporting [ZCAP-LD][zcap-ld] scheme
and as an operational KMS for regular user's crypto operations.

Here is an example setup for user onboarding flow in the Wallet:

```{image} ../_static/onboard_user_flow.png
```

## Authorization KMS

Authorization KMS allows creating Controller identity (identified by a cryptographic key pair) and then using it for
signing requests in a ZCAP-LD authorization model.

The key store for the Controller uses the server's database for storing keys. These keys are encrypted with a
[Shamir secret lock][shamir-secret-lock].

To unlock a key store, the controller's secret share needs to be present in a `Secret-Share` header for every request.
The other share comes from the Auth server. The endpoint for fetching that share is configured with `KMS_AUTH_SERVER_URL`
environment variable.

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

## Recovery


[zcap-ld]: https://w3c-ccg.github.io/zcap-ld/
[shamir-secret-lock]: https://github.com/trustbloc/kms#shamir-secret-lock
[local-secret-lock]: https://github.com/trustbloc/kms#local-secret-lock
[aws-secret-lock]: https://github.com/trustbloc/kms#aws-secret-lock
[kms-architecture]: https://github.com/trustbloc/kms#architecture-overview
[kms-storage]: https://github.com/trustbloc/kms#storage
[edv-server]: https://github.com/trustbloc/edv
