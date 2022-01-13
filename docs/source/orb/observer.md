# Observer

Observer listens to witnessed anchor index events. Upon receiving an anchor index event observer will:

1) Load anchor index file from Content Addressable Storage(CAS)

2) Validate witness signatures

3) Retrieve Sidetree batch files from CAS

4) Validate Sidetee batch files

*   Verify the number of operations does not exceed the maximum allowed limit.
*   Verify size of each operation does not exceed the maximum allowed limit.
*   Ensure the batch meets proof-of-work requirements.

_5) Store each DID operation into Operations store._

_6) Delete corresponding unpublished DID operations from Unpublished Operations store_

