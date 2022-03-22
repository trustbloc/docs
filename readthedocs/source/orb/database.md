# Databases

## Permanent Data

The following databases store data permanently, i.e. the data (for now) is never deleted.

### activity

The _activity_ database stores ActivityPub activities that are posted to the
[outbox](restendpoints/activitypub.html#outbox) or received in the
[inbox](restendpoints/activitypub.html#inbox).

### activity-ref

The _activity-ref_ database stores references to the _activity_ database using a _RefType_ tag so that
a query may be performed for activities referenced by a certain type. For example, setting the _RefType_
tag to OUTBOX ensures that the activity is included in the result set for a query of activities
in the [outbox](restendpoints/activitypub.html#outbox).

Valid values for _RefType_ are:
1) INBOX
2) OUTBOX
3) PUBLIC_OUTBOX
4) FOLLOWER
5) FOLLOWING
6) WITNESS
7) WITNESSING
8) LIKE
9) LIKED
10) SHARE
11) ANCHOR_LINKSET

### anchor-ref

The _anchor_ref_ database contains the hashlinks (with metadata) of where an anchor
(tagged with _anchorHash_) may be resolved. This includes the local domain, remote domains, and IPFS.

### cas

The _cas_ database stores content addressable objects. This database is only used if Orb is configured
with the [local](parameters.html#cas-type) CAS type.

### did-anchor

The _did-anchor_ database stores the latest anchor hash of a DID suffix.

### ldcontexts

The _ldcontexts_ database stores JSON linked-data (JSON-LD) contexts.

### operation

The _operation_ database stores Sidetree operations.

### orb-config

The _orb-config_ database stores configuration data.

### remoteproviders

TBD

### verifiable

The _verifiable_ database store verifiable credentials.

## Temporary Data

The following databases (for the most part) contain temporary data that is used only during processing
of a batch of operations. The data is deleted after the batch has been processed.

### anchor-link

The _anchor-link_ database stores anchor links (of an anchor Linkset) that have not yet been witnessed.
After a sufficient number of proofs have been received for the anchor (according to
[witness policy](witnesspolicy.html#witness-policy)) the anchor link is deleted.

### anchor-status

The _anchor-status_ database stores the status of an anchor while it is waiting for witnesses.
The status is either _in-process_ or _completed_. After a sufficient number of proofs have been
received for the anchor (according to [witness policy](witnesspolicy.html#witness-policy)) the
anchor entry is deleted.

### activity-sync

The _activity-sync_ database stores the page number and index of the last activity
that was synchronized for each of the domains that are followed. This information is used by the
[Activity Sync task](onboardrecover.html#activity-sync-task).

### monitoring

The _monitoring_ database keeps track of the proofs that were received by other domains and ensures
that the proofs were added to their VCTs.

### op-queue

The _op-queue_ database contains the operations posted to the [operation queue](batchwriter.html#operation-queue).
Operations are deleted after they have been processed.

### unpublished-operation

The _unpublished-operation_ database contains the operations that were posted by a client via the
[operations endpoint](restendpoints/sidetree.html#operations) but have not yet been anchored.
Operations are deleted from this database after they have been anchored.

### witness

The _witness_ database stores the URIs of the domains that were asked for proofs for a given anchor.
The witness URIs are deleted from this database after the anchor has been processed.
