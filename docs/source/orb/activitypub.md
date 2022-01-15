# ActivityPub

Orb implements the ActivityPub spec for server to server communication. Communication is based on posting an
activity (JSON document) to the server's local outbox which then gets delivered to one or more server inboxes.
HTTP signatures are used for authentication.

## Outbox/Inbox

When an activity is posted to the local server's outbox, it is first stored in the local *outbox* database and then
posted to one or more recipients as specified in the Activity's "to" field. The ActivityPub
[service](https://trustbloc.github.io/activityanchors/#actor-discovery) (actor) of each recipient URI is resolved via
WebFinger (see [Discovery](discovery.html#Discovery)). The activity is then posted to the "inbox" URL that is specified
in the service document. The post to each of the URLs is performed asynchronously using an AMQP message queue. The
message is published to the _orb.activity.outbox_ queue and then one of the servers in the domain processes it by 
sending the HTTP request. Before sending the request, an HTTP signature is added to the request header which may be 
verified by the recipient using the [public key](https://trustbloc.github.io/activityanchors/#actor-discovery) of 
the local service.

The Inbox is a REST endpoint in Orb which accepts activities. When an activity is received, the HTTP signature in the
header of the request is verified using the public key of the
[actor](https://trustbloc.github.io/activityanchors/#actor-discovery) that sent the activity. After the actor is
authenticated, a message is posted to the _orb.activity.inbox_ queue and is processed by one of the server instances in
the domain.

The inbox handler first stores the activity in the 'inbox' database and then authorizes the activity. Each activity
could have different authorization criteria which are explained in each of the activities below.

```{image} ../_static/orb/ap-outbox-inbox.svg

```

## Activities

### Follow

A [Follow](https://trustbloc.github.io/activityanchors/#follow-activity) activity is posted by a server to other server
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
[Offer](#Offer) activity).

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
Announce activity is posted to the *orb.anchor* queue so that it may be processed by the [Observer](observer.htmlobserver).
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
[FOLLOW_AUTH_POLICY](parameters.html#follow-auth-policy). The possible values are  _accept-all_ and _accept-list_. If 
accept-all
is used then all Follow requests are accepted. If accept-list is used then the actor in the Follow activity must be in the
[accept-list](#accept-list) database.

### Invite Witness Authorization Policy

An authorization policy may be configured for the [Invite Witness](#invite-witness) activity using the configuration
parameters, [INVITE_WITNESS_AUTH_POLICY](parameters.html#invite-witness-auth-policy). The possible values are  _accept-all_
and _accept-list_. If accept-all is used then all Invite witness requests are accepted. If accept-list is used then the
actor in the Invite activity must be in the [accept-list](#accept-list) database.

## Accept List

An accept-list is simply a database of server URLs that are authorized for a particular type of operation. The
two types of operations are _follow_ and _invite-witness_. If the actor URL is in the _follow_ accept list then a
Follow request from that server is authorized. If the actor URL is in the _invite-witness_ accept list then an Invite
witness request from that server is authorized.

The accept-list is managed via the REST endpoint [/services/orb/acceptlist](restendpoints.html#accept-list)
