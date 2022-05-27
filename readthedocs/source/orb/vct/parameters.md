# Startup Parameters

This section enumerates the startup parameters for an VCT server. Parameters in the
[Required Parameters](#required-parameters) section are required, otherwise the server will not start.
Parameters in the [Optional Parameters](#optional-parameters) section are optional and will use a
default value if not specified.

## Required Parameters

Following are the required parameters for a VCT server.

### api-host

| Arg        | Env          |
|------------|--------------|
| --api-host | VCT_API_HOST |

URL to run the VCT instance on. Format: HostName:Port.

### base-url

| Arg        | Env          |
|------------|--------------|
| --base-url | VCT_BASE_URL |

Base URL. e.g (https://vct.com)

### kms-type

| Arg        | Env          | Default |
|------------|--------------|---------|
| --kms-type | VCT_KMS_Type |         |

KMS type (local,web,aws).

### kms-endpoint

| Arg            | Env              | Default |
|----------------|------------------|---------|
| --kms-endpoint | VCT_KMS_ENDPOINT |         |

Remote KMS URL.


### log-active-key-id

| Arg                 | Env                        | Default |
|---------------------|----------------------------|---------|
| --log-active-key-id | VCT_LOG_SIGN_ACTIVE_KEY_ID |         |

Active Key ID for signing logs.

### logs

| Arg    | Env      |
|--------|----------|
| --logs | VCT_LOGS |

A list of Trillian logs (comma separated). Format must be <alias>:<permission>@<endpoint>.

Examples: maple2021:rw@server.com,maple2020:r@server.com:9890

### issuers

| Arg       | Env         | Default |
|-----------|-------------|---------|
| --issuers | VCT_ISSUERS |         |

Comma-Separated list of supported issuers.

Examples:
maple2021@did:key:zUC724vuGvHpnCGFG1qqpXb81SiBLu3KLSqVzenwEZNPoY35i2Bscb8DLaVwHvRFs6F2NkNNXRcPWvqnPDUd9ukdjLkjZd3u9zzL4wDZDUpkPAatLDGLEYVo8kkAzuAKJQMr7N2,
maple2020@did:key:zUC724vuGvHpnCGFG1qqpXb81SiBLu3KLSqVzenwEZNPoY35i2Bscb8DLaVwHvRFs6F2NkNNXRcPWvqnPDUd9ukdjLkjZd3u9zzL4wDZDUpkPAatLDGLEYVo8kkAzuAKJQMr7N7

### trillian-db-conn

| Arg                | Env                  |
|--------------------|----------------------|
| --trillian-db-conn | VCT_TRILLIAN_DB_CONN |

Trillian db conn.

Example: user=postgres host=trillian.postgres password=password dbname=test port=5432 sslmode=disable

### dsn

| Arg   | Env     |
|-------|---------|
| --dsn | VCT_DSN |

Datasource Name with credentials if required. Format must be <driver>:[//]<driver-specific-dsn>.
Supported drivers are [mem, couchdb, postgres, mongodb].

Examples:
'postgres://jack:secret@pg.example.com:5432/mydb',
'mem://test',
'mongodb://mongodb.example.com:27017'

### timeout

| Arg         | Env         |
|-------------|-------------|
| --timeout   | VCT_TIMEOUT |

Total time in seconds to wait until the services are available before giving up.

### sync-timeout

| Arg            | Env              |
|----------------|------------------|
| --sync-timeout | VCT_SYNC_TIMEOUT |

Total time in seconds to resolve config values.

### tls-systemcertpool

| Arg                  | Env                    | Default |
|----------------------|------------------------|---------|
| --tls-systemcertpool | VCT_TLS_SYSTEMCERTPOOL | false   |

Use system certificate pool. Possible values true and false. Defaults to false if not set.

### tls-cacerts

| Arg           | Env             | Default |
|---------------|-----------------|---------|
| --tls-cacerts | VCT_TLS_CACERTS |         |

Comma-Separated list of ca certs path.

### tls-serve-cert

| Arg              | Env                | Default |
|------------------|--------------------|---------|
| --tls-serve-cert | VCT_TLS_SERVE_CERT |         |

TLS certificate for VCT server. Path to the server certificate to use when serving HTTPS.

### tls-serve-key

| Arg             | Env               | Default |
|-----------------|-------------------|---------|
| --tls-serve-key | VCT_TLS_SERVE_KEY |         |

TLS key for VCT server. Path to the private key to use when serving HTTPS.

### context-provider-url

| Arg                    | Env                      | Default |
|------------------------|--------------------------|---------|
| --context-provider-url | VCT_CONTEXT_PROVIDER_URL |         |

Comma-separated list of remote context provider URLs to get JSON-LD contexts from.


## Optional Parameters

Below are the optional parameters for an VCT server. If not specified then the default value is used.

### api-read-token

| Arg                | Env                | Default |
|--------------------|--------------------|---------|
| --api-read-token   | VCT_API_READ_TOKEN |         |

Check for bearer token in the authorization header (optional).

### api-write-token

| Arg               | Env                 | Default |
|-------------------|---------------------|---------|
| --api-write-token | VCT_API_WRITE_TOKEN |         |

Check for bearer token in the authorization header (optional).

### dev-mode

| Arg        | Env          | Default |
|------------|--------------|---------|
| --dev-mode | VCT_DEV_MODE | false   |


### database-prefix

| Arg               | Env                 | Default |
|-------------------|---------------------|---------|
| --database-prefix | VCT_DATABASE_PREFIX |         |

An optional prefix to be used when creating and retrieving underlying databases. This allows
a database to be shared by multiple VCT domains. (Mainly used in development environments.)

### host-metrics-url

| Arg            | Env              | Default |
|----------------|------------------|---------|
| --metrics-host | VCT_METRICS_HOST |         |

URL that exposes the metrics endpoint. Format: HostName:Port.