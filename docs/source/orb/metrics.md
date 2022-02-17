# Metrics

An Orb server records performance metrics at each subsystem. A [Prometheus](https://prometheus.io/) server may be used to periodically read the metrics at the URL specified by the startup parameter, [host-metrics-url](parameters.html#host-metrics-url). Below are the metrics defined at each subsystem.

## ActivityPub

The ActivityPub subsystem deals with server to server communications.

### activitypub_outbox_post_seconds

The time (in seconds) that it takes to post a message to the outbox.

### activitypub_outbox_resolve_inboxes_seconds

The time (in seconds) that it takes to resolve the inboxes of the destination URLs when posting to the outbox.

### activitypub_inbox_handler_seconds

The time (in seconds) that it takes to handle an activity posted to the inbox.

### activitypub_outbox_count

The number of activities posted to the outbox.

## AnchorEvent

The AnchorEvent subsystem is responsible for gathering proofs for an AnchorEvent from multiple witnesses.

### anchor_write_seconds

The time (in seconds) that it takes to write an anchor credential and post an 'Offer' activity.

### anchor_witness_seconds

The time (in seconds) that it takes for a verifiable credential to gather proofs from all required witnesses (according to witness policy). The start time is when the verifiable credential is issued and the end time is the time that the witness policy is satisfied.

### anchor_process_witnessed_seconds

The time (in seconds) that it takes to process a witnessed anchor credential by publishing it to the Observer and posting a *Create* activity.

### anchor_write_build_cred_seconds

The time (in seconds) that it takes to build a credential (inside the *write anchor* function).

### anchor_write_get_witnesses_seconds

The time (in seconds) that it takes to get witnesses (inside the *write anchor* function).

### anchor_write_sign_cred_seconds

The time (in seconds) that it takes to sign the credential (inside the *write anchor* function).

### anchor_write_post_offer_activity_seconds

The time (in seconds) that it takes to post an offer activity (inside the *write anchor* function).

### anchor_write_get_previous_anchor_get_bulk_seconds

The time (in seconds) that it takes to perform a database 'bulk get' (inside the *get previous anchor* function).

### anchor_write_get_previous_anchor_seconds

The time (in seconds) that it takes to get the previous anchor.

### anchor_write_sign_with_local_witness_seconds

The time (in seconds) that it takes to sign with the local witness key.

### anchor_write_sign_with_server_key_seconds

The time (in seconds) that it takes to sign with a server key.

### anchor_write_sign_local_witness_log_seconds

The time (in seconds) that it takes to witness the log (inside the *sign local* function).

### anchor_write_sign_local_watch_seconds

The time (in seconds) that it takes to add the verifiable credential to the VCT monitoring service (inside the *sign local* function).

### anchor_write_resolve_host_meta_link_seconds

The time (in seconds) that it takes to resolve a host meta link.

### anchor_write_store_seconds

The time (in seconds) that it takes to store an anchor event.

## Operation Queue

The *Operation Queue* is an [AMQP](https://www.amqp.org/) implementation of a [Sidetree](https://identity.foundation/sidetree/spec/) operation queue. Operations are posted to the queue and a batch is cut when the queue size reaches the maximum batch size (configured in the Sidetree protocol) or whwn a batch timeout occurs.

### opqueue_add_operation_seconds

The time (in seconds) that it takes to add an operation to the queue.

### opqueue_batch_cut_seconds

The time (in seconds) that it takes to cut an operation batch. The duration is from the time that the first operation was added to the time that the batch was cut.

### opqueue_batch_rollback_seconds

The time (in seconds) that it takes to roll back an operation batch (in case of a transient error). The duration is from the time that the first operation was added to the time that the batch was cut.

### opqueue_batch_size

The size of a cut batch.

### opqueue_process_anchor_seconds

The time (in seconds) that it takes for the Observer to process an anchor credential.

### opqueue_process_did_seconds

The time (in seconds) that it takes for the Observer to process a DID.

## Content Addressable Storage

The Content Addressable Store (CAS) is either implemented as a local store or using [IPFS](https://ipfs.io/).

### cas_write_seconds

The time (in seconds) that it takes to write a document to CAS.

### cas_resolve_seconds

The time (in seconds) that it takes to resolve a document from CAS.

### cas_cache_hit_count

The number of times a CAS document was retrieved from the cache.

### cas_read_seconds

The time (in seconds) that it takes to read a document from the CAS storage.

## Document

The Document metrics measure times for posting *create* and *update* Sidetree operations, as well as resolving DID documents.

### document_create_update_seconds

The time (in seconds) it takes the REST handler to process a create/update operation.

### document_resolve_seconds

The time (in seconds) it takes the REST handler to resolve a document.

## Database

Database metrics record the times for reads, writes, bulk writes, etc.

### db_put_seconds

The time (in seconds) it takes the DB to store data.

### db_get_seconds

The time (in seconds) it takes the DB to retrieve data by primary key.

### db_get_tags_seconds

The time (in seconds) it takes the DB to get tags.

### db_get_bulk_seconds

The time (in seconds) it takes the DB to get bulk.

### db_query_seconds

The time (in seconds) it takes to query for data.

### db_delete_seconds

The time (in seconds) it takes to delete data.

### db_batch_seconds

The time (in seconds) it takes to perform a batch update.

## Verifiable Credential Transparency

The Verifiable Credential Transparency (VCT) subsystem records verifiable credentials to a ledger (backed by [Google Trillian](https://github.com/google/trillian)).

### vct_witness_add_proof_vct_nil_seconds

The time (in seconds) it takes the add proof when vct is nil in witness.

### vct_witness_add_vc_seconds

The time (in seconds) it takes the add vc in witness.

### vct_witness_add_proof_seconds

The time (in seconds) it takes the add proof in witness.

### vct_witness_webfinger_seconds

The time (in seconds) it takes web finger in witness.

### vct_witness_verify_vct_signature_seconds

The time (in seconds) it takes verify vct signature in witness.

### vct_witness_add_proof_parse_credential_seconds

The time (in seconds) it takes the parse credential in add proof.

### vct_witness_add_proof_sign_seconds

The time (in seconds) it takes the sign in add proof.

## Signer

Following are metrics for the signer, which is either using a local key or [KMS](https://github.com/trustbloc/kms).

### signer_get_key_seconds

The time (in seconds) it takes to get the key.

### signer_sign_seconds

The time (in seconds) it takes to sign.

### signer_add_linked_data_proof_seconds

The time (in seconds) it takes to add linked data proof.

## Resolver

The document resolver resolves a DID document, either from local store, or from the anchor origin.

### resolver_resolve_document_locally_seconds

The time (in seconds) it takes to resolve the document locally.

### resolver_get_anchor_origin_endpoint_seconds

The time (in seconds) it takes to get endpoint information from the anchor origin.

### resolve_document_from_anchor_origin_seconds

The time (in seconds) it takes to resolve a document from the anchor origin.

### resolver_resolve_document_from_create_document_store_seconds

The time (in seconds) it takes to resolve a document from the _create_ document store.

### resolver_delete_document_from_create_document_store_seconds

The time (in seconds) it takes the resolver to delete a document from the _create_ document store.

### resolver_verify_cid_seconds

The time (in seconds) it takes to verify a CID in the anchor graph.

### resolver_request_discovery_seconds

The time (in seconds) it takes to request DID discovery.

## Decorator

The decorator is invoked by the core [Sidetree](https://github.com/trustbloc/sidetree-core-go) library. It verifies (from the anchor origin) that the local domain has the latest operations.

### decorator_decorate_seconds

The time (in seconds) it takes the decorator to pre-process the document operation.

### decorator_processor_resolve_seconds

The time (in seconds) it takes the processor to resolve a document before accepting the document operation.

### decorator_get_ao_endpoint_and_resolve_from_ao_seconds

The time (in seconds) it takes to resolve a document from the anchor origin before accepting the document operation.

## Operation Store

The operation store contains published and unpublished operations.

### operations_put_unpublished_operation_seconds

The time (in seconds) it takes to store an unpublished operation.

### operations_get_unpublished_operations_seconds

The time (in seconds) it takes to get an unpublished operations for a given suffix.

### operations_calculate_unpublished_operation_key_seconds

The time (in seconds) it takes to calculate a key for an unpublished operation.

### operations_put_published_operations_seconds

The time (in seconds) it takes to store published operations.

### operations_get_published_operations_seconds

The time (in seconds) it takes to get published operations for a given suffix.

## Sidetree Core

The follow metrics are produced by the core [Sidetree](https://github.com/trustbloc/sidetree-core-go) library.

### core_process_operation_seconds

The time (in seconds) it takes to process a DID operation.

### core_get_protocol_version_seconds

The time (in seconds) it takes to get the protocol version (in *process operation*).

### core_parse_operation_seconds

The time (in seconds) it takes to parse an operation in (in *process operation*).

### core_validate_operation_seconds

The time (in seconds) it takes to validate an operation (in *process operation*).

### core_decorate_operation_seconds

The time (in seconds) it takes to decorate an operation (in *process operation*).

### core_add_unpublished_operation_seconds

The time (in seconds) it takes to add an unpublished operation to the store (in *process operation*).

### core_add_operation_to_batch_seconds

The time (in seconds) it takes to add an operation to the batch (in *process operation*).

### core_get_create_operation_result_seconds

The time (in seconds) it takes to get a _create_ operation result (in *process operation*).

### core_http_create_update_seconds

The time (in seconds) it takes for a _create/update_ HTTP call.

### core_http_resolve_seconds

The time (in seconds) it takes for a _resolve_ HTTP call.
