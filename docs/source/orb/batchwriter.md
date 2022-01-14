# Batch Writer

Batch writer batches multiple Sidetree operations together and stores them in Sidetree batch files as per [Sidetree file structure spec](https://identity.foundation/sidetree/spec/#file-structures). The number of operations that can be stored in the Sidetree batch files is limited by [Sidetree protocol parameter](https://identity.foundation/sidetree/spec/#default-parameters) MAX_OPERATION_COUNT.
