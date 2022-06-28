# System Endpoints

Various endpoints are defined to configure Orb and to retrieve system information, such as Orb version and metrics.

## Accept List

**Endpoint:** /services/orb/acceptlist

An [accept list](../system/activitypub.html#accept-list) is a database of server URLs that are authorized for a particular type of operation.

### GET

The accept-list is retrieved using a GET request to this endpoint.

**Example**

Request:

```
GET /services/orb/acceptlist HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json
Accept-Encoding: gzip, deflate
```

Response contains the accept-list for both _follow_ and _invite-witness_:

```json
[
  {
    "type": "follow",
    "url": [
      "https://orb.domain2.com/services/orb",
      "https://orb.domain3.com/services/orb"
    ]
  },
  {
    "type": "invite-witness",
    "url": [
      "https://orb.domain2.com/services/orb",
      "https://orb.domain3.com/services/orb"
    ]
  }
]
```

### POST

The accept-list is updated using a POST request to this endpoint. Services may
be added and removed from the accept-list for _Follow_ and _Invite_ witness activities.

**Example**

Request to add domain2 and domain3 to the _follow_ and _invite-witness_ accept-list as well as remove domain4
from the _follow_ accept-list:

```
POST /services/orb/acceptlist HTTP/1.1
Host: orb.domain1.com
Content-Type: application/ld+json"
Accept-Encoding: gzip, deflate

[
  {
    "add": [
      "https://orb.domain2.com/services/orb",
      "https://orb.domain3.com/services/orb"
    ],
    "remove": [
      "https://orb.domain4.com/services/orb",
    ],
    "type": "follow"
  },
  {
    "add": [
      "https://orb.domain2.com/services/orb",
      "https://orb.domain3.com/services/orb"
    ],
    "type": "invite-witness"
  }
]
```

## Allowed Anchor Origins

**Endpoint:** /allowedorigins

An allowed anchor origins is a database of server URIs that are authorized for DID create operations.

### GET

The allowed anchor origins are retrieved using a GET request to this endpoint.

**Example**

Request:

```
GET /allowedorigins HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json
```

Response contains the anchor origins:

```json
[
  "https://orb.domain1.com",
  "https://orb.domain2.com"
]
```

### POST

The allowed anchor origins are updated using a POST request to this endpoint. URIs may be added and removed.

**Example**

Request to add domain3 and domain4, and remove domain2 from the allowed anchor origins list:

```
POST /allowedorigins HTTP/1.1
Host: orb.domain1.com
Content-Type: application/json"

{
  "add": [
    "https://orb.domain3.com",
    "https://orb.domain4.com"
  ],
  "remove": [
    "https://orb.domain2.com"
  ]
}
```

## Node Info Endpoints

The NodeInfo endpoints provide general information about an Orb server, including the version, the number
of posts ([Create](https://trustbloc.github.io/activityanchors/#create-activity) activities) and the number of
comments ([Like](https://trustbloc.github.io/activityanchors/#like-activity) activities). Two versions of NodeInfo
are supported:
[v2.0](https://nodeinfo.diaspora.software/docson/index.html#/ns/schema/2.0#$$expand) and
[v2.1](https://nodeinfo.diaspora.software/docson/index.html#/ns/schema/2.1#$$expand).

### Node Info version 2.0

**Endpoint:** /nodeinfo/2.0

### GET

**Example**

Request:

```
GET /nodeinfo/2.0 HTTP/1.1
Host: orb.domain1.com
Accept: application/json
Accept-Encoding: gzip, deflate
```

Response:

```json
{
  "version": "2.0",
  "software": {
    "name": "Orb",
    "version": "0.1.2"
  },
  "protocols": [
    "activitypub"
  ],
  "services": {
    "inbound": [],
    "outbound": []
  },
  "openRegistrations": false,
  "usage": {
    "users": {
      "total": 1
    },
    "localPosts": 4,
    "localComments": 6
  }
}
```

### Node Info version 2.1

**Endpoint:** /nodeinfo/2.1

### GET

**Example**

Request:

```
GET /nodeinfo/2.1 HTTP/1.1
Host: orb.domain1.com
Accept: application/json
Accept-Encoding: gzip, deflate
```

Response:

```json
{
  "version": "2.1",
  "software": {
    "name": "Orb",
    "version": "0.1.2",
    "repository": "https://github.com/trustbloc/orb"
  },
  "protocols": [
    "activitypub"
  ],
  "services": {
    "inbound": [],
    "outbound": []
  },
  "openRegistrations": false,
  "usage": {
    "users": {
      "total": 1
    },
    "localPosts": 4,
    "localComments": 6
  }
}
```

## Metrics Endpoint

**Endpoint:** /metrics

### GET

This endpoint retrieves metrics data that may be consumed by a [prometheus](https://prometheus.io/) server.

Request:

```
GET /metrics HTTP/1.1
Host: orb.domain1.com
Accept: text/plain
Accept-Encoding: gzip, deflate
```

Response:

```
# HELP go_gc_duration_seconds A summary of the pause duration of garbage collection cycles.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 6.49e-05
go_gc_duration_seconds{quantile="0.25"} 0.0001778
go_gc_duration_seconds{quantile="0.5"} 0.0002633
go_gc_duration_seconds{quantile="0.75"} 0.0004953
go_gc_duration_seconds{quantile="1"} 0.0111963
go_gc_duration_seconds_sum 0.1513972
go_gc_duration_seconds_count 248
# HELP go_goroutines Number of goroutines that currently exist.
# TYPE go_goroutines gauge
go_goroutines 104
# HELP orb_activitypub_inbox_handler_seconds The time (in seconds) that it takes to handle an activity posted to the inbox.
# TYPE orb_activitypub_inbox_handler_seconds histogram
orb_activitypub_inbox_handler_seconds_bucket{type="Accept",le="0.005"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Accept",le="0.01"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Accept",le="0.025"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Accept",le="0.05"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Accept",le="0.1"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Accept",le="0.25"} 1
orb_activitypub_inbox_handler_seconds_bucket{type="Accept",le="0.5"} 1
orb_activitypub_inbox_handler_seconds_bucket{type="Accept",le="1"} 1
orb_activitypub_inbox_handler_seconds_bucket{type="Accept",le="2.5"} 1
orb_activitypub_inbox_handler_seconds_bucket{type="Accept",le="5"} 1
orb_activitypub_inbox_handler_seconds_bucket{type="Accept",le="10"} 1
orb_activitypub_inbox_handler_seconds_bucket{type="Accept",le="+Inf"} 1
orb_activitypub_inbox_handler_seconds_sum{type="Accept"} 0.1140751
orb_activitypub_inbox_handler_seconds_count{type="Accept"} 1
orb_activitypub_inbox_handler_seconds_bucket{type="Announce",le="0.005"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Announce",le="0.01"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Announce",le="0.025"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Announce",le="0.05"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Announce",le="0.1"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Announce",le="0.25"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Announce",le="0.5"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Announce",le="1"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Announce",le="2.5"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Announce",le="5"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Announce",le="10"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Announce",le="+Inf"} 0
orb_activitypub_inbox_handler_seconds_sum{type="Announce"} 0
orb_activitypub_inbox_handler_seconds_count{type="Announce"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Create",le="0.005"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Create",le="0.01"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Create",le="0.025"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Create",le="0.05"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Create",le="0.1"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Create",le="0.25"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Create",le="0.5"} 1
orb_activitypub_inbox_handler_seconds_bucket{type="Create",le="1"} 1
orb_activitypub_inbox_handler_seconds_bucket{type="Create",le="2.5"} 1
orb_activitypub_inbox_handler_seconds_bucket{type="Create",le="5"} 1
orb_activitypub_inbox_handler_seconds_bucket{type="Create",le="10"} 1
orb_activitypub_inbox_handler_seconds_bucket{type="Create",le="+Inf"} 1
orb_activitypub_inbox_handler_seconds_sum{type="Create"} 0.2653678
orb_activitypub_inbox_handler_seconds_count{type="Create"} 1
orb_activitypub_inbox_handler_seconds_bucket{type="Follow",le="0.005"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Follow",le="0.01"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Follow",le="0.025"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Follow",le="0.05"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Follow",le="0.1"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Follow",le="0.25"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Follow",le="0.5"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Follow",le="1"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Follow",le="2.5"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Follow",le="5"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Follow",le="10"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="Follow",le="+Inf"} 0
orb_activitypub_inbox_handler_seconds_sum{type="Follow"} 0
orb_activitypub_inbox_handler_seconds_count{type="Follow"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="InviteWitness",le="0.005"} 0
orb_activitypub_inbox_handler_seconds_bucket{type="InviteWitness",le="0.01"} 0
. . .
```


## Health Check Endpoint

**Endpoint:** /healthcheck

The health check endpoint performs a check on various subsystems. If the health check fails then an HTTP status,
_503: Service Unavailable_, is returned along with details of which component(s) failed. An HTTP status, _200: OK_, is
returned when Orb is ready to receive requests. Following are the returned statuses:

- **mqStatus** - [Message Broker](../system/pubsub.html#amqp-publisher-subscriber)
  - _success_ - The message broker is connected.
  - _not connected_ = The message broker is not connected.
- **dbStatus** - Database - Contains "success" or an error message.
  - _success_ - a Database 'ping' has succeeded.
  - _error message_ - The error message received from the database 'ping'.
- **kmsStatus** - [Key Management Service](../../kms/index.html) - Contains "success" or an error message.
    - _success_ - The KMS health check succeeded.
    - _error message_ - The error message received from the KMS health check.
- **vctStatus** - [Verifiable Credential Transparency](../vct/index.html)
  - _success_ - VCT health check succeeded.
  - _disabled_ - VCT is disabled for the Orb domain. (This status is not considered to be a failed status.)
  - _log endpoint not configured_ - VCT is enabled but the log endpoint has not yet been [configured](../restendpoints/log.html#post). (This status is not considered to be a failed status.)
  - _error message_ - The error message returned from the VCT health check.

### GET

**Example**

Request:

```
GET /healthcheck HTTP/1.1
Host: orb.domain1.com
Accept: application/json
```

Response:

```json
{
  "mqStatus": "success",
  "vctStatus": "disabled",
  "dbStatus": "success",
  "kmsStatus": "success",
  "currentTime": "2022-06-24T20:10:43.0115373Z"
}
```
