# Key Rotation

There are three types of keys in KMS that can be rotated:

* user operational keys
* primary key for the user's key store
* primary key for the server's key store

## User operational keys

A user operation key can be rotated by making a request

`POST /v1/keystores/{keystore}/keys/{key}/rotate`.

It will return a new URL for the rotated key.

When the key is rotated, a new key material is added, but old ones are not deleted.
New key material is used to encrypt new data.
To decrypt data at first the new key material is used. If decryption fails, old key material is used.
This allows decrypting data that were encrypted before rotation.

Key material - is actually key cryptographic data that is used to encrypt, decrypt operations.
Key - is a set of key material ordered by data, from newest to oldest.

Key rotation is implemented using LocalKMS.Rotate from aries-framework-go.

## User key store primary key

Not implemented yet.

## Server key store primary key

The server key store uses AWS KMS to protect keys. AWS KMS supports [automatic key rotations](https://docs.aws.amazon.com/kms/latest/developerguide/rotate-keys.html#rotate-keys-how-it-works).
