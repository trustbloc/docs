# Startup Parameters

## ORB_HOST_URL

URL to run the orb-server instance on. Format: HostName:Port.

## ORB_HOST_METRICS_URL

URL that exposes the metrics endpoint. Format: HostName:Port.

## ORB_EXTERNAL_ENDPOINT

External endpoint that clients use to invoke services. This endpoint is used to generate IDs of anchor credentials and ActivityPub objects and should be resolvable by external clients. Format: HostName[:Port].

## ORB_SYNC_TIMEOUT

Total time in seconds to resolve config values.

## ORB_VCT_URL

Verifiable credential transparency URL.

## VCT_MONITORING_INTERVAL

The interval in which VCTs are monitored to ensure that proofs are anchored. Defaults to 10s if not set.

## ANCHOR_STATUS_MONITORING_INTERVAL

The interval in which 'in-process' anchors are monitored to ensure that they will be witnessed(completed) as per
policy. Defaults to 5s if not set.

## ANCHOR_STATUS_IN_PROCESS_GRACE_PERIOD

The period in which witnesses will not be re-selected for 'in-process' anchors. Defaults to 10s if not set.

## ORB_KMS_STORE_ENDPOINT

KMS storage URL.

## ORB_KMS_ENDPOINT

Remote KMS URL.

## ORB_ACTIVE_KEY_ID

Active Key ID (ED25519Type).

## ORB_PRIVATE_KEYS

Private Keys base64 (ED25519Type). For example,  key1=privatekeyBase64Value,key2=privatekeyBase64Value

## ORB_SECRET_LOCK_KEY_PATH

The path to the file with key to be used by local secret lock. If missing noop service lock is used.

## ORB_DISCOVERY_DOMAIN

Discovery domain for this domain. Format: HostName

## ORB_TLS_SYSTEMCERTPOOL

Use system certificate pool. Possible values true and false. Defaults to false if not set.

## ORB_TLS_CACERTS

Comma-Separated list of ca certs path.

## ORB_TLS_CERTIFICATE

TLS certificate for ORB server.

## ORB_TLS_KEY

TLS key for ORB server.

## DID_NAMESPACE

DID Namespace.

## DID_ALIASES

Aliases for this did method.

## CAS_TYPE

The type of the Content Addressable Storage (CAS). Supported options: local, ipfs.

## IPFS_URL

Enables IPFS support. If set, this Orb server will use the node at the given URL. To use the public ipfs.io node, set this to https://ipfs.io (or http://ipfs.io). If using ipfs.io, then the CAS type flag must be set to local since the ipfs.io node is read-only. If the URL doesn't include a scheme, then HTTP will be used by default.

## REPLICATE_LOCAL_CAS_WRITES_IN_IPFS

If enabled, writes to the local CAS will also be replicated in IPFS. This setting only takes effect if this server has both a local CAS and IPFS enabled. If the IPFS node is set to ipfs.io, then this setting will be disabled since ipfs.io does not support writes. Supported options: false, true. Defaults to false if not set.

## MQ_URL

The URL of the message broker.

## MQ_OP_POOL

The size of the operation queue subscriber pool. If <=1 then a pool will not be created.

## MQ_OBSERVER_POOL

The size of the observer queue subscriber pool. If not specified then the default size will be used.

## MQ_MAX_CONNECTION_SUBSCRIPTIONS

The maximum number of subscriptions per connection.

## MQ_PUBLISHER_POOL

The size of a channel pool for an AMQP publisher (default is 25). If set to 0 then a channel pool is not used and a new channel is opened/closed for every publish to a queue.

## CID_VERSION

The version of the CID format to use for generating CIDs. Supported options: 0, 1. If not set, defaults to 1.

## BATCH_WRITER_TIMEOUT

Maximum time (in millisecond) in-between cutting batches.

## DATABASE_TYPE

The type of database to use for everything except key storage. Supported options: mem, couchdb, mongodb.

## DATABASE_URL

The URL (or connection string) of the database. Not needed if using memstore. For CouchDB, include the username:password@ text if required.

## DATABASE_PREFIX

An optional prefix to be used when creating and retrieving underlying databases.

## KMSSECRETS_DATABASE_TYPE

The type of database to use for storage of KMS secrets. Supported options: mem, couchdb, mongodb.

## KMSSECRETS_DATABASE_URL

The URL (or connection string) of the database. Not needed if using memstore. For CouchDB, include the username:password@ text if required.

## KMSSECRETS_DATABASE_PREFIX

An optional prefix to be used when creating and retrieving the underlying KMS secrets database.

## DATABASE_TIMEOUT

The timeout for database requests. For example, '30s' for a 30 second timeout. Currently this setting only applies if you're using MongoDB.

## ANCHOR_CREDENTIAL_ISSUER

Anchor credential issuer (required).

## ANCHOR_CREDENTIAL_URL

Anchor credential url (required).

## ANCHOR_CREDENTIAL_SIGNATURE_SUITE

Anchor credential signature suite (required).

## ANCHOR_CREDENTIAL_DOMAIN

Anchor credential domain (required).

## ALLOWED_ORIGINS

Allowed origins for this did method.

## MAX_WITNESS_DELAY

Maximum witness response time (in seconds).

## SIGN_WITH_LOCAL_WITNESS

Always sign with local witness flag (default true).

## DISCOVERY_DOMAINS

Discovery domains.

## DISCOVERY_VCT_DOMAINS

Discovery VCT domains.

## DISCOVERY_MINIMUM_RESOLVERS

Discovery minimum resolvers number.

## HTTP_SIGNATURES_ENABLED

Set to "true" to enable HTTP signatures in ActivityPub.

## DID_DISCOVERY_ENABLED

Set to "true" to enable did discovery.

## UNPUBLISHED_OPERATION_STORE_ENABLED

Set to "true" to enable un-published operation store. Used to enable storing unpublished operations and including them when resolving documents.

## UNPUBLISHED_OPERATION_STORE_OPERATION_TYPES

Comma-separated list of operation types. Used if unpublished operation store is enabled. 
Default value is "create,update" which enables storing unpublished 'create' and 'update' operations into 
unpublished store and using those unpublished 'create' and 'update' operations for resolving document.

## INCLUDE_UNPUBLISHED_OPERATIONS_IN_METADATA

Set to "true" to include unpublished operations in metadata.

## INCLUDE_PUBLISHED_OPERATIONS_IN_METADATA

Set to "true" to include published operations in metadata.

## RESOLVE_FROM_ANCHOR_ORIGIN

Set to "true" to resolve from anchor origin.

## VERIFY_LATEST_FROM_ANCHOR_ORIGIN

Set to "true" to verify latest operations against anchor origin.

## ORB_AUTH_TOKENS_DEF

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

## ORB_AUTH_TOKENS

Specifies the actual values of the tokens defined in ORB_AUTH_TOKENS_DEF.

**Example:**

admin=ADMIN_PASSWORD,read=READ_PASSWORD

## ORB_CLIENT_AUTH_TOKENS_DEF

Follows the same rules as ORB_AUTH_TOKENS_DEF but is used by the Orb client transport to
determine whether an HTTP signature is required for an outbound HTTP request. If not specified then it is assumed
to be the same as ORB_AUTH_TOKENS_DEF.

## ORB_CLIENT_AUTH_TOKENS

Specifies the actual values of the tokens defined in ORB_CLIENT_AUTH_TOKENS_DEF. If not specified then it is assumed to be the same as ORB_AUTH_TOKENS.

## ACTIVITYPUB_PAGE_SIZE

The maximum page size for an ActivityPub collection or ordered collection.

## DEV_MODE_ENABLED

Set to "true" to enable dev mode.

## NODEINFO_REFRESH_INTERVAL

The interval for refreshing NodeInfo data. For example, '30s' for a 30 second interval.

## IPFS_TIMEOUT

The timeout for IPFS requests. For example, '30s' for a 30 second timeout.

## ORB_CONTEXT_PROVIDER_URL

Comma-separated list of remote context provider URLs to get JSON-LD contexts from."

## UNPUBLISHED_OPERATION_LIFETIME

How long unpublished operations remain stored before expiring (and thus, being deleted some time later). For example, '1m' for a 1 minute lifespan. Defaults to 1 minute if not set.

## TASK_MANAGER_CHECK_INTERVAL

How frequently to check for scheduled tasks. For example, a setting of '10s' will cause the task manager to check for outstanding tasks every 10s. Defaults to 10 seconds if not set.

## DATA_EXPIRY_CHECK_INTERVAL

How frequently to check for (and delete) any expired data. For example, a setting of '1m' will cause the expiry service to run a check every 1 minute. Defaults to 1 minute if not set.

## FOLLOW_AUTH_POLICY

The type of authorization to use when a 'Follow' ActivityPub request is received. Possible values are: 'accept-all' and 'accept-list'. The value, 'accept-all', indicates that this server will accept any 'Follow' request. The value, 'accept-list', indicates that the service sending the 'Follow' request must be included in an 'accept list'. Defaults to 'accept-all' if not set.

## INVITE_WITNESS_AUTH_POLICY

The type of authorization to use when a 'Invite' witness ActivityPub request is received. Possible values are: 'accept-all' and 'accept-list'. The value, 'accept-all', indicates that this server will accept any 'Invite' request for a witness. The value, 'accept-list', indicates that the service sending the 'Invite' witness request must be included in an 'accept list'. Defaults to 'accept-all' if not set.

## HTTP_TIMEOUT

The timeout for http requests. For example, '30s' for a 30 second timeout.

## HTTP_DIAL_TIMEOUT

The timeout for HTTP dial. For example, '30s' for a 30 second timeout.

## ANCHOR_EVENT_SYNC_INTERVAL

The interval in which anchor events are synchronized with other services that this service is following. Defaults to 1m if not set.

## ACTIVITYPUB_CLIENT_CACHE_SIZE

The maximum size of an ActivityPub service and public key cache.

## ACTIVITYPUB_CLIENT_CACHE_EXPIRATION

The expiration time of an ActivityPub service and public key cache.

## ACTIVITYPUB_IRI_CACHE_SIZE

The maximum size of an ActivityPub actor IRI cache.

## ACTIVITYPUB_IRI_CACHE_EXPIRATION

The expiration time of an ActivityPub actor IRI cache.

## SERVER_IDLE_TIMEOUT

The idle timeout for the HTTP server. For example, '30s' for a 30 second timeout.

## WITNESS_POLICY_CACHE_EXPIRATION

The expiration time(period) of witness policy cache. Default value is 30s.
