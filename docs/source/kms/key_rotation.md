# Key Rotation

There are three types of keys in KMS that can be rotated. User operational keys, the primary key for the user key store, the primary key for the server key store.

## User operational keys

A user operation key can be rotated by calling POST /v1/keystores/{keystore}/keys/{key}/rotate
It will return a new URL for the rotated key.

When the key is rotated new key material is added, but old ones are not deleted.
New key material is used to encrypt new data.
To decrypt data at first the new key material is used, if decrypt fails old key material is used.
This allows decrypting data encrypted before rotation.

Key material - is actually key cryptographic data that is used to encrypt, decrypt operations.
Key - is a set of key material ordered by data, from newest to oldest.

Key rotation implemented using LocalKMS.Rotate from aries-framework-go.

## User key store primary key

Not implemented.

## Server key store primary key

The server key store used AWS KMS to protect keys. AWS KMS supports [automatic key rotations](https://docs.aws.amazon.com/kms/latest/developerguide/rotate-keys.html#rotate-keys-how-it-works).
