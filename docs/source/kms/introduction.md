# Introduction

## What is KMS?

KMS lets to manage cryptographic keys and use them to perform crypto operations. [TrustBloc KMS][trustbloc-kms] is a
server implementation of [KMS][kms-api] and [Crypto][crypto-api] APIs from the [Aries Framework Go][aries-framework-go].

It can be used as a [remote KMS][remote-kms] and a [remote Crypto][remote-crypto] for the Aries Framework's `webkms`
implementation.

[trustbloc-kms]: https://github.com/trustbloc/kms
[aries-framework-go]: https://github.com/hyperledger/aries-framework-go
[kms-api]: https://github.com/hyperledger/aries-framework-go/blob/main/pkg/kms/api.go
[crypto-api]: https://github.com/hyperledger/aries-framework-go/blob/main/pkg/crypto/api.go
[remote-kms]: https://github.com/hyperledger/aries-framework-go/blob/main/pkg/kms/webkms/remotekms.go
[remote-crypto]: https://github.com/hyperledger/aries-framework-go/blob/main/pkg/crypto/webkms/remotecrypto.go
