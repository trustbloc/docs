# ActivityPub

Orb implements the [ActivityPub](https://www.w3.org/TR/activitypub/) spec for server to server communication.
Communication is based on posting an activity (JSON document) to the server's local outbox which then gets delivered
to one or more server inboxes. [HTTP signatures](authorization.html#http-signatures) are used for authentication
and authorization.

## Outbox/Inbox

Inter-server communication is performed by posting an Activity to the Orb server's Outbox. The
Outbox handler delivers the activity to one or more targets by resolving the URIs of the target inboxes
(from the "to" field of the activity) and posting the activity to each of the URIs. The target Inbox authorizes
the message and invokes the appropriate handler. 

```{image} ../_static/orb/ap-outbox-inbox.svg

```

### Outbox

Messages are published to the AMQP _orb.activity.outbox_ queue and are handled asynchronously by a single instance
in the Orb domain. A message contains two fields:
1) Type: One of 'broadcast', 'deliver', or 'resolve-and-deliver'
2) Activity: The posted ActivityPub activity

When an activity is posted to the Outbox, it is first validated and then a message of type, 'broadcast', is published
to the AMQP _orb.activity.outbox_ queue. The message is handled by a single instance in the Orb domain.

#### Broadcast Message Handler

The 'broadcast' message handler performs the following steps:
- Stores the activity (which is contained in the message) to the local *outbox* database.
- Invokes an activity handler (this handler may include additional steps, depending on the type of activity).
- Resolves the inboxes of the URIs in the "to" field of the activity. This may involve retrieving URIs from the
  _followers_ or _witnesses_ collections. The ActivityPub
  [service](https://trustbloc.github.io/activityanchors/#actor-discovery) (actor) of each recipient URI is resolved via
  WebFinger (see [Discovery](discovery.html#Discovery)) and a result containing the resolved URI (and potentially
  an error) is returned for each resolved URI.

Each result returned from the _Outbox Resolver_ contains a URI and potentially an error. For each result that does not
contain an error, a message of type, 'deliver', is published to the _orb.activity.outbox_ queue with the URI from the
result. For each result containing an error, a message of type, 'resolve-and-deliver', is published to the _outbox queue_
so that the URI may be retried.

#### Deliver Message Handler

The 'deliver' handler posts the activity to the inbox URI (contained in the message). (Note that the appropriate
[HTTP signatures](authorization.html#http-signatures) are added to the HTTP request.) If a transient error occurs
(such as HTTP 500) then the message is NACK'ed and the 'deliver' message will be retried (according to the
[Pub/Sub redelivery mechanism](pubsub.html#message-redelivery)).

#### Resolve-and-Deliver Message Handler

The 'resolve-and-deliver' handler resolves the inboxes of the URI contained in the message. Each result returned from
the _Outbox Resolver_ contains a URI and potentially an error. For each result that does not contain
an error, a 'deliver' message is published to the _orb.activity.outbox_ queue with the URI from the result.
If the result contains a transient error (e.g. HTTP 500) then the message is NACK'ed and the 'resolve-and-deliver'
message is retried (according to the [Pub/Sub redelivery mechanism](pubsub.html#message-redelivery)).
If a persistent error occurs (e.g. 400) then the URI is skipped.

### Inbox

The [inbox](restendpoints/activitypub.html#inbox) is a REST endpoint in Orb which accepts activities. When an activity
is received, the HTTP signature in the header of the request is [verified](authorization.html#signature-verification)
using the public key of the [actor](https://trustbloc.github.io/activityanchors/#actor-discovery) that sent the activity.
After the actor is authenticated, a message is posted to the _orb.activity.inbox_ queue and is processed by one of the
server instances in the domain.

The inbox handler first stores the activity in the 'inbox' database and authorizes the activity. (Each activity
could have different authorization criteria which are explained in each of the activities below.) The appropriate
activity handler is then invoked.

## Activities

### Follow

A [Follow](https://trustbloc.github.io/activityanchors/#follow-activity) activity is posted by a server to another server
so that activities may be synchronized between them. For example, domain1 posts a Follow activity to domain2 indicating
that it wants notifications of objects created by domain2 (via the
[Create](https://trustbloc.github.io/activityanchors/#create-activity) activity). Domain1 is also notified of any objects
created by servers that domain2 is following via the
[Announce](https://trustbloc.github.io/activityanchors/#announce-activity) activity. When a server receives a
Follow activity in its inbox, it authorizes the 'actor' (which is the originating server) in the Follow request using a
configurable [Follow Authorization Policy](#follow-authorization-policy). If authorized, the actor is added to the list
of _followers_ and an [Accept](https://trustbloc.github.io/activityanchors/#accept-follow-activity) activity is posted
to the actor's inbox. When an [Accept](https://trustbloc.github.io/activityanchors/#accept-follow-activity) activity is
received in the inbox, the activity is first validated against a previously posted Follow activity and then the 'actor'
in the Accept activity (which was the target server in Follow) is added to the _following_ collection.

```{image} ../_static/orb/ap-follow-accept.svg

```

If the actor posting the Follow activity is not authorized then a
[Reject](https://trustbloc.github.io/activityanchors/#reject-follow-activity) activity is posted to the actor's inbox.
A [Reject](https://trustbloc.github.io/activityanchors/#reject-follow-activity) of a Follow activity simply logs the
fact that the request was rejected.

```{image} ../_static/orb/ap-follow-reject.svg

```

### Undo Follow

An [Undo](https://trustbloc.github.io/activityanchors/#undo-follow-activity) activity is posted to 'unfollow' a
server. The Undo activity is posted to the server to which the previous Follow activity was posted and the target server
is removed from the _following_ list. The target server handles the Undo activity by removing the originating server from
the _followers_ list.

### Invite Witness

An Invite activity is posted to another server in order to invite that server to be a witness of
[Anchor Events](https://trustbloc.github.io/activityanchors/#anchorevent). For example, domain1 posts an Invite activity
to domain2 indicating that it wants domain2 to be a witness of Anchor Events produced by domain1 (using the
[Offer](#offer) activity).

When a server receives an Invite witness activity in its inbox, it authorizes the actor (which is the originating
server) in the Invite request using an [Invite Witness Authorization Policy](#invite-witness-authorization-policy). If
authorized, the actor is added to the _witnessing_ collection and an
[Accept](https://trustbloc.github.io/activityanchors/#accept-invite-witness-activity) activity is posted to the actor's
inbox. When an [Accept](https://trustbloc.github.io/activityanchors/#accept-invite-witness-activity) activity is received
in the inbox, the activity is first validated against a previously posted Invite witness activity and then the 'actor' in
the Accept activity is added to the _witnesses_ collection.

```{image} ../_static/orb/ap-invite-accept.svg

```

If the actor is not authorized, then a [Reject](https://trustbloc.github.io/activityanchors/#reject-invite-witness-activity)
activity is posted to the actor's inbox. A [Reject](https://trustbloc.github.io/activityanchors/#reject-invite-witness-activity)
of an Invite witness activity simply logs the fact that the request was rejected.

```{image} ../_static/orb/ap-invite-reject.svg

```

### Undo Invite Witness

An [Undo](https://trustbloc.github.io/activityanchors/#undo-invite-witness-activity) activity is posted to remove a
server as a witness. The Undo activity is posted to the server to which the previous Invite activity was posted and the
target server is removed from the _witnesses_ list. The target server handles the Undo activity by removing the originating
server from the _witnessing_ list.

### Offer

An [Offer](https://trustbloc.github.io/activityanchors/#offer-activity) activity is posted to one or more servers that are
contained in the _witnesses_ collection in order to collect proofs. (The selection of witnesses is dictated by a
[Witness Policy](witnesspolicy.html#witness-policy) which determines the minimum number of proofs required.) When a server
receives an Offer activity in its inbox, the request is first authorized by ensuring that the actor of the Offer is in the
_witnessing_ collection. Once authorized, the [Anchor Object](https://trustbloc.github.io/activityanchors/#anchorevent)
which is embedded in the Offer activity is added to the ledger (VCT) and an
[Accept](https://trustbloc.github.io/activityanchors/#accept-anchor-activity) anchor activity (which contains the proof)
is posted back to the originator of the offer. When an [Accept](https://trustbloc.github.io/activityanchors/#accept-anchor-activity)
anchor activity is received in the inbox, the activity is first validated against a previously posted Offer activity
and then the embedded proof for the [Anchor Object](https://trustbloc.github.io/activityanchors/#anchorevent) is
added to the collection of existing proofs. Once a sufficient number of proofs is received (according to the
[Witness Policy](witnesspolicy.html#witness-policy)) then a complete Anchor Event (containing all of the proofs) is
constructed and the Anchor Event is posted to the queue, _orb.anchor_event_, to be processed by the
[Batch Writer](batchwriter.html#batch-writer).

```{image} ../_static/orb/ap-offer-accept.svg

```

If the actor of the Offer is not in the _witnessing_ collection then a
[Reject](https://trustbloc.github.io/activityanchors/#reject-anchor-activity) activity is posted to the originating actor.
The [Reject](https://trustbloc.github.io/activityanchors/#reject-anchor-activity) of an Offer activity simply logs the
fact that the request was rejected.

```{image} ../_static/orb/ap-offer-reject.svg

```

### Create/Announce

A [Create](https://trustbloc.github.io/activityanchors/#create-activity) activity is posted by the
[Batch Writer](batchwriter.html#batch-writer) to one or more servers that are contained in the *followers* collection.

When a server receives a [Create](https://trustbloc.github.io/activityanchors/#create-activity) activity in its
inbox the [Anchor Event](https://trustbloc.github.io/activityanchors/#anchorevent) which is embedded in the Create
activity is posted to the *orb.anchor* queue so that it may be processed by the [Observer](observer.html#observer).
After the Observer processes the AnchorEvent, it posts a [Like](https://trustbloc.github.io/activityanchors/#like-activity)
activity back to the "actor" of the Create activity. The Create activity is also forwarded to the servers in the
_followers_ collection using the [Announce](https://trustbloc.github.io/activityanchors/#announce-activity) activity.

When a server receives an [Announce](https://trustbloc.github.io/activityanchors/#announce-activity) activity in its
inbox the [Anchor Event](https://trustbloc.github.io/activityanchors/anchorevent) which is embedded in the
Announce activity is posted to the *orb.anchor* queue so that it may be processed by the [Observer](observer.html#observer).
After the Observer processes the AnchorEvent, it posts a [Like](https://trustbloc.github.io/activityanchors/#like-activity)
activity back to the "actor" of the Announce activity as well as to the originator of the AnchorEvent (which is determined
from the "attributed-to" field of the [AnchorEvent](https://trustbloc.github.io/activityanchors/#anchorevent)).

```{image} ../_static/orb/ap-create-announce.svg

```

## Like

A [Like](https://trustbloc.github.io/activityanchors/#like-activity) activity is posted by the Observer after having
processed an AnchorEvent. The Like activity includes a "url" field which is a
[hashlink](https://datatracker.ietf.org/doc/html/draft-sporny-hashlink). This hashlink contains metadata of one or
more locations where the anchor is replicated. When a server receives the Like activity in its inbox, it adds the
additional URIs to the _anchor link_ database.

## Undo Like

An [Undo](https://trustbloc.github.io/activityanchors/#undo-like-activity) activity is posted to inform other servers
that it no longer replicates an anchor. The Undo activity is posted to the server to which the previous Like
activity was posted. The target server handles the Undo activity by removing the link from the _anchor link_ database.

## Authorization Policy

Authorization policies are applied when handling the Follow and Invite activities. If authorization fails then a Reject
activity is posted to the originating server.

### Follow Authorization Policy

An authorization policy may be configured for the [Follow](#follow) activity using the configuration parameters,
[follow-auth-policy](parameters.html#follow-auth-policy). The possible values are  _accept-all_ and _accept-list_. If
accept-all
is used then all Follow requests are accepted. If accept-list is used then the actor in the Follow activity must be in the
[accept-list](#accept-list) database.

### Invite Witness Authorization Policy

An authorization policy may be configured for the [Invite Witness](#invite-witness) activity using the configuration
parameters, [invite-witness-auth-policy](parameters.html#invite-witness-auth-policy). The possible values are  _accept-all_
and _accept-list_. If accept-all is used then all Invite witness requests are accepted. If accept-list is used then the
actor in the Invite activity must be in the [accept-list](#accept-list) database.

## Accept List

An accept-list is simply a database of server URLs that are authorized for a particular type of operation. The
two types of operations are _follow_ and _invite-witness_. If the actor URL is in the _follow_ accept list then a
Follow request from that server is authorized. If the actor URL is in the _invite-witness_ accept list then an Invite
witness request from that server is authorized.

The accept-list is managed via the REST endpoint [/services/orb/acceptlist](restendpoints/activitypub.html#accept-list)
