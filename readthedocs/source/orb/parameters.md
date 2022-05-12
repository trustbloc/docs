# Startup Parameters

This section enumerates the startup parameters for an Orb server. Parameters in the
[Required Parameters](#required-parameters) section are required, otherwise the server will not start.
Parameters in the [Optional Parameters](#optional-parameters) section are optional and will use a
default value if not specified.

## Required Parameters

Following are the required parameters for an Orb server.

### host-url

| Arg        | Env           |
|------------|---------------|
| --host-url | ORB_HOST_URL  |

URL to run the orb-server instance on. Format: HostName:Port.

### external-endpoint

| Arg                 | Env                    |
|---------------------|------------------------|
| --external-endpoint | ORB_EXTERNAL_ENDPOINT  |

External endpoint that clients use to invoke services. This endpoint is used to generate IDs of anchor credentials and ActivityPub objects and should be resolvable by external clients. Format: HostName[:Port].

### database-type

| Arg             | Env            |
|-----------------|----------------|
| --database-type | DATABASE_TYPE  |

The type of database to use for everything except key storage. Supported options: mem, couchdb, mongodb.

### vc-sign-private-keys

| Arg            | Env              |
|----------------|------------------|
| --vc-sign-private-keys | ORB_VC_SIGN_PRIVATE_KEYS |

VC Sign Private Keys base64 (ED25519Type). For example,  key1=privatekeyBase64Value,key2=privatekeyBase64Value

### vc-sign-active-key-id

| Arg             | Env              - |
|-----------------|--------------------|
| --vc-sign-active-key-id | ORB_VC_SIGN_ACTIVE_KEY_ID  |

VC Sign Active Key ID (ED25519Type).

### vc-sign-keys-id

| Arg             | Env              - |
|-----------------|--------------------|
| --vc-sign-keys-id | ORB_VC_SIGN_KEYS_ID  |

VC Sign Keys id in kms (ED25519Type).

### http-sign-private-key

| Arg            | Env              |
|----------------|------------------|
| --http-sign-private-key | ORB_HTTP_SIGN_PRIVATE_KEY |

HTTP Sign Private Key base64 (ED25519Type). For example,  key1=privatekeyBase64Value

### http-sign-active-key-id

| Arg             | Env              - |
|-----------------|--------------------|
| --http-sign-active-key-id | ORB_HTTP_SIGN_ACTIVE_KEY_ID  |

HTTP Sign Active Key ID (ED25519Type).

### did-namespace

| Arg             | Env            |
|-----------------|----------------|
| --did-namespace | DID_NAMESPACE  |

DID Namespace.

### cas-type

| Arg        | Env       |
|------------|-----------|
| --cas-type | CAS_TYPE  |

The type of the Content Addressable Storage (CAS). Supported options: local, ipfs.

### anchor-credential-issuer

| Arg                        | Env                      |
|----------------------------|--------------------------|
| --anchor-credential-issuer | ANCHOR_CREDENTIAL_ISSUER |

Anchor credential issuer (required).

### anchor-credential-url

| Arg                     | Env                    |
|-------------------------|------------------------|
| --anchor-credential-url | ANCHOR_CREDENTIAL_URL  |

Anchor credential url (required).

### anchor-credential-signature-suite

| Arg                                 | Env                                 |
|-------------------------------------|-------------------------------------|
| --anchor-credential-signature-suite | ANCHOR_CREDENTIAL_SIGNATURE_SUITE   |

Anchor credential signature suite (required).

## Optional Parameters

Below are the optional parameters for an Orb server. If not specified then the default value is used.

### host-metrics-url

| Arg                | Env                  | Default |
|--------------------|----------------------|---------|
| --host-metrics-url | ORB_HOST_METRICS_URL |         |

URL that exposes the metrics endpoint. Format: HostName:Port.

### sync-timeout

| Arg            | Env              | Default |
|----------------|------------------|---------|
| --sync-timeout | ORB_SYNC_TIMEOUT | 1       |

Total time in seconds to resolve config values.

### vct-proof-monitoring-interval

| Arg                             | Env                           | Default |
|---------------------------------|-------------------------------|---------|
| --vct-proof-monitoring-interval | VCT_PROOF_MONITORING_INTERVAL | 10s     |

The interval in which VCTs are monitored to ensure that proofs are anchored.

### vct-log-monitoring-interval

| Arg                           | Env                         | Default |
|-------------------------------|-----------------------------|---------|
| --vct-log-monitoring-interval | VCT_LOG_MONITORING_INTERVAL | 10s     |

The interval in which VCT logs are monitored to ensure that they are consistent.


### vct-log-monitoring-max-tree-size

| Arg                                | Env                              | Default |
|------------------------------------|----------------------------------|---------|
| --vct-log-monitoring-max-tree-size | VCT_LOG_MONITORING_MAX_TREE_SIZE | 50000   |

The maximum tree size for which new VCT logs will be re-constructed in order to verify signed tree head.

### vct-log-monitoring-get-entries-range

| Arg                                    | Env                                  | Default |
|----------------------------------------|--------------------------------------|---------|
| --vct-log-monitoring-get-entries-range | VCT_LOG_MONITORING_GET_ENTRIES_RANGE | 1000    |

The maximum number of entries to be retrieved from VCT log in one attempt. Has to be less or equal than 1000 due to VCT limitation.

### vct-log-entries-store-enabled

| Arg                               | Env                           | Default |
|-----------------------------------|-------------------------------|---------|
| --vct-log-entries-store-enabled   | VCT_LOG_ENTRIES_STORE_ENABLED | false   |

Enables storing of log entries during log monitoring. Defaults to false if not set.

### anchor-status-monitoring-interval

| Arg                                 | Env                               | Default |
|-------------------------------------|-----------------------------------|---------|
| --anchor-status-monitoring-interval | ANCHOR_STATUS_MONITORING_INTERVAL | 5s      |

The interval in which 'in-process' anchors are monitored to ensure that they will be witnessed(completed) as per
policy.

### anchor-status-in-process-grace-period

| Arg                                     | Env                                   | Default |
|-----------------------------------------|---------------------------------------|---------|
| --anchor-status-in-process-grace-period | ANCHOR_STATUS_IN_PROCESS_GRACE_PERIOD | 1m      |

The period in which witnesses will not be re-selected for 'in-process' anchors.

### kms-store-endpoint

| Arg                  | Env                     | Default |
|----------------------|-------------------------|---------|
| --kms-store-endpoint | ORB_KMS_STORE_ENDPOINT  |         |

KMS storage URL. If this parameter is not set then ORB_KMS_ENDPOINT needs to be set.

### kms-endpoint

| Arg            | Env               | Default |
|----------------|-------------------|---------|
| --kms-endpoint | ORB_KMS_ENDPOINT  |         |

Remote KMS URL. If this parameter is not set then ORB_KMS_STORE_ENDPOINT needs to be set.

### secret-lock-key-path

| Arg                    | Env                       | Default |
|------------------------|---------------------------|---------|
| --secret-lock-key-path | ORB_SECRET_LOCK_KEY_PATH  |         |

The path to the file with key to be used by local secret lock. If missing noop service lock is used.

### discovery-domain

| Arg                | Env                   | Default |
|--------------------|-----------------------|---------|
| --discovery-domain | ORB_DISCOVERY_DOMAIN  |         |

Discovery domain for this domain. Format: HostName

### tls-systemcertpool

| Arg                  | Env                     | Default |
|----------------------|-------------------------|---------|
| --tls-systemcertpool | ORB_TLS_SYSTEMCERTPOOL  | false   |

Use system certificate pool. Possible values true and false. Defaults to false if not set.

### tls-cacerts

| Arg           | Env              | Default |
|---------------|------------------|---------|
| --tls-cacerts | ORB_TLS_CACERTS  |         |

Comma-Separated list of ca certs path.

### tls-certificate

| Arg               | Env                  | Default |
|-------------------|----------------------|---------|
| --tls-certificate | ORB_TLS_CERTIFICATE  |         |

TLS certificate for ORB server.

### tls-key

| Arg       | Env          | Default |
|-----------|--------------|---------|
| --tls-key | ORB_TLS_KEY  |         |

TLS key for ORB server.

### did-aliases

| Arg           | Env          | Default |
|---------------|--------------|---------|
| --did-aliases | DID_ALIASES  |         |

Aliases for this did method.

### ipfs-url

| Arg        | Env       | Default |
|------------|-----------|---------|
| --ipfs-url | IPFS_URL  |         |

Enables IPFS support. If set, this Orb server will use the node at the given URL. To use the public ipfs.io node, set this to https://ipfs.io (or http://ipfs.io). If using ipfs.io, then the CAS type flag must be set to local since the ipfs.io node is read-only. If the URL doesn't include a scheme, then HTTP will be used by default.

### replicate-local-cas-writes-in-ipfs

| Arg                                   | Env                                | Default |
|---------------------------------------|------------------------------------|---------|
| --replicate-local-cas-writes-in-ipfs  | REPLICATE_LOCAL_CAS_WRITES_IN_IPFS | false   |

If enabled, writes to the local CAS will also be replicated in IPFS. This setting only takes effect if this server has both a local CAS and IPFS enabled. If the IPFS node is set to ipfs.io, then this setting will be disabled since ipfs.io does not support writes. Supported options: false, true. Defaults to false if not set.

### mq-url

| Arg      | Env     | Default |
|----------|---------|---------|
| --mq-url | MQ_URL  |         |

The URL of the message broker. If not specified then an in-memory message queue is used.

### mq-connect-max-retries

| Arg                      | Env                    | Default |
|--------------------------|------------------------|---------|
| --mq-connect-max-retries | MQ_CONNECT_MAX_RETRIES | 25      |

The maximum number of retries to connect to an AMQP service, after which the server will panic.

### mq-observer-pool

| Arg                | Env              | Default |
|--------------------|------------------|---------|
| --mq-observer-pool | MQ_OBSERVER_POOL | 5       |

The size of the Observer queue subscriber pool. When a message is posted to the Observer queue, it is
handled by a pool of subscribers. If not specified then the default size will be used.

### mq-outbox-pool

| Arg              | Env            | Default |
|------------------|----------------|---------|
| --mq-outbox-pool | MQ_OUTBOX_POOL | 5       |

The size of the Outbox queue subscriber pool. When a message is posted to the Outbox queue, it is
handled by a pool of subscribers. If not specified then the default size will be used.

### mq-inbox-pool

| Arg             | Env           | Default |
|-----------------|---------------|---------|
| --mq-inbox-pool | MQ_INBOX_POOL | 5       |

The size of the Inbox queue subscriber pool. When a message is posted to the Inbox queue, it is
handled by a pool of subscribers. If not specified then the default size will be used.

### mq-max-connection-subscriptions

| Arg                               | Env                             | Default  |
|-----------------------------------|---------------------------------|----------|
| --mq-max-connection-subscriptions | MQ_MAX_CONNECTION_SUBSCRIPTIONS | 1000     |

The maximum number of subscriptions per connection.

### mq-publisher-channel-pool-size

| Arg                              | Env               | Default |
|----------------------------------|-------------------|---------|
| --mq-publisher-channel-pool-size | MQ_PUBLISHER_POOL | 25      |

The size of a channel pool for an AMQP publisher (default is 25). If set to 0 then a channel pool is not used and a new channel is opened/closed for every publish to a queue.

### mq-publisher-confirm-delivery

| Arg                             | Env                           | Default |
|---------------------------------|-------------------------------|---------|
| --mq-publisher-confirm-delivery | MQ_PUBLISHER_CONFIRM_DELIVERY | true    |

Turns on delivery confirmation of published messages. If set to true then the AMQP publisher waits
until a confirmation is received from the AMQP server to guarantee that the message is delivered.

### mq-redelivery-max-attempts

| Arg                               | Env                        | Default  |
|-----------------------------------|----------------------------|----------|
| --mq-redelivery-max-attempts      | MQ_REDELIVERY_MAX_ATTEMPTS | 20       |

The maximum number of redelivery attempts for a failed message.

### mq-redelivery-initial-interval

| Arg                                | Env                            | Default |
|------------------------------------|--------------------------------|---------|
| --mq-redelivery-initial-interval   | MQ_REDELIVERY_INITIAL_INTERVAL | 2s      |

The delay for the initial redelivery attempt.

### mq-redelivery-multiplier

| Arg                        | Env                      | Default |
|----------------------------|--------------------------|---------|
| --mq-redelivery-multiplier | MQ_REDELIVERY_MULTIPLIER | 1.5     |

The multiplier for a redelivery attempt. For example, if set to 1.5 and the previous
redelivery interval was 2s then the next redelivery interval is set 3s.

### mq-redelivery-max-interval

| Arg                          | Env                        | Default |
|------------------------------|----------------------------|---------|
| --mq-redelivery-max-interval | MQ_REDELIVERY_MAX_INTERVAL | 30s     |

The maximum delay for a redelivery.

### op-queue-pool

| Arg             | Env           | Default  |
|-----------------|---------------|----------|
| --op-queue-pool | OP_QUEUE_POOL | 5        |

The size of the operation queue subscriber pool. If <=1 then a pool will not be created.

### op-queue-task-monitor-interval

| Arg                              | Env                            | Default |
|----------------------------------|--------------------------------|---------|
| --op-queue-task-monitor-interval | OP_QUEUE_TASK_MONITOR_INTERVAL | 10s     |

The interval (period) in which operation queue tasks from other server instances are monitored.

### op-queue-task-expiration

| Arg                        | Env                      | Default |
|----------------------------|--------------------------|---------|
| --op-queue-task-expiration | OP_QUEUE_TASK_EXPIRATION | 30s     |

The maximum time that an operation queue task can exist in the database before it is considered to have expired.
Once expired, any other server instance may delete the task and repost operations associated with the task
to the queue, so that they will be processed by another Orb instance.

### op-queue-max-reposts

| Arg                    | Env                  | Default |
|------------------------|----------------------|---------|
| --op-queue-max-reposts | OP_QUEUE_MAX_REPOSTS | 20      |

The maximum number of times an operation may be reposted to the queue after having failed.

### cid-version

| Arg           | Env          | Default |
|---------------|--------------|---------|
| --cid-version | CID_VERSION  | 1       |

The version of the CID format to use for generating CIDs. Supported options: 0, 1. If not set, defaults to 1.

### batch-writer-timeout

| Arg                    | Env                   | Default |
|------------------------|-----------------------|---------|
| --batch-writer-timeout | BATCH_WRITER_TIMEOUT  | 60s     |

Maximum time (in millisecond) in-between cutting batches.

### database-url

| Arg            | Env           | Default  |
|----------------|---------------|----------|
| --database-url | DATABASE_URL  |          |

The URL (or connection string) of the database. Not needed if using memstore. For CouchDB, include the username:password@ text if required.

### database-prefix

| Arg               | Env              | Default |
|-------------------|------------------|---------|
| --database-prefix | DATABASE_PREFIX  |         |

An optional prefix to be used when creating and retrieving underlying databases. This allows
a database to be shared by multiple Orb domains. (Mainly used in development environments.)

### kms-secrets-database-type

| Arg                         | Env                       | Default  |
|-----------------------------|---------------------------|----------|
| --kms-secrets-database-type | KMSSECRETS_DATABASE_TYPE  |          |

The type of database to use for storage of KMS secrets. Supported options: mem, couchdb, mongodb.

### kms-secrets-database-url

| Arg                        | Env                      | Default |
|----------------------------|--------------------------|---------|
| --kms-secrets-database-url | KMSSECRETS_DATABASE_URL  |         |

The URL (or connection string) of the database. Not needed if using memstore. For CouchDB, include the username:password@ text if required.

### kms-secrets-database-prefix

| Arg                           | Env                         | Default |
|-------------------------------|-----------------------------|---------|
| --kms-secrets-database-prefix | KMSSECRETS_DATABASE_PREFIX  |         |

An optional prefix to be used when creating and retrieving the underlying KMS secrets database.
This allows a database to be shared by multiple Orb domains. (Mainly used in development environments.)

### database-timeout

| Arg                | Env               | Default |
|--------------------|-------------------|---------|
| --database-timeout | DATABASE_TIMEOUT  | 10s     |

The timeout for database requests. For example, '30s' for a 30 second timeout. Currently this setting only applies if you're using MongoDB.

### anchor-credential-domain

| Arg                        | Env                       | Default               |
|----------------------------|---------------------------|-----------------------|
| --anchor-credential-domain | ANCHOR_CREDENTIAL_DOMAIN  | ORB_EXTERNAL_ENDPOINT |

Anchor credential domain.

### allowed-origins

| Arg               | Env              | Default |
|-------------------|------------------|---------|
| --allowed-origins | ALLOWED_ORIGINS  |         |

Allowed origins for this did method.

### max-witness-delay

| Arg                 | Env                | Default |
|---------------------|--------------------|---------|
| --max-witness-delay | MAX_WITNESS_DELAY  | 10m     |

Maximum witness response time.

### sign-with-local-witness

| Arg                       | Env                      | Default |
|---------------------------|--------------------------|---------|
| --sign-with-local-witness | SIGN_WITH_LOCAL_WITNESS  | true    |

Always sign with local witness flag (default true).

### discovery-domains

| Arg                 | Env                | Default |
|---------------------|--------------------|---------|
| --discovery-domains | DISCOVERY_DOMAINS  |         |

Discovery domains.

### discovery-minimum-resolvers

| Arg                           | Env                          | Default |
|-------------------------------|------------------------------|---------|
| --discovery-minimum-resolvers | DISCOVERY_MINIMUM_RESOLVERS  | 1       |

Discovery minimum resolvers number.

### enable-http-signatures

| Arg                      | Env                      | Default |
|--------------------------|--------------------------|---------|
| --enable-http-signatures | HTTP_SIGNATURES_ENABLED  | true    |

Set to "true" to enable HTTP signatures in ActivityPub.

### enable-did-discovery

| Arg                    | Env                    | Default |
|------------------------|------------------------|---------|
| --enable-did-discovery | DID_DISCOVERY_ENABLED  | false   |

Set to "true" to enable did discovery.

### enable-unpublished-operation-store

| Arg                                  | Env                                 | Default |
|--------------------------------------|-------------------------------------|---------|
| --enable-unpublished-operation-store | UNPUBLISHED_OPERATION_STORE_ENABLED | false   |

Set to "true" to enable un-published operation store. Used to enable storing unpublished operations and including them when resolving documents.

### unpublished-operation-store-operation-types

| Arg                                           | Env                                          | Default        |
|-----------------------------------------------|----------------------------------------------|----------------|
| --unpublished-operation-store-operation-types | UNPUBLISHED_OPERATION_STORE_OPERATION_TYPES  | create, update |

Comma-separated list of operation types. Used if unpublished operation store is enabled.
Default value is "create,update" which enables storing unpublished 'create' and 'update' operations into
unpublished store and using those unpublished 'create' and 'update' operations for resolving document.

### include-unpublished-operations-in-metadata

| Arg                                          | Env                                         | Default |
|----------------------------------------------|---------------------------------------------|---------|
| --include-unpublished-operations-in-metadata | INCLUDE_UNPUBLISHED_OPERATIONS_IN_METADATA  | false   |

Set to "true" to include unpublished operations in metadata.

### include-published-operations-in-metadata

| Arg                                        | Env                                       | Default |
|--------------------------------------------|-------------------------------------------|---------|
| --include-published-operations-in-metadata | INCLUDE_PUBLISHED_OPERATIONS_IN_METADATA  | false   |

Set to "true" to include published operations in metadata.

### resolve-from-anchor-origin

| Arg                          | Env                         | Default |
|------------------------------|-----------------------------|---------|
| --resolve-from-anchor-origin | RESOLVE_FROM_ANCHOR_ORIGIN  | false   |

Set to "true" to resolve from anchor origin.

### verify-latest-from-anchor-origin

| Arg                                | Env                               | Default |
|------------------------------------|-----------------------------------|---------|
| --verify-latest-from-anchor-origin | VERIFY_LATEST_FROM_ANCHOR_ORIGIN  | false   |

Set to "true" to verify latest operations against anchor origin.

### auth-tokens-def

| Arg                 | Env                                | Default |
|---------------------|------------------------------------|---------|
| --auth-tokens-def   | ORB_AUTH_TOKENS_DEF                |         |

Contains the authorization definition for each of the REST endpoints. Format:

```
<path-expr>|<read-token1>&<read-token2>&...>|<write-token1>&<write-token2>&...>,<path-expr>
```

**Where:**

- path-expr contains a regular expression for a path. Path expressions are processed in the order they are specified.
- read-token defines a token for a read (GET) operation. If not specified then authorization is not performed.
- write-token defines a token for a write (POST) operation. If not specified then authorization is not performed.

If no definition is included for an endpoint then authorization is NOT performed for that endpoint.

**Example:**

```
ORB_AUTH_TOKENS_DEF=/services/orb/outbox|admin&read|admin,/services/orb/.*|read&admin
```

- The client requires a 'read' or 'admin' token in order to view the outbox's contents
- The client requires an 'admin' token in order to post to the outbox
- The client requires a 'read' or 'admin' token in order to perform a GET on any endpoint starting with /services/orb/

### auth-tokens

| Arg           | Env              | Default |
|---------------|------------------|---------|
| --auth-tokens | ORB_AUTH_TOKENS  |         |

Specifies the actual values of the tokens defined in ORB_AUTH_TOKENS_DEF.

**Example:**

admin=ADMIN_PASSWORD,read=READ_PASSWORD

### client-auth-tokens-def

| Arg                      | Env                         | Default             |
|--------------------------|-----------------------------|---------------------|
| --client-auth-tokens-def | ORB_CLIENT_AUTH_TOKENS_DEF  | ORB_AUTH_TOKENS_DEF |

Follows the same rules as ORB_AUTH_TOKENS_DEF but is used by the Orb client transport to
determine whether an HTTP signature is required for an outbound HTTP request. If not specified then it is assumed
to be the same as ORB_AUTH_TOKENS_DEF.

### client-auth-tokens

| Arg                  | Env                     | Default         |
|----------------------|-------------------------|-----------------|
| --client-auth-tokens | ORB_CLIENT_AUTH_TOKENS  | ORB_AUTH_TOKENS |

Specifies the actual values of the tokens defined in ORB_CLIENT_AUTH_TOKENS_DEF. If not specified then it is assumed to be the same as ORB_AUTH_TOKENS.

### activitypub-page-size

| Arg                     | Env                    | Default |
|-------------------------|------------------------|---------|
| --activitypub-page-size | ACTIVITYPUB_PAGE_SIZE  | 50      |

The maximum page size for an ActivityPub collection or ordered collection.

### enable-dev-mode

| Arg               | Env               | Default |
|-------------------|-------------------|---------|
| --enable-dev-mode | DEV_MODE_ENABLED  | false   |

Set to "true" to enable dev mode. When dev mode is enabled, no TLS is used.

### nodeinfo-refresh-interval

| Arg                         | Env                        | Default |
|-----------------------------|----------------------------|---------|
| --nodeinfo-refresh-interval | NODEINFO_REFRESH_INTERVAL  | 15s     |

The interval for refreshing NodeInfo data. For example, '30s' for a 30 second interval.

### ipfs-timeout

| Arg            | Env           | Default |
|----------------|---------------|---------|
| --ipfs-timeout | IPFS_TIMEOUT  | 20s     |

The timeout for IPFS requests. For example, '30s' for a 30 second timeout.

### context-provider-url

| Arg                    | Env                       | Default |
|------------------------|---------------------------|---------|
| --context-provider-url | ORB_CONTEXT_PROVIDER_URL  |         |

Comma-separated list of remote context provider URLs to get JSON-LD contexts from."

### unpublished-operation-lifetime

| Arg                              | Env                             | Default |
|----------------------------------|---------------------------------|---------|
| --unpublished-operation-lifetime | UNPUBLISHED_OPERATION_LIFETIME  | 5m      |

How long unpublished operations remain stored before expiring (and thus, being deleted some time later). For example, '1m' for a 1 minute lifespan. Defaults to 1 minute if not set.

### task-manager-check-interval

| Arg                           | Env                          | Default |
|-------------------------------|------------------------------|---------|
| --task-manager-check-interval | TASK_MANAGER_CHECK_INTERVAL  | 10s     |

How frequently to check for scheduled tasks. For example, a setting of '10s' will cause the task manager to check for outstanding tasks every 10s. Defaults to 10 seconds if not set.

### data-expiry-check-interval

| Arg                          | Env                         | Default |
|------------------------------|-----------------------------|---------|
| --data-expiry-check-interval | DATA_EXPIRY_CHECK_INTERVAL  | 1m      |

How frequently to check for (and delete) any expired data. For example, a setting of '1m' will cause the expiry service to run a check every 1 minute. Defaults to 1 minute if not set.

### follow-auth-policy

| Arg                  | Env                 | Default    |
|----------------------|---------------------|------------|
| --follow-auth-policy | FOLLOW_AUTH_POLICY  | accept-all |

The type of authorization to use when a 'Follow' ActivityPub request is received. Possible values are: 'accept-all' and 'accept-list'. The value, 'accept-all', indicates that this server will accept any 'Follow' request. The value, 'accept-list', indicates that the service sending the 'Follow' request must be included in an 'accept list'. Defaults to 'accept-all' if not set.

### invite-witness-auth-policy

| Arg                          | Env                         | Default    |
|------------------------------|-----------------------------|------------|
| --invite-witness-auth-policy | INVITE_WITNESS_AUTH_POLICY  | accept-all |

The type of authorization to use when a 'Invite' witness ActivityPub request is received. Possible values are: 'accept-all' and 'accept-list'. The value, 'accept-all', indicates that this server will accept any 'Invite' request for a witness. The value, 'accept-list', indicates that the service sending the 'Invite' witness request must be included in an 'accept list'. Defaults to 'accept-all' if not set.

### http-timeout

| Arg            | Env           | Default |
|----------------|---------------|----------|
| --http-timeout | HTTP_TIMEOUT  | 20s     |

The timeout for http requests. For example, '30s' for a 30 second timeout.

### http-dial-timeout

| Arg                 | Env                | Default |
|---------------------|--------------------|---------|
| --http-dial-timeout | HTTP_DIAL_TIMEOUT  | 2s      |

The timeout for HTTP dial. For example, '30s' for a 30 second timeout.

### sync-interval

| Arg             | Env                         | Default |
|-----------------|-----------------------------|---------|
| --sync-interval | ANCHOR_EVENT_SYNC_INTERVAL  | 1m      |

The interval in which anchor events are synchronized with other services that this service is following. Defaults to 1m if not set.

### sync-min-activity-age

| Arg                     | Env                                 | Default |
|-------------------------|-------------------------------------|---------|
| --sync-min-activity-age | ANCHOR_EVENT_SYNC_MIN_ACTIVITY_AGE  | 1m      |

The minimum age of an anchor activity to be synchronized. The activity will be processed only if its age is
greater than this value. Defaults to 1m if not set.

### apclient-cache-size

| Arg                   | Env                            | Default |
|-----------------------|--------------------------------|---------|
| --apclient-cache-size | ACTIVITYPUB_CLIENT_CACHE_SIZE  | 100     |

The maximum size of an ActivityPub service and public key cache.

### apclient-cache-Expiration

| Arg                         | Env                                  | Default |
|-----------------------------|--------------------------------------|---------|
| --apclient-cache-Expiration | ACTIVITYPUB_CLIENT_CACHE_EXPIRATION  | 1h      |

The expiration time of an ActivityPub service and public key cache.

### apiri-cache-size

| Arg                | Env                         | Default |
|--------------------|-----------------------------|---------|
| --apiri-cache-size | ACTIVITYPUB_IRI_CACHE_SIZE  | 100     |

The maximum size of an ActivityPub actor IRI cache.

### apiri-cache-Expiration

| Arg                      | Env                               | Default |
|--------------------------|-----------------------------------|---------|
| --apiri-cache-Expiration | ACTIVITYPUB_IRI_CACHE_EXPIRATION  | 1m      |

The expiration time of an ActivityPub actor IRI cache.

### server-idle-timeout

| Arg                   | Env                  | Default |
|-----------------------|----------------------|---------|
| --server-idle-timeout | SERVER_IDLE_TIMEOUT  | 20s     |

The idle timeout for the HTTP server. For example, '30s' for a 30 second timeout.

### witness-policy-cache-expiration

| Arg                               | Env                              | Default |
|-----------------------------------|----------------------------------|---------|
| --witness-policy-cache-expiration | WITNESS_POLICY_CACHE_EXPIRATION  | 30s     |

The expiration time(period) of witness policy cache. Default value is 30s.

### anchor-data-uri-media-type

| Arg                          | Env                        | Default          |
|------------------------------|----------------------------|------------------|
| --anchor-data-uri-media-type | ANCHOR_DATA_URI_MEDIA_TYPE | application/json |

The media type for data URIs in an anchor Linkset. Possible values are 'application/json' and
'application/gzip;base64'. If 'application/json' is specified then the content of the data URIs
in the anchor Linkset are encoded as an escaped JSON string. If 'application/gzip;base64' is
specified then the content is compressed with gzip and base64 encoded.

### sidetree-protocol-versions

| Arg                          | Env                        | Default |
|------------------------------|----------------------------|---------|
| --sidetree-protocol-versions | SIDETREE_PROTOCOL_VERSIONS | 1.0     |

Comma-separated list of Sidetree protocol versions.

### current-sidetree-protocol-version

| Arg                                 | Env                               | Default |
|-------------------------------------|-----------------------------------|---------|
| --current-sidetree-protocol-version | CURRENT_SIDETREE_PROTOCOL_VERSION |         |

One of available Sidetree protocol versions. Defaults to latest Sidetree protocol version.
