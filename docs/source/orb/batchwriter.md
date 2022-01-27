# Batch Writer

Orb node will validate each supported Sidetree operation(Create, Update, Recover and Deactivate) 
as per [Sidetree DID Operations](sidetree.html#did-operations) and then add valid operation to the batch writer queue.

Batch writer will:
- drain operations from the operations queue
- batch multiple Sidetree operations together into Sidetree batch files 
as per [Sidetree file structure spec](https://identity.foundation/sidetree/spec/#file-structures)
- store Sidetree batch files into content addressable storage (CAS)
- anchor a reference to the main Sidetree batch file (core index file) on the anchoring system as Sidetree transaction

The number of operations that can be stored in the Sidetree batch files is limited 
by [Sidetree protocol parameter](https://identity.foundation/sidetree/spec/#default-parameters) MAX_OPERATION_COUNT.

Batch Writer will cut Sidetree batches if the number of operations in the batch writer queue reaches MAX_OPERATION_COUNT 
or if the batch writer reaches batch writer timeout [BATCH_WRITER_TIMEOUT](parameters.html#batch-writer-timeout).


