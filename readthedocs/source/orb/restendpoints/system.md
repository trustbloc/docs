# System Endpoints

Various endpoints are defined to retrieve system information, such as Orb version and metrics.

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