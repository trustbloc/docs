# Introduction

Orb implements the following specifications: [did:orb](https://trustbloc.github.io/did-method-orb/),
[Activity Anchors](https://trustbloc.github.io/activityanchors/). The did:orb method is based on the
[Sidetree](https://identity.foundation/sidetree/spec/) specification and Activity Anchors is based on the
[ActivityPub](https://www.w3.org/TR/activitypub/) and [ActivityStreams](https://www.w3.org/TR/activitystreams-core/)
specifications.

## Services

A typical Orb domain consists of the following services:

* [Orb](https://github.com/trustbloc/orb) instance (multiple instances may be running for redundancy scalability)
* Document database (AWS [DocumentDB](https://aws.amazon.com/documentdb/) or [MongoDB](https://www.mongodb.com/))
* AMQP message broker ([RabbitMQ](key-management-system-kms))
* [Key Management Service](../kms.html#key-management-system-kms) (Aries KMS)
* Verifiable Credential Transparency (VCT) ([Google Trillian](https://github.com/google/trillian))
* [IPFS](https://ipfs.io/) (optional)

```{image} ../_static/orb/nodes.svg

```

## Components

The diagram below displays the components that make up the Orb server.

```{image} ../_static/orb/components.svg

```

The section below describes a typical DID creation flow that gives an overview of the component interactions.
More detailed descriptions are provided in the various sections of this document.

### DID Creation Flow

Creation and resolution of DIDs involve two REST endpoints: Operation Writer and DID Resolver. The 
Operation Writer endpoint accepts Sidetree operations to create, update, deactivate and recover DIDs. The DID 
Resolver endpoint reads the Sidetree operations for a DID suffix and returns a DID document.

When an operation is posted to the Operation Writer, a message is posted to the AMQP operation queue which is
consumed by the [Batch Writer](batchwriter.html#batch-writer). The Batch Writer stores the operation and cuts a
batch when a maximum number of operations have been posted (or a batch [timeout](parameters.html#batch-writer-timeout)
is reached). The batch contains the DID operations that were posted since the last batch was cut. The batch is written to
the [Content Addressable Storage](cas.html#content-addressable-storage-cas) (CAS) (which can be in a local storage or 
IPFS) and an [AnchorEvent](https://trustbloc.github.io/activityanchors/#anchorevent)
(which includes the DIDs from the batch) is posted via an [Offer](https://trustbloc.github.io/activityanchors/#offer-activity)
activity to the ActivityPub Outbox. The Offer activity is stored in the Activities database and then delivered to all 
services in the [witnesses](activitypub.html#invite-witness) collection in order to collect proofs from these services.
An HTTP signature is added to the header of the Offer request (signed by [KMS](../kms.html#key-management-system-kms)) so 
that the recipient can authenticate the sender.

Proofs are posted to the ActivityPub inbox using a signed HTTPS request that contains an
[Accept](https://trustbloc.github.io/activityanchors/#accept-anchor-activity) activity. The Accept activity is stored
in the Activities database, the HTTP signature is verified, and the proof is sent to the Anchor Writer. When the 
AnchorWriter determines that enough proofs have been received (according to the
[Witness Policy](witnesspolicy.html#witness-policy)), a new AnchorEvent is constructed 
with the gathered proofs. The AnchorEvent is written to CAS and the hash of the AnchorEvent is posted to the Observer 
queue for processing.

The [Observer](observer.html#observer) consumes the anchor hash from the queue and reads the AnchorEvent (along
with the corresponding Sidetree operations) from CAS, and processes/stores each operation to the Operations 
database.

A client sends a request to the DID Resolver endpoint to retrieve the DID document for the created/updated operation.
The DID Resolver endpoint reads the operations for the provided DID suffix, applies the operation, and 
returns the DID document.
