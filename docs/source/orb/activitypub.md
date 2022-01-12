# ActivityPub

Orb implements the ActivityPub spec for its server to server communication. Communication is based on posting an activity (JSON document) to the server's local outbox which then gets delivered to one or more service's inboxes.

## Outbox

When an activity is posted to the local server's Outbox, it is first stored in the local "outbox" database and then posted to one or more recipients as specified in the Activity's "to" field. Each URI is first resolved via WebFinger and then posted to the service's inbox. The post to each of the URIs is performed asynchronously using an AMQP message queue. The message is published to a queue (orb.activity.outbox) and then one of the servers in the domain processes it by sending the HTTP request to the recipient's inbox. Before sending the request, an HTTP signature is added to the request header which may be verified by the recipient using the [public key](https://trustbloc.github.io/activityanchors/#actor-discovery) of the local service.

## Inbox

The Inbox is a REST endpoint in Orb which accepts activities. When an Activity is received, the HTTP signature in the header of the request is verified using the public key of the [Actor](https://trustbloc.github.io/activityanchors/#actor-discovery) that sent the activity. After the Actor is authenticated, a message is posted to a queue (orb.activity.inbox) and is processed by one of the server instances in the domain.

The inbox handler first stores the activity in the 'inbox' database and then authorizes the activity. Each activity could have different authorization criteria which are explained in each of the activities below.

## Activities

### Follow

A [Follow](https://trustbloc.github.io/activityanchors/#follow-activity) activity is posted by a server to other server so that activities may be synchronized between them. For example, domain1.com posts a Follow activity to domain2.com indicating that it wants notifications of objects created by domain2.com (using the [Create](#Create) activity). It domain1.com is also notified of any objects created by servers that domain2.com is following [Announce](#Announce). When a server receives a Follow activity in its inbox, it authorizes the 'actor' (which is the originating server) in the Follow request using a configurable [Follow Authorization Policy](#follow-authorization-policy). If authorized, the actor is added to the list of _followers_ and an [Accept](https://trustbloc.github.io/activityanchors/#accept-follow-activity) activity is posted to the actor's inbox. If the actor is not authorized then a [Reject](https://trustbloc.github.io/activityanchors/#reject-follow-activity) activity is posted to the actor's inbox.

### Accept/Reject Follow

When an [Accept](https://trustbloc.github.io/activityanchors/#accept-follow-activity) activity is received in the inbox, the activity is first validated against a previously posted Follow activity and then the 'actor' in the Accept activity (which was the target server in Follow) is added to the _following_ collection. A [Reject](https://trustbloc.github.io/activityanchors/#reject-invite-witness-activity) of an Invite activity simply logs the fact that the request was rejected.

### Undo Follow

An [Undo](https://trustbloc.github.io/activityanchors/#undo-follow-activity) activity may be posted to 'unfollow' a server. The Undo activity is posted to the server to which the previous Follow activity was posted and the target server is removed from the _following_ list. The target server handles the Undo activity by removing the originating server from the _followers_ list.

### Invite Witness

An Invite activity is posted to another server in order to invite that server to be a witness of [Anchor Events](https://trustbloc.github.io/activityanchors/#anchorevent). For example, domain1.com posts an Invite activity to domain2.com indicating that it wants domain2.com to be a witness of Anchor Events produced by domain1.com (using the [Offer](#Offer) activity).

When a server receives an Invite witness activity in its inbox, it will authorize the actor (which is the originating server) in the Invite request using an [Invite Witness Authorization Policy](#invite-witness-authorization-policy). If authorized, the actor is added to the _witnessing_ collection and an [Accept](https://trustbloc.github.io/activityanchors/#accept-invite-witness-activity) activity is posted to the actor's inbox. If the actor is not authorized then a [Reject](https://trustbloc.github.io/activityanchors/#reject-invite-witness-activity) activity is posted to the actor's inbox.

### Accept/Reject Invite Witness

When an [Accept](https://trustbloc.github.io/activityanchors/#accept-invite-witness-activity) activity is received in the inbox, the activity is first validated against a previously posted Invite witness activity and then the 'actor' in the Accept activity is added to the _witnesses_ collection. A [Reject](https://trustbloc.github.io/activityanchors/#reject-follow-activity) of a Follow activity simply logs the fact that the request was rejected.

### Undo Invite Witness

An [Undo](https://trustbloc.github.io/activityanchors/#undo-invite-witness-activity) activity may be posted to remove a server as a witness. The Undo activity is posted to the server to which the previous Invite activity was posted and the target server is removed from the _witnesses_ list. The target server handles the Undo activity by removing the originating server from the _witnessing_ list.

### Offer

An [Offer](https://trustbloc.github.io/activityanchors/#offer-activity) activity is posted to one or more servers that are contained in the _witnesses_ collection in order to collect proofs. (The selection of witnesses is dictated by a [Witness Policy](witnesspolicy.html#witness-policy) which determines the minimum number of proofs required.) When a server receives an Offer activity in its inbox, the request is first authorized by ensuring that the actor of the Offer is in the _witnessing_ collection. Once authorized, the [Anchor Object](https://trustbloc.github.io/activityanchors/#anchorevent) which is embedded in the Offer activity is added to the ledger and an [Accept](https://trustbloc.github.io/activityanchors/#accept-anchor-activity) offer activity (which contains the proof) is posted back to the originator of the offer. If the actor of the Offer is not in the _witnessing_ collection then a [Reject](https://trustbloc.github.io/activityanchors/#reject-anchor-activity) activity is posted to the originating actor.

### Accept/Reject Offer

When an [Accept](https://trustbloc.github.io/activityanchors/#accept-anchor-activity) offer activity is received in the inbox, the activity is first validated against a previously posted Offer activity and then the embedded proof for the [Anchor Object](https://trustbloc.github.io/activityanchors/#anchorevent) is added to the collection of existing proofs. Once a sufficient number of proofs are received (according to the [Witness Policy](witnesspolicy.html#witness-policy)) then a complete Anchor Event (containing all of the proofs) is constructed and the Anchor Event is posted to the queue, _orb.anchor_event_, to be processed by [Batch Writer](batchwriter.html#batch-writer). A [Reject](https://trustbloc.github.io/activityanchors/#reject-anchor-activity) of an Offer activity simply logs the fact that the request was rejected.

### Create

A [Create](https://trustbloc.github.io/activityanchors/#create-activity) activity is posted by the [Batch Writer](batchwriter.html#batch-writer) to one or more servers that are contained in the *followers* collection. When a server receives a Create activity in its inbox, the request is first authorized by ensuring that the actor of the Create is in the _following_ collection. Once authorized, the [Anchor Event](https://trustbloc.github.io/activityanchors/#anchorevent) which is embedded in the Create activity is posted to the *orb.anchor* queue so that it may be processed by the [Observer](observer.html#observer). The Create is also forwarded to the servers in the followers collection using the [Announce](https://trustbloc.github.io/activityanchors/announce-activity) activity. If the actor of the Create activity is not in the _following_ collection then a the activity is ignored.

### Announce

When a server receives an [Announce](https://trustbloc.github.io/activityanchors/announce-activity) activity in its inbox, the request is first authorized by ensuring that the actor of the Announce is in the _following_ collection. Once authorized, the [Anchor Event](https://trustbloc.github.io/activityanchors/anchorevent) which is embedded in the Announce activity is posted to the *orb.anchor* queue so that it may be processed by the [Observer](observer.htmlobserver). If the actor of the Announce activity is not in the _following_ collection then a the activity is ignored.

## Authorization Policy

### Follow Authorization Policy

An authorization policy may be configured for the [Follow](#follow) activity using the configuration parameters, [FOLLOW_AUTH_POLICY](config.html#follow-auth-policy). The possible values are  _accept-all_ and _accept-list_. If accept-all is used then all Follow requests are accepted. If accept-list is used then the actor in the Follow activity must be in the accept list database. The accept list is configured using a REST [endpoint](#accept-list).

### Invite Witness Authorization Policy

An authorization policy may be configured for the [Invite Witness](#invite-witness) activity using the configuration parameters, [INVITE_WITNESS_AUTH_POLICY](config.html#invite-witnesss-auth-policy). The possible values are  _accept-all_ and _accept-list_. If accept-all is used then all Invite witness requests are accepted. If accept-list is used then the actor in the Invite activity must be in the accept list database. The accept list is configured using a REST [endpoint](#accept-list).

## REST Endpoints

### Service

### Activities

### Inbox

### Outbox

### Followers

### Following

### Witnesses

### Witnessing

### Likes

### Liked

### Shares

### Anchor Event

## Accept List
