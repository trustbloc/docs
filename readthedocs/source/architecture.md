# Architecture

```{image} _static/platform_components.png
```

Diagram represents an overview of the components related to the TrustBloc initiative.

The TrustBloc initiative provides a common infrastructure for consortium to enable document provenance and data exchange, while still allowing these consortiums to differentiate based on their service offerings. We are building on top of existing community efforts from Hyperledger Fabric along with the proposed Hyperledger Aries project.

The TrustBloc initiative aims to provide consortium-based document provenance and services to reduce trust obstacles. We are enabling DIDs to be managed and exposed from Fabric, and more generically enabling document provenance. The following features are early goals of the initiative:

- Provide out-of-the-box capabilities for document provenance (including DIDs).
- Enhance Hyperledger Fabric private collections to support SideTrees – allowing identifiers and documents to be anchored to a channel without performing individual Fabric transaction for each identifier or document.
- Enable off-chain distributed storage model to support transactions (e.g., transient storage, content-addressable storage).
- Support a storage and query model for data scoped to a particular organization.

**Data Exchange Components**

TrustBloc will also provide a verifiable credential exchange flow. We are enabling a model for digital identity exchange based on DIDs, Verifiable Credentials, and Hubs. This will be achieved by:

- Leveraging emerging community specifications (e.g., DIDs and Verifiable Credentials).
- Enabling usage of Peer DIDs to establish data exchange transactions while anchoring to ledger-based DIDs and verifiable credentials.
- Supporting a Hub-based model for efficient, real-time data exchange.
