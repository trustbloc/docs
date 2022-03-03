# Keys

## Types of the keys in KMS

1) User's working key - for operations initiated by the user (sign, wrap, encrypt, etc.).
2) Secret lock key - for encrypting/decrypting other keys in the server's DB or EDV.
3) Keys for supporting EDV operations (recipient public key, MAC key).

## Where keys are stored

**Users' working keys** are stored either in the server's DB (CouchDB or MongoDB) or in EDV. This depends on whether the
`create key store` request contains EDV options (vault URL and authorization capabilities (ZCAPs) to access vault).

When EDV is used, both recipient and MAC keys that are needed for the EDV provider are stored in the server's DB. In EDV
data are encrypted so the private key associated with the **recipient public key** can decrypt them. **MAC key** is used
for generating deterministic document ID.

Any key that is stored in the server's DB is encrypted with the lock key. There are 2 options for the server's lock key:
**local file** or **AWS KMS**.

Users' working keys are protected with the secret lock as well. There are also 2 options for the user's secret lock key:
**local key** stored in the server's DB (that key is created when the `create key store` request is processed and is
associated with the key store) or key based on **Shamir Secret Sharing** scheme. In this case, a key is generated on a
fly from 2 shares - one is stored on Auth server and the other comes in the header with each request.

## Use cases

**Scenario 1**: server's lock is based on AWS key, user's lock uses local key, no EDV

In this scenario, a key for the user's lock is created when the key store is created. That key is encrypted with an AWS
key and stored in the server's DB. When a working key is created for the user, it is encrypted with that stored lock key.
Before using, user's lock key should be decrypted with an AWS key.

**Scenario 2**: server's lock is based on local key, user's lock uses Shamir-based key, working keys are stored in EDV

Key for the server's lock is stored in a local file and the path to it is specified in a startup flag or environment
variable. When a key store is created, helper recipient and MAC keys for the EDV provider are created as well. They are
encrypted with a key from the local file (server's lock) and saved to the server's DB. These keys are associated with
a created key store to support EDV operations.

User's lock key is created on a fly using HKDF algorithm that expands the combined secret (from shares using Shamir
Secret Sharing) into a symmetric key. That key is used to encrypt/decrypt the user's working keys stored in EDV.

## What is the impact of losing/compromising the key

| Key                | Loss/compromise                                                        |
|--------------------|------------------------------------------------------------------------|
| server's lock key  | no/malicious access to all keys locked with that key                   |
| user's lock key    | no/malicious access to all user's working keys                         |
| user's working key | blocked/malicious operations with that key (signing, encrypting, etc.) |
| EDV recipient key  | no/malicious access to encrypted doc in EDV                            |
