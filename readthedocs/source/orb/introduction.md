# Introduction

Orb implements the following specifications: [did:orb](https://trustbloc.github.io/did-method-orb/),
[Activity Anchors](https://trustbloc.github.io/activityanchors/). The did:orb method is based on the
[Sidetree](https://identity.foundation/sidetree/spec/) specification and Activity Anchors is based on the
[ActivityPub](https://www.w3.org/TR/activitypub/),
[ActivityStreams](https://www.w3.org/TR/activitystreams-core/), and
[Linkset](https://datatracker.ietf.org/doc/draft-ietf-httpapi-linkset/) specifications.

## Services

A typical Orb domain consists of the following services:

1) [Orb](https://github.com/trustbloc/orb) instance (multiple instances may be running for redundancy scalability)
2) Document database (AWS [DocumentDB](https://aws.amazon.com/documentdb/) or [MongoDB](https://www.mongodb.com/))
3) AMQP message broker ([RabbitMQ](https://www.rabbitmq.com))
4) [Key Management Service](../kms/index.html#key-management-system-kms) (Aries KMS)
5) Verifiable Credential Transparency (VCT) ([Google Trillian](https://github.com/google/trillian))
6) [IPFS](https://ipfs.io/) (optional)

```{image} ../_static/orb/nodes.svg

```

## Components

The diagram below displays the components that make up the Orb server.

```{image} ../_static/orb/components.svg

```

The section below describes a typical DID creation flow that gives an overview of the component interactions.
More detailed descriptions are provided in the various sections of this document.

### DID Creation Flow

Creation and resolution of DIDs involve two REST endpoints: [Operation Writer](restendpoints/sidetree.html#operations) and
[DID Resolver](restendpoints/sidetree.html#identifiers). The Operation Writer endpoint accepts Sidetree operations to create,
update, deactivate and recover DIDs. The DID Resolver endpoint reads the Sidetree operations for a DID suffix and
returns a DID document.

When an operation is posted to the Operation Writer, a message is posted to the AMQP operation queue which is
consumed by the [Batch Writer](system/batchwriter.html#operation-queue). The Batch Writer stores the operation and cuts a
batch when the maximum batch size is reached, or the batch [times out](parameters.html#batch-writer-timeout).
The batch contains the DID operations that were posted since the last batch was cut. The batch is written to
the [Content Addressable Storage](system/cas.html#content-addressable-storage-cas) (CAS) (which can be in a local storage or
IPFS) and an [anchor linkset](https://trustbloc.github.io/activityanchors/#anchorevent) is created. The anchor linkset
contains the DIDs from the batch and a verifiable credential containing a proof from the local server. The verifiable
credential is added to the local [VCT](vct/introduction.html#vct). An [AnchorEvent](https://trustbloc.github.io/activityanchors/#anchorevent)
(which wraps the linkset) is created and posted via an [Offer](system/activitypub.html#offer-accept) activity to the
[Outbox](system/activitypub.html#outbox-inbox). The Offer activity is stored in the Activities database and an HTTP request
(signed by [KMS](../kms/index.html#key-management-system-kms)) is sent to the [Inbox](system/activitypub.html#outbox-inbox) of
one or more Orb servers in the [witnesses](system/activitypub.html#invite-witness) collection.

Proofs from witnessing servers are received in the Inbox as [Accept](system/activitypub.html#offer-accept) 
activities. After the Inbox verifies the [HTTP signature](system/authorization.html#http-signatures) of the request,
the Accept activity is stored in the Activities database and the
proof is sent to the [Witness Proof Handler](system/batchwriter.html#witness-proof-handler). When this handler determines
that enough proofs have been received (according to the [Witness Policy](system/witnesspolicy.html#witness-policy)),
a new [anchor linkset](https://trustbloc.github.io/activityanchors/#anchorevent) is constructed with the gathered
proofs. The anchor linkset is written to CAS and the hash of the anchor linkset is posted to the _anchor_ queue for
processing.

The [Observer](system/observer.html#observer) consumes the anchor linkset hash from the queue and reads the anchor linkset
(along with the corresponding Sidetree operations) from CAS, and processes/stores each operation to the _operations_
database.

A client sends a request to the [DID Resolver](restendpoints/sidetree.html#identifiers) endpoint to retrieve the DID document for
the created/updated operation. The DID Resolver retrieves all operations related to the provided DID suffix
from the Operations database, applies the operations, and returns the DID document.
