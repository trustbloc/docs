# Caching

KMS uses caching to improve the performance of several components (server's database, users' key stores, etc.). Caching
functionality is backed by the [ristretto][ristretto] implementation.

Caching support is enabled with `KMS_CACHE_ENABLE=true` environment variable (`--enable-cache=true` flag). This turns on
caching for operations with server's database. Other caches can be additionally configured with further parameters.   

## Server's DB cache

The server's DB is used for storing and retrieving

* keys for server's KMS;
* as a default option for users' key stores (EDV is an alternative option);
* JSON-LD contexts and authorization capabilities (ZCAP-LD);
* metadata for users' key stores.

Cache [wraps][wrap-provider] the storage and acts as a proxy, so the operations with the underlying database are cached.

It's important to note that when the server's KMS uses cached storage, key materials are cached in an encrypted form.
The secret lock is still required to get usable keys.

## Key store cache

Users' key stores can be cached for a limited period of time set by `KMS_KEY_STORE_CACHE_TTL=10m` environment variable
(`--key-store-cache-ttl=10m` flag). The default value is 10 minutes.

This cache is especially useful when the key store uses EDV. The keys' materials are always cached in an encrypted form.

## Secret lock keys cache

In the case of using AWS KMS as a secret lock for server kms, encrypt and decrypt operations can become expensive.
To decrease the number of calls to AWS KMS, decrypted keys can be cached. Caching is enabled by default and default ttl is 10m.
To disable caching set KMS_KMS_CACHE_TTL=0s.
An appropriate ttl can be set using the same KMS_KMS_CACHE_TTL variable.

## Shamir secret cache

Shamir secrets fetched from Auth Server are cached by default. 
Default ttl for Shamir secrets is 10m.
To disable caching set KMS_SHAMIR_SECRET_CACHE_TTL=0s.
An appropriate ttl can be set using the same KMS_SHAMIR_SECRET_CACHE_TTL variable.


[ristretto]: https://github.com/dgraph-io/ristretto
[wrap-provider]: https://github.com/trustbloc/kms/blob/main/pkg/storage/cache/wrap_provider.go
