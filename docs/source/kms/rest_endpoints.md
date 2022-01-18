# REST Endpoints

Following is a list of all REST endpoints exposed by KMS:

## Server

### GET /healthcheck

Returns a health check status.

## KMS

### POST /v1/keystores

Creates a new key store.

### POST /v1/keystores/{key_store_id}/keys

Creates a new key.

### PUT /v1/keystores/{key_store_id}/keys

Imports a private key.

### GET /v1/keystores/{key_store_id}/keys/{key_id}

Exports a public key.

### POST /v1/keystores/{key_store_id}/keys/{key_id}/rotate

Rotates the key.

### POST /v1/keystores/did

Creates a DID.

## Crypto

### POST /v1/keystores/{key_store_id}/easyopen

Unseals a message sealed with Easy.

### POST /v1/keystores/{key_store_id}/keys/{key_id}/computemac

Computes message authentication code (MAC) for data.

### POST /v1/keystores/{key_store_id}/keys/{key_id}/decrypt

Decrypts a ciphertext with associated authenticated data.

### POST /v1/keystores/{key_store_id}/keys/{key_id}/deriveproof

Creates a BBS+ signature proof for a list of revealed messages.

### POST /v1/keystores/{key_store_id}/keys/{key_id}/easy

Seals a message.

### POST /v1/keystores/{key_store_id}/keys/{key_id}/encrypt

Encrypts a message with associated authenticated data.

### POST /v1/keystores/{key_store_id}/keys/{key_id}/sign

Signs a message.

### POST /v1/keystores/{key_store_id}/keys/{key_id}/signmulti

Creates a BBS+ signature of messages.

### POST /v1/keystores/{key_store_id}/keys/{key_id}/unwrap

Unwraps a wrapped key.

### POST /v1/keystores/{key_store_id}/keys/{key_id}/verify

Verifies a signature.

### POST /v1/keystores/{key_store_id}/keys/{key_id}/verifymac

Verifies whether MAC is a correct authentication code for data.

### POST /v1/keystores/{key_store_id}/keys/{key_id}/verifymulti

Verifies a signature of messages (BBS+).

### POST /v1/keystores/{key_store_id}/keys/{key_id}/verifyproof

Verifies a BBS+ signature proof for revealed messages.

### POST /v1/keystores/{key_store_id}/keys/{key_id}/wrap

Wraps CEK using ECDH-1PU key wrapping (Authcrypt).

### POST /v1/keystores/{key_store_id}/sealopen

Decrypts a payload encrypted with Seal.

### POST /v1/keystores/{key_store_id}/wrap

Wraps CEK using ECDH-ES key wrapping (Anoncrypt).
