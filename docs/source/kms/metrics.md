# Metrics

An KMS server records performance metrics at each subsystem. A [Prometheus](https://prometheus.io/) server may be used to periodically read the metrics at the URL `KMS_METRICS_HOST/metrics`. Below are the metrics defined at each subsystem.

## Crypto

The Crypto metrics measure times for crypto operations.

### crypto_sign_seconds

The time (in seconds) it takes to sign message.

## Database

Database metrics record the times for reads, writes, bulk writes, etc.

### db_put_seconds

The time (in seconds) it takes the DB to store data.

### db_get_seconds

The time (in seconds) it takes the DB to retrieve data by primary key.

### db_get_tags_seconds

The time (in seconds) it takes the DB to get tags.

### db_get_bulk_seconds

The time (in seconds) it takes the DB to get bulk.

### db_query_seconds

The time (in seconds) it takes to query for data.

### db_delete_seconds

The time (in seconds) it takes to delete data.

### db_batch_seconds

The time (in seconds) it takes to perform a batch update.

## Key store.

The Key store metrics measure times for operations with keys in a key store.

### key_store_resolve_seconds

The time (in seconds) it takes to resolve a user key store.

### key_store_get_key_seconds

The time (in seconds) it takes to get a key from a user key store.

### key_store_aws_secret_lock_encrypt_seconds

The time (in seconds) it takes to encrypt a key using AWS secret lock.

### key_store_aws_secret_lock_decrypt_seconds

The time (in seconds) it takes to decrypt a key using AWS secret lock.

### key_store_key_secret_lock_encrypt_seconds

The time (in seconds) it takes to encrypt a key using a key-based secret lock.

### key_store_key_secret_lock_decrypt_seconds

The time (in seconds) it takes to decrypt a key using a key-based secret lock.
