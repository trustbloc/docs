# REST Endpoints

Following is a list of all REST endpoints exposed by Orb:

## .well-known

### did-orb

/.well-known/did-orb

### webfinger

/.well-known/webfinger

### host-meta

/.well-known/host-meta

### host-meta.json

/.well-known/host-meta.json

### did.json

/.well-known/did.json

### nodeinfo

/.well-known/nodeinfo

## Sidetree

### operations

/sidetree/v1/operations

Issue DID operation as per [Sidetree DID Operations](sidetree.html#did-operations)

### identifiers

/sidetree/v1/identifiers/[id]

Resolve DID documents as per [Sidetree DID Resolution](sidetree.html#did-resolution)


## Services

### Service

/services/orb

### Keys

/services/orb/keys/[id]

### Followers

/services/orb/followers

### Following

/services/orb/following

### Outbox

/services/orb/outbox

### Inbox

/services/orb/inbox

### Witnesses

/services/orb/witnesses

### Witnessing

/services/orb/witnessing

### Liked

/services/orb/liked

### Likes

/services/orb/likes

### Shares

/services/orb/shares

### Activities

/services/orb/activities/[id]

### Accept List

/services/orb/acceptlist

## CAS

/cas/[cid]

## Witness Policy

/policy

Configure witness policy as per [Witness Policy](witnesspolicy.html#witness-policy)

## VC

/vc/[id]

## LD

## LD Context

/ld/context

### Post Remote Provider

/ld/remote-provider

### Refresh a Remote Provider

/ld/remote-provider/[id]/refresh

### Get Remote provider

/ld/remote-provider/[id]

### Get Remote Providers

/ld/remote-providers

### Refresh all Remote Provider

/ld/remote-providers/refresh

## Node Info

### Node Info version 2.0

/nodeinfo/2.0

### Node Info version 2.1

/nodeinfo/2.1

## Metrics

### /metrics
