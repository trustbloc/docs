# Observer

The Observer subscribes to the _orb.anchor_ queue and handles witnessed [anchor linksets](https://trustbloc.github.io/activityanchors/#anchorevent).
Upon receiving an anchor linkset, the Observer:

1) Loads the anchor linkset from the [Content Addressable Storage (CAS)](cas.html#content-addressable-storage-cas)
2) Validates the witness signatures
3) Retrieves Sidetree batch files from CAS
4) Validates Sidetree batch files:
   * Ensures that the number of operations does not exceed the maximum allowed limit
   * Ensures that the size of each operation does not exceed the maximum allowed limit
   * Ensures the batch meets proof-of-work requirements

5) Stores each DID operation into the _Operation_ store
6) Deletes the corresponding unpublished DID operations from the _Unpublished Operation_ store

```{image} ../../_static/orb/observer.svg

```
