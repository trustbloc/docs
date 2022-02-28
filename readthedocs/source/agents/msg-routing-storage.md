# Message Routing and Storage

## Summary

This proposal reuses, modifies, and adapts several proposals from the Hyperledger Aries/Indy, and the DIF communities to in order to enable:

- Advanced use cases for credentials exchange, such as when the Issuer requires the User's prior consent for issuance
- Guaranteed message delivery - even if the Agent is temporarily unavailable
- User-specified routing of messages from mediators to their Agents
- Unified message routing protocols and APIs
- Safe storage of encrypted identities separated from the encryption keys
- Simpler model for synchronization of wallets
- Simplex and duplex messaging paradigms

Specifically, this proposal builds on the foundation laid down by these proposals:

- [DIF Identity Hub](https://github.com/decentralized-identity/identity-hub/blob/master/explainer.md)
- [Aries RFC 0046: Mediators and Relays](https://github.com/hyperledger/aries-rfcs/blob/master/concepts/0046-mediators-and-relays/README.md)
- [Aries RFC 0019: Encryption Envelope](https://github.com/hyperledger/aries-rfcs/tree/master/features/0019-encryption-envelope)
- [Aries RFC 0094: Cross Domain Messaging](https://github.com/hyperledger/aries-rfcs/blob/master/concepts/0094-cross-domain-messaging/README.md)
- [Aries RFC 0050: Wallets](https://github.com/hyperledger/aries-rfcs/blob/master/concepts/0050-wallets/README.md)

Here is a generic, simplified view:

```{image} ../_static/agents/a2a-overview.png
```

### 1 - Hub Storage

Corresponds to the DIF Identity Hub's [Collections](https://github.com/decentralized-identity/identity-hub/blob/master/explainer.md#collections) interface and [Replication Protocol](https://hackmd.io/OInEIRLxQY2s48tze0E7IQ).

The Permissions API is disregarded because objects stored here are accessible solely by the Agent.

This storage service is plugged into the Agent's wallet as an implementation of the Storage Interface as shown [here](https://github.com/hyperledger/indy-sdk/tree/master/docs/design/003-wallet-storage#wallet-components).

### 2 - Mediator

The mediator filters messages (authorization) and routes them to the Agent of the Identity Owner's choosing based on user-specified rules stored in a user-specified Hub Storage location.

Mediators buffer undelivered messages sent to the Agents until confirmation of delivery.

Mediators can be extended in many different ways to support many interesting use cases. For example, an Issuer's Agent can request collaboration from other mediators (with prior consent from their respective Agents) in order to fulfill a request.

### 3 - Relay Network

Messages between sovereign domains are transported via a network of relays.

Mediators may communicate through one or several relay networks as per their requirements.

For example, a mediator might leverage the public TOR relay network to protect the Agent's privacy, or it might simply use the internet.

## Trusted Contexts

The basic exchange implied in the diagram above solves many real-world use cases, but needs to be extended to support scenarios where the Issuer remains the Holder of the User Data - while keeping the User in the locus of control.

The User may introduce themselves directly to the other parties by sharing [Peer DIDs](https://dhh1128.github.io/peer-did-method-spec), or they may discover these other peers through an app that displays the public, well-known, blockchain-anchored [DIDs](https://w3c-ccg.github.io/did-spec/) of recognized institutions.

:::{figure} _static/ledger-anchored-trust.png
:::

Mobile agent displaying parties with well-known DIDs anchored to a blockchain, all of which possess Verifiable Credentials from a trusted Issuer (trusted issuer not shown).

Once introduced to these parties (individually), the User proceeds to create a Trusted Context between all parties by asking them each for a new DID identifier for use in this context, along with any [Verifiable Credentials](https://w3c.github.io/vc-data-model/) required for membership.

Setup of the Trusted Context ends with the User providing the other parties a consent receipt.

```{image} ../_static/agents/trusted-context.png
```

### Relays based on Trusted Context

The previous diagram shows the logical construction of a trusted context. For greater clarity, there are Mediators and Relay Networks between the participants that route based on the trusted context. In particular, the User Data traverses the mediators and relay network:

```{image} ../_static/agents/trusted-context-with-relay.png
```

## Putting it all together

Trust contexts are realized by:

- Using DIDs as identifiers within the context
- Using Verifiable Credentials for user data representation
  \* Including the user's consent receipt, which will follow well known [standard schemas](https://kantarainitiative.org/file-downloads/consent-receipt-specification-v1-1-0/)
- A credential schema negotiated among the parties
- A relay network negotiated among the parties
- Mediators ensuring message delivery to the Agents

```{image} ../_static/agents/putting-together.png
```

Another scenario has the Issuer mediator delegating to the User mediator, in a manner similar to [UMA](https://kantarainitiative.org/confluence/display/uma/Home):

```{image} ../_static/agents/putting-together-uma.png
```
