# Content Addressable Storage (CAS)

Content Addressable Storage (CAS) is a persistent storage of content where the primary key is the hash of the content.
This ensures that content for a given key is immutable, i.e. if the content changes then the primary key changes.

Orb supports two types of CAS: local storage (in a database) and [IPFS](https://ipfs.io/). The type of CAS storage that
an Orb instance uses is dictated by the startup parameter, [cas-type](../parameters.html#cas-type). Additionally, if
[cas-type](../parameters.html#cas-type) is set to _local_ and the startup parameter,
[replicate-local-cas-writes-in-ipfs](../parameters.html#replicate-local-cas-writes-in-ipfs), is set to _true_, then CAS
data is stored in the local database and also replicated to [IPFS](https://ipfs.io/).

## Local

A local CAS storage is simply a [database](database.html#cas) that stores content using the
[sha-256 multihash](https://w3c-ccg.github.io/multihash/index.xml#tv-sha256) of the content as the primary key.
The content is _not_ automatically replicated (as is the case with IPFS). A _local_ CAS
storage relies on ActivityPub activities ([Create and Announce](activitypub.html#create-announce)) to replicate data.

## IPFS

If the [cas-type](../parameters.html#cas-type) is set to _ipfs_ then an additional startup parameter,
[ipfs-url](../parameters.html#ipfs-url) is required to point to the IPFS node. The IPFS node
must have permissions to read and write content. This node replicates the content to other IPFS nodes in the network.

The version of the content ID ([CID](https://docs.ipfs.io/concepts/content-addressing/#identifier-formats)) that is used
as the key for content retrieval is specified with startup parameter [cid-version](../parameters.html#cid-version).
