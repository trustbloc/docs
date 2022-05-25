# Getting Started Tutorial

In this tutorial you'll start up two stand-alone Orb nodes (with in-memory database and message queue)
and create, update and resolve DIDs. Further in the tutorial you'll start up the Orb nodes along with a
[VCT](vct.html#verifiable-credential-transparency-vct) node, and you'll verify that the proofs
in an anchor linkset are in the VCT log.

## Setup

Install Docker Desktop from [here](https://docs.docker.com/get-docker/).

Clone the orb project:

```commandline
git clone git@github.com:trustbloc/orb.git
```

Build Orb and the CLI:

```commandline
cd orb

make clean orb-docker build-orb-cli-binaries extract-orb-cli-binaries
```

## Orb with no VCT

Start two Orb instances (orb1.local and orb2.local) and a command-line container:

```commandline
cd orb/samples/tutorial

docker-compose -f docker-compose-cli.yml -f ../docker/docker-compose-dev.yml up&
```

In another terminal, open an interactive Docker shell:

```commandline
docker exec -ti cli /bin/bash

cd ./orb
```

Query non-existing DID:

```commandline
orb-cli did resolve --did-uri=did:orb:http:orb1.local:uAAA:EiBFejklGvpC6hn--gZEoiDnEaeineV8xP7p0AcH1-N33A --verify-resolution-result-type=all
```

Response:

```commandline
Error: failed to resolve did: failed to resolve did: DID does not exist
 [orb-cli] 2022/05/24 16:06:31 UTC - main.main -> CRITICAL Failed to run orb-cli: failed to resolve did: failed to resolve did: DID does not exist
```

Create a DID at orb1:

```commandline
orb-cli did create --domain=http://orb1.local --publickey-file=./create_publickeys.json --service-file=./create_services.json --recoverykey-file=./recover_publickey.pem --updatekey-file=./update_publickey.pem --did-anchor-origin=http://orb1.local | jq
```

Response:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/ed25519-2018/v1",
    "https://w3id.org/security/suites/jws-2020/v1"
  ],
  "id": "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w",
  "verificationMethod": [
    {
      "controller": "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w",
      "id": "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#key1",
      "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
      "type": "Ed25519VerificationKey2018"
    },
    {
      "controller": "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w",
      "id": "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#key2",
      "publicKeyJwk": {
        "kty": "EC",
        "crv": "P-256",
        "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
        "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
      },
      "type": "JsonWebKey2020"
    }
  ],
  "service": [
    {
      "id": "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#svc1",
      "priority": 1,
      "recipientKeys": [
        "key1"
      ],
      "routingKeys": [
        "key1"
      ],
      "serviceEndpoint": {
        "uri": "https://example.com",
        "routingKeys": [
          "key1"
        ]
      },
      "type": "type1"
    },
    {
      "id": "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#svc2",
      "priority": 2,
      "recipientKeys": [
        "key2"
      ],
      "routingKeys": [
        "key2"
      ],
      "serviceEndpoint": {
        "uri": "https://example.com",
        "routingKeys": [
          "key2"
        ]
      },
      "type": "type2"
    }
  ],
  "authentication": [
    "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#key1",
    "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#key2"
  ]
}
```

Resolve the un-anchored DID at orb1:

**_NOTE:_**  Set DID_SUFFIX to the DID suffix in the _did create_ response.

```commandline
export DID_SUFFIX=EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w

orb-cli did resolve --domain=http://orb1.local --did-uri=did:orb:uAAA:${DID_SUFFIX} --verify-resolution-result-type=all | jq
```

Response:

```json
{
  "@context": [
    "https://w3id.org/did-resolution/v1"
  ],
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1",
      "https://w3id.org/security/suites/jws-2020/v1"
    ],
    "id": "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w",
    "verificationMethod": [
      {
        "controller": "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w",
        "id": "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#key1",
        "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
        "type": "Ed25519VerificationKey2018"
      },
      {
        "controller": "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w",
        "id": "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#key2",
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "P-256",
          "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
          "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
        },
        "type": "JsonWebKey2020"
      }
    ],
    "service": [
      {
        "id": "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#svc1",
        "priority": 1,
        "recipientKeys": [
          "key1"
        ],
        "routingKeys": [
          "key1"
        ],
        "serviceEndpoint": {
          "uri": "https://example.com",
          "routingKeys": [
            "key1"
          ]
        },
        "type": "type1"
      },
      {
        "id": "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#svc2",
        "priority": 2,
        "recipientKeys": [
          "key2"
        ],
        "routingKeys": [
          "key2"
        ],
        "serviceEndpoint": {
          "uri": "https://example.com",
          "routingKeys": [
            "key2"
          ]
        },
        "type": "type2"
      }
    ],
    "authentication": [
      "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#key1",
      "did:orb:uAAA:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#key2"
    ]
  },
  "didDocumentMetadata": {
    "versionId": "uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ",
    "canonicalId": "did:orb:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w",
    "equivalentId": [
      "did:orb:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w",
      "did:orb:hl:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQl9WX2xSY3J1dGZoRjJMdkVJdlJxc1IydU80al8xbmJ4NlQ2WC1wVVpRUFE:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w"
    ],
    "method": {
      "updateCommitment": "EiDsq7uyQDTYJss8JAV68EcVb1sJHXgUl8knESbzvN4RWw",
      "recoveryCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "published": true,
      "anchorOrigin": "http://orb1.local",
      "publishedOperations": [
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6ImtleTEiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiRWQyNTUxOSIsImt0eSI6Ik9LUCIsIngiOiJvMWJHMVU3RzNDTmJ0QUxNYWZVaUZPcThPRHJhVHlWVG1QdFJETzFRVVdnIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJFZDI1NTE5VmVyaWZpY2F0aW9uS2V5MjAxOCJ9LHsiaWQiOiJrZXkyIiwicHVibGljS2V5SndrIjp7ImNydiI6IlAtMjU2Iiwia3R5IjoiRUMiLCJ4IjoiYkdNOWFOdWZwS05QeGxreWFjVTFoR2hRWG1fYUM4aEl6U1ZlS0Rwd2pCdyIsInkiOiJQZmRtQ090SWRWWTJCNnVjUjRvUWt0NmV2UWRkWWhPeUhvRFlDYUkyQkpBIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJKc29uV2ViS2V5MjAyMCJ9XX0seyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJzdmMxIiwicHJpb3JpdHkiOjEsInJlY2lwaWVudEtleXMiOlsia2V5MSJdLCJyb3V0aW5nS2V5cyI6WyJrZXkxIl0sInNlcnZpY2VFbmRwb2ludCI6eyJyb3V0aW5nS2V5cyI6WyJrZXkxIl0sInVyaSI6Imh0dHBzOi8vZXhhbXBsZS5jb20ifSwidHlwZSI6InR5cGUxIn0seyJpZCI6InN2YzIiLCJwcmlvcml0eSI6MiwicmVjaXBpZW50S2V5cyI6WyJrZXkyIl0sInJvdXRpbmdLZXlzIjpbImtleTIiXSwic2VydmljZUVuZHBvaW50Ijp7InJvdXRpbmdLZXlzIjpbImtleTIiXSwidXJpIjoiaHR0cHM6Ly9leGFtcGxlLmNvbSJ9LCJ0eXBlIjoidHlwZTIifV19XSwidXBkYXRlQ29tbWl0bWVudCI6IkVpRHNxN3V5UURUWUpzczhKQVY2OEVjVmIxc0pIWGdVbDhrbkVTYnp2TjRSV3cifSwic3VmZml4RGF0YSI6eyJhbmNob3JPcmlnaW4iOiJodHRwOi8vb3JiMS5sb2NhbCIsImRlbHRhSGFzaCI6IkVpQm9rcG5MeDA3UEFzVS05QTY2VThCTkRIR0tLYUNSQzNmMFdhUkEtQ2RjVHciLCJyZWNvdmVyeUNvbW1pdG1lbnQiOiJFaUFWdGpYeC1XSFlmV2NBenpnYkhkY0FEaEYxS1ZTenB5NVRiX3hudTFjYnpBIn0sInR5cGUiOiJjcmVhdGUifQ==",
          "transactionTime": 1653410161,
          "type": "create",
          "anchorOrigin": "http://orb1.local",
          "canonicalReference": "uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ",
          "equivalentReferences": [
            "hl:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQl9WX2xSY3J1dGZoRjJMdkVJdlJxc1IydU80al8xbmJ4NlQ2WC1wVVpRUFE"
          ]
        }
      ]
    }
  }
}
```

Get the hash of the anchor from the _resolve_ response (from the "canonicalId" field in the metadata) and set the ANCHOR_HASH environment variable:

**_NOTE:_**  Set ANCHOR_HASH to the anchor hash of the DID in the _did resolve_ response.

```commandline
export ANCHOR_HASH=uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ
```

Resolve the canonical DID at orb1:

```commandline
orb-cli did resolve --domain=http://orb1.local --did-uri=did:orb:${ANCHOR_HASH}:${DID_SUFFIX} --verify-resolution-result-type=all | jq
```

Response:

```json
{
  "@context": [
    "https://w3id.org/did-resolution/v1"
  ],
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1",
      "https://w3id.org/security/suites/jws-2020/v1"
    ],
    "id": "did:orb:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w",
    "verificationMethod": [
      {
        "controller": "did:orb:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w",
        "id": "did:orb:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#key1",
        "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
        "type": "Ed25519VerificationKey2018"
      },
      {
        "controller": "did:orb:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w",
        "id": "did:orb:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#key2",
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "P-256",
          "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
          "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
        },
        "type": "JsonWebKey2020"
      }
    ],
    "service": [
      {
        "id": "did:orb:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#svc1",
        "priority": 1,
        "recipientKeys": [
          "key1"
        ],
        "routingKeys": [
          "key1"
        ],
        "serviceEndpoint": {
          "uri": "https://example.com",
          "routingKeys": [
            "key1"
          ]
        },
        "type": "type1"
      },
      {
        "id": "did:orb:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#svc2",
        "priority": 2,
        "recipientKeys": [
          "key2"
        ],
        "routingKeys": [
          "key2"
        ],
        "serviceEndpoint": {
          "uri": "https://example.com",
          "routingKeys": [
            "key2"
          ]
        },
        "type": "type2"
      }
    ],
    "authentication": [
      "did:orb:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#key1",
      "did:orb:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w#key2"
    ]
  },
  "didDocumentMetadata": {
    "versionId": "uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ",
    "canonicalId": "did:orb:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w",
    "equivalentId": [
      "did:orb:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w",
      "did:orb:hl:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQl9WX2xSY3J1dGZoRjJMdkVJdlJxc1IydU80al8xbmJ4NlQ2WC1wVVpRUFE:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w"
    ],
    "method": {
      "updateCommitment": "EiDsq7uyQDTYJss8JAV68EcVb1sJHXgUl8knESbzvN4RWw",
      "recoveryCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "published": true,
      "anchorOrigin": "http://orb1.local",
      "publishedOperations": [
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6ImtleTEiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiRWQyNTUxOSIsImt0eSI6Ik9LUCIsIngiOiJvMWJHMVU3RzNDTmJ0QUxNYWZVaUZPcThPRHJhVHlWVG1QdFJETzFRVVdnIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJFZDI1NTE5VmVyaWZpY2F0aW9uS2V5MjAxOCJ9LHsiaWQiOiJrZXkyIiwicHVibGljS2V5SndrIjp7ImNydiI6IlAtMjU2Iiwia3R5IjoiRUMiLCJ4IjoiYkdNOWFOdWZwS05QeGxreWFjVTFoR2hRWG1fYUM4aEl6U1ZlS0Rwd2pCdyIsInkiOiJQZmRtQ090SWRWWTJCNnVjUjRvUWt0NmV2UWRkWWhPeUhvRFlDYUkyQkpBIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJKc29uV2ViS2V5MjAyMCJ9XX0seyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJzdmMxIiwicHJpb3JpdHkiOjEsInJlY2lwaWVudEtleXMiOlsia2V5MSJdLCJyb3V0aW5nS2V5cyI6WyJrZXkxIl0sInNlcnZpY2VFbmRwb2ludCI6eyJyb3V0aW5nS2V5cyI6WyJrZXkxIl0sInVyaSI6Imh0dHBzOi8vZXhhbXBsZS5jb20ifSwidHlwZSI6InR5cGUxIn0seyJpZCI6InN2YzIiLCJwcmlvcml0eSI6MiwicmVjaXBpZW50S2V5cyI6WyJrZXkyIl0sInJvdXRpbmdLZXlzIjpbImtleTIiXSwic2VydmljZUVuZHBvaW50Ijp7InJvdXRpbmdLZXlzIjpbImtleTIiXSwidXJpIjoiaHR0cHM6Ly9leGFtcGxlLmNvbSJ9LCJ0eXBlIjoidHlwZTIifV19XSwidXBkYXRlQ29tbWl0bWVudCI6IkVpRHNxN3V5UURUWUpzczhKQVY2OEVjVmIxc0pIWGdVbDhrbkVTYnp2TjRSV3cifSwic3VmZml4RGF0YSI6eyJhbmNob3JPcmlnaW4iOiJodHRwOi8vb3JiMS5sb2NhbCIsImRlbHRhSGFzaCI6IkVpQm9rcG5MeDA3UEFzVS05QTY2VThCTkRIR0tLYUNSQzNmMFdhUkEtQ2RjVHciLCJyZWNvdmVyeUNvbW1pdG1lbnQiOiJFaUFWdGpYeC1XSFlmV2NBenpnYkhkY0FEaEYxS1ZTenB5NVRiX3hudTFjYnpBIn0sInR5cGUiOiJjcmVhdGUifQ==",
          "transactionTime": 1653410161,
          "type": "create",
          "anchorOrigin": "http://orb1.local",
          "canonicalReference": "uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ",
          "equivalentReferences": [
            "hl:uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQl9WX2xSY3J1dGZoRjJMdkVJdlJxc1IydU80al8xbmJ4NlQ2WC1wVVpRUFE"
          ]
        }
      ]
    }
  }
}
```

Resolve the DID at orb2. These should return 'not found' since orb2 is not following orb1:

```commandline
orb-cli did resolve --sidetree-url-resolution=http://orb2.local/sidetree/v1/identifiers --did-uri=did:orb:uAAA:${DID_SUFFIX} --verify-resolution-result-type=all
orb-cli did resolve --sidetree-url-resolution=http://orb2.local/sidetree/v1/identifiers --did-uri=did:orb:${ANCHOR_HASH}:${DID_SUFFIX} --verify-resolution-result-type=all
```

Response:

```commandline
Error: failed to resolve did: failed to resolve did: DID does not exist
 [orb-cli] 2022/05/24 16:39:50 UTC - main.main -> CRITICAL Failed to run orb-cli: failed to resolve did: failed to resolve did: DID does not exist
```

Resolve a discoverable DID at orb2. This should return 'not found' but when we wait a while it should be
available at orb2 since replication should have been triggered:

```commandline
orb-cli did resolve --sidetree-url-resolution=http://orb2.local/sidetree/v1/identifiers --did-uri=did:orb:http:orb1.local:${ANCHOR_HASH}:${DID_SUFFIX}  --verify-resolution-result-type=all | jq
```

Wait a couple of seconds and resolve the same DID at orb2. This should return the DID document:

```commandline
orb-cli did resolve --sidetree-url-resolution=http://orb2.local/sidetree/v1/identifiers --did-uri=did:orb:${ANCHOR_HASH}:${DID_SUFFIX}  --verify-resolution-result-type=all | jq
```

Have orb2 be a follower of orb1:

```commandline
orb-cli follower --outbox-url=http://orb2.local/services/orb/outbox --actor=http://orb2.local/services/orb --to http://orb1.local/services/orb --action=Follow
```

Response:

```commandline
success Follow id: "http://orb2.local/services/orb/activities/17a3f004-305a-44b2-84c6-7f5312ab007f"
```

Query the servers that orb2 is following:

```commandline
curl -s "http://orb2.local/services/orb/following?page=true&page-num=0" | jq
```

Response:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "http://orb2.local/services/orb/following?page=true&page-num=0",
  "items": [
    "http://orb1.local/services/orb"
  ],
  "totalItems": 1,
  "type": "CollectionPage"
}
```

Query the servers that are following orb1:

```commandline
curl -s "http://orb1.local/services/orb/followers?page=true&page-num=0" | jq
```

Response:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "http://orb1.local/services/orb/followers?page=true&page-num=0",
  "items": [
    "http://orb2.local/services/orb"
  ],
  "totalItems": 1,
  "type": "CollectionPage"
}
```

Create another DID at orb1:

```commandline
orb-cli did create --domain=http://orb1.local --publickey-file=./create_publickeys.json --service-file=./create_services2.json --recoverykey-file=./recover_publickey.pem --updatekey-file=./update_publickey.pem --did-anchor-origin=http://orb1.local | jq
```

Response:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/ed25519-2018/v1",
    "https://w3id.org/security/suites/jws-2020/v1"
  ],
  "id": "did:orb:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
  "verificationMethod": [
    {
      "controller": "did:orb:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
      "id": "did:orb:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key1",
      "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
      "type": "Ed25519VerificationKey2018"
    },
    {
      "controller": "did:orb:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
      "id": "did:orb:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key2",
      "publicKeyJwk": {
        "kty": "EC",
        "crv": "P-256",
        "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
        "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
      },
      "type": "JsonWebKey2020"
    }
  ],
  "service": [
    {
      "id": "did:orb:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#svc3",
      "priority": 1,
      "recipientKeys": [
        "key3"
      ],
      "routingKeys": [
        "key3"
      ],
      "serviceEndpoint": {
        "uri": "https://example.com",
        "routingKeys": [
          "key3"
        ]
      },
      "type": "type3"
    }
  ],
  "authentication": [
    "did:orb:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key1",
    "did:orb:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key2"
  ]
}
```

**_NOTE:_**  Set DID_SUFFIX to the DID suffix in the _did create_ response.

```commandline
export DID_SUFFIX=EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg

orb-cli did resolve --did-uri=did:orb:http:orb1.local:uAAA:${DID_SUFFIX} --verify-resolution-result-type=all | jq
```

Response:

```json
{
  "@context": [
    "https://w3id.org/did-resolution/v1"
  ],
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1",
      "https://w3id.org/security/suites/jws-2020/v1"
    ],
    "id": "did:orb:http:orb1.local:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
    "verificationMethod": [
      {
        "controller": "did:orb:http:orb1.local:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
        "id": "did:orb:http:orb1.local:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key1",
        "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
        "type": "Ed25519VerificationKey2018"
      },
      {
        "controller": "did:orb:http:orb1.local:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
        "id": "did:orb:http:orb1.local:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key2",
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "P-256",
          "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
          "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
        },
        "type": "JsonWebKey2020"
      }
    ],
    "service": [
      {
        "id": "did:orb:http:orb1.local:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#svc3",
        "priority": 1,
        "recipientKeys": [
          "key3"
        ],
        "routingKeys": [
          "key3"
        ],
        "serviceEndpoint": {
          "uri": "https://example.com",
          "routingKeys": [
            "key3"
          ]
        },
        "type": "type3"
      }
    ],
    "authentication": [
      "did:orb:http:orb1.local:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key1",
      "did:orb:http:orb1.local:uAAA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key2"
    ]
  },
  "didDocumentMetadata": {
    "versionId": "uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA",
    "canonicalId": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
    "equivalentId": [
      "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
      "did:orb:hl:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQUlTcUVXb0d3WGdLRXR4N0hndDFHWEpraDBqUHBxM3c2Ylo2XzhGSW81bkE:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg"
    ],
    "method": {
      "updateCommitment": "EiDsq7uyQDTYJss8JAV68EcVb1sJHXgUl8knESbzvN4RWw",
      "recoveryCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "published": true,
      "anchorOrigin": "http://orb1.local",
      "publishedOperations": [
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6ImtleTEiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiRWQyNTUxOSIsImt0eSI6Ik9LUCIsIngiOiJvMWJHMVU3RzNDTmJ0QUxNYWZVaUZPcThPRHJhVHlWVG1QdFJETzFRVVdnIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJFZDI1NTE5VmVyaWZpY2F0aW9uS2V5MjAxOCJ9LHsiaWQiOiJrZXkyIiwicHVibGljS2V5SndrIjp7ImNydiI6IlAtMjU2Iiwia3R5IjoiRUMiLCJ4IjoiYkdNOWFOdWZwS05QeGxreWFjVTFoR2hRWG1fYUM4aEl6U1ZlS0Rwd2pCdyIsInkiOiJQZmRtQ090SWRWWTJCNnVjUjRvUWt0NmV2UWRkWWhPeUhvRFlDYUkyQkpBIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJKc29uV2ViS2V5MjAyMCJ9XX0seyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJzdmMzIiwicHJpb3JpdHkiOjEsInJlY2lwaWVudEtleXMiOlsia2V5MyJdLCJyb3V0aW5nS2V5cyI6WyJrZXkzIl0sInNlcnZpY2VFbmRwb2ludCI6eyJyb3V0aW5nS2V5cyI6WyJrZXkzIl0sInVyaSI6Imh0dHBzOi8vZXhhbXBsZS5jb20ifSwidHlwZSI6InR5cGUzIn1dfV0sInVwZGF0ZUNvbW1pdG1lbnQiOiJFaURzcTd1eVFEVFlKc3M4SkFWNjhFY1ZiMXNKSFhnVWw4a25FU2J6dk40Uld3In0sInN1ZmZpeERhdGEiOnsiYW5jaG9yT3JpZ2luIjoiaHR0cDovL29yYjEubG9jYWwiLCJkZWx0YUhhc2giOiJFaUR6Ml8wSm1xU3BaRm5kalluYjJUVkZHX2lqdjc3anRIdDVjbTg1R0ZUNmVnIiwicmVjb3ZlcnlDb21taXRtZW50IjoiRWlBVnRqWHgtV0hZZldjQXp6Z2JIZGNBRGhGMUtWU3pweTVUYl94bnUxY2J6QSJ9LCJ0eXBlIjoiY3JlYXRlIn0=",
          "transactionTime": 1653410667,
          "type": "create",
          "anchorOrigin": "http://orb1.local",
          "canonicalReference": "uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA",
          "equivalentReferences": [
            "hl:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQUlTcUVXb0d3WGdLRXR4N0hndDFHWEpraDBqUHBxM3c2Ylo2XzhGSW81bkE"
          ]
        }
      ]
    }
  }
}
```

**_NOTE:_**  Set ANCHOR_HASH to the anchor hash of the DID in the _did resolve_ response.

```commandline
export ANCHOR_HASH=uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA
```

Resolve the canonical DID at orb2:

```commandline
orb-cli did resolve --domain=http://orb2.local --did-uri=did:orb:${ANCHOR_HASH}:${DID_SUFFIX} --verify-resolution-result-type=all | jq
```

Response:

```json
{
  "@context": [
    "https://w3id.org/did-resolution/v1"
  ],
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1",
      "https://w3id.org/security/suites/jws-2020/v1"
    ],
    "id": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
    "verificationMethod": [
      {
        "controller": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
        "id": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key1",
        "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
        "type": "Ed25519VerificationKey2018"
      },
      {
        "controller": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
        "id": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key2",
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "P-256",
          "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
          "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
        },
        "type": "JsonWebKey2020"
      }
    ],
    "service": [
      {
        "id": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#svc3",
        "priority": 1,
        "recipientKeys": [
          "key3"
        ],
        "routingKeys": [
          "key3"
        ],
        "serviceEndpoint": {
          "uri": "https://example.com",
          "routingKeys": [
            "key3"
          ]
        },
        "type": "type3"
      }
    ],
    "authentication": [
      "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key1",
      "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key2"
    ]
  },
  "didDocumentMetadata": {
    "versionId": "uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA",
    "canonicalId": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
    "equivalentId": [
      "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
      "did:orb:hl:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQUlTcUVXb0d3WGdLRXR4N0hndDFHWEpraDBqUHBxM3c2Ylo2XzhGSW81bkE:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg"
    ],
    "method": {
      "updateCommitment": "EiDsq7uyQDTYJss8JAV68EcVb1sJHXgUl8knESbzvN4RWw",
      "recoveryCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "published": true,
      "anchorOrigin": "http://orb1.local",
      "publishedOperations": [
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6ImtleTEiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiRWQyNTUxOSIsImt0eSI6Ik9LUCIsIngiOiJvMWJHMVU3RzNDTmJ0QUxNYWZVaUZPcThPRHJhVHlWVG1QdFJETzFRVVdnIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJFZDI1NTE5VmVyaWZpY2F0aW9uS2V5MjAxOCJ9LHsiaWQiOiJrZXkyIiwicHVibGljS2V5SndrIjp7ImNydiI6IlAtMjU2Iiwia3R5IjoiRUMiLCJ4IjoiYkdNOWFOdWZwS05QeGxreWFjVTFoR2hRWG1fYUM4aEl6U1ZlS0Rwd2pCdyIsInkiOiJQZmRtQ090SWRWWTJCNnVjUjRvUWt0NmV2UWRkWWhPeUhvRFlDYUkyQkpBIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJKc29uV2ViS2V5MjAyMCJ9XX0seyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJzdmMzIiwicHJpb3JpdHkiOjEsInJlY2lwaWVudEtleXMiOlsia2V5MyJdLCJyb3V0aW5nS2V5cyI6WyJrZXkzIl0sInNlcnZpY2VFbmRwb2ludCI6eyJyb3V0aW5nS2V5cyI6WyJrZXkzIl0sInVyaSI6Imh0dHBzOi8vZXhhbXBsZS5jb20ifSwidHlwZSI6InR5cGUzIn1dfV0sInVwZGF0ZUNvbW1pdG1lbnQiOiJFaURzcTd1eVFEVFlKc3M4SkFWNjhFY1ZiMXNKSFhnVWw4a25FU2J6dk40Uld3In0sInN1ZmZpeERhdGEiOnsiYW5jaG9yT3JpZ2luIjoiaHR0cDovL29yYjEubG9jYWwiLCJkZWx0YUhhc2giOiJFaUR6Ml8wSm1xU3BaRm5kalluYjJUVkZHX2lqdjc3anRIdDVjbTg1R0ZUNmVnIiwicmVjb3ZlcnlDb21taXRtZW50IjoiRWlBVnRqWHgtV0hZZldjQXp6Z2JIZGNBRGhGMUtWU3pweTVUYl94bnUxY2J6QSJ9LCJ0eXBlIjoiY3JlYXRlIn0=",
          "transactionTime": 1653410667,
          "type": "create",
          "anchorOrigin": "http://orb1.local",
          "canonicalReference": "uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA",
          "equivalentReferences": [
            "hl:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQUlTcUVXb0d3WGdLRXR4N0hndDFHWEpraDBqUHBxM3c2Ylo2XzhGSW81bkE"
          ]
        }
      ]
    }
  }
}
```

Look at orb2's inbox:

```commandline
curl -s "http://orb2.local/services/orb/inbox?page=true&page-num=0" | jq
```

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "http://orb2.local/services/orb/inbox?page=true&page-num=0",
  "orderedItems": [
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "http://orb1.local/services/orb",
      "id": "http://orb1.local/services/orb/activities/fc4eeabf-5fad-484d-a818-7b8aac3fd0ca",
      "object": {
        "@context": "https://www.w3.org/ns/activitystreams",
        "actor": "http://orb2.local/services/orb",
        "id": "http://orb2.local/services/orb/activities/17a3f004-305a-44b2-84c6-7f5312ab007f",
        "object": "http://orb1.local/services/orb",
        "to": "http://orb1.local/services/orb",
        "type": "Follow"
      },
      "to": "http://orb2.local/services/orb",
      "type": "Accept"
    },
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "http://orb1.local/services/orb",
      "id": "http://orb1.local/services/orb/activities/9265e039-4e40-41d8-8177-e89ce9432aea",
      "object": {
        "@context": "https://w3id.org/activityanchors/v1",
        "object": {
          "linkset": [
            {
              "anchor": "hl:uEiAAbEVaZdmpOK-902szJSWr0SAXGv6shtq5yOxbJ2mM4w",
              "author": "http://orb1.local/services/orb",
              "original": [
                {
                  "href": "data:application/gzip;base64,H4sIAAAAAAAA/yzN0W6CMBSA4Xc5uyXWBTVb78isS5ibm7CoW7xAqHCgUDhtYYvh3RcXb//kz3cBhU1lpAX+fYGkSQtNwKFQ3AlcCtF/VCXS3P8Nl36xOOyysp4+L37Wj3L9XtcPs9ddtI2rKMrBg8TZ221tyxnTdLqfKJ0mihlJPabSXBt4gFbW/2BB8gwcMsy4phN3QRBwgYEot+Jp3w6rpFsdBh3G871zn3E3YB690Wajupn5elHUhDmMRw9a0mdU8kYbztjgYzbRlF/Bu34K43H8CwAA//+UYLw46wAAAA==",
                  "type": "application/linkset+json"
                }
              ],
              "profile": "https://w3id.org/orb#v0",
              "related": [
                {
                  "href": "data:application/gzip;base64,H4sIAAAAAAAA/zzNzW6CMAAA4HfprjoRxCg3HYhhApNiURcP1KKtUIul1r/w7st22AN8+V6gYueyKRRwvl8gP++pkMABtHKuHptMsIfyLeF1/NkdG2bzDGAmDThZ+3rYUHWxH/EdByYPBzfQAbUUB1YVv1ypunF6vZvFyLuQx56Q+E0boAM0y/8mKovD/+N6nl6WJyZt6xG4Fh1uMnLihj+8L8bF4ovz0SDMYJKWEB6dq1h2p4WHBJknt5iNNLYCFkK7wWZE8cf4tFmHmqxQnXhoRqqZzHn1iNLIhuXymbvbLTKTC06n8+hMwzhD4crvq9hNItSPIPJVujL3oN21u/YnAAD//91tM4IcAQAA",
                  "type": "application/linkset+json"
                }
              ],
              "replies": [
                {
                  "href": "data:application/gzip;base64,H4sIAAAAAAAA/4yS35KaMBSH3yW9FQkh6JKr0i7V6uI/XFe309kJSZRYBEwC6O7su3fYttqLttPrfOf7nZNzXsB7VuRGnAwgX0BqTKmJbTdN023cbqF2NoLOjc2U4CI3kmbarh3QuYKu5G+YFqxS0pxtXUkjtL1vtIUggv+JC448z/EvJV874JoZV8leMAMISDNShTIIknBFH/mhnI4tHyL9PIofFIyD9aDu6dQcvfP0lIzQIcIN6ADJ20pjSmLbhUqcblYwmtk1sz3fTbz+jW+5EHILI7G1/J7ft+hWeBhynzHutgKtK5ozcUuNAAQgiJAFPQvhpdMjGBPU77qu63no5vEnLdSfIkEHlKootoC8tNNRI/jfdf0edlodLw5U5v/SzSpVFrrtjGotlJFFHgmTFvwXsKJZ1T7LWZZBPGfqvEg243k89fVgNNuj2aBg35aHOtnj2L+Xi+P21K+f1rhK8edPqd7o+TxcrIcBhnLHpni4P0anO47D8gg/BO0Pm3PZ+sMfS4zlLqemUqJdJuiAWii5lYz+1hgBXHLSiIRcx3kXWHM524WL8fIpfxhuaK0mt2qyHLDmfuoNe6NJ9LyMIrW+oxi8XkJXb3aaZOLj5WDA6/cAAAD//7xmqXXXAgAA",
                  "type": "application/ld+json"
                }
              ]
            }
          ]
        },
        "type": "AnchorEvent",
        "url": "hl:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQUlTcUVXb0d3WGdLRXR4N0hndDFHWEpraDBqUHBxM3c2Ylo2XzhGSW81bkE"
      },
      "published": "2022-05-24T16:44:27.3549742Z",
      "to": [
        "http://orb1.local/services/orb/followers",
        "https://www.w3.org/ns/activitystreams#Public"
      ],
      "type": "Create"
    }
  ],
  "totalItems": 2,
  "type": "OrderedCollectionPage"
}
```

Update the DID:

```commandline
orb-cli did update --domain=http://orb1.local --did-uri=did:orb:${ANCHOR_HASH}:${DID_SUFFIX} --add-publickey-file=./update_publickeys.json --signingkey-file=./update_privatekey.pem --nextupdatekey-file=./nextupdate_publickey.pem --tls-systemcertpool=true
```

Response:

```commandline
successfully updated DID did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJgroot@af0b0d3c551a
```

Resolve the DID on orb1:

```commandline
orb-cli did resolve --domain=http://orb1.local --did-uri=did:orb:${ANCHOR_HASH}:${DID_SUFFIX} --verify-resolution-result-type=all | jq
```

Response:

```json
{
  "@context": [
    "https://w3id.org/did-resolution/v1"
  ],
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1",
      "https://w3id.org/security/suites/jws-2020/v1"
    ],
    "id": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
    "verificationMethod": [
      {
        "controller": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
        "id": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key3",
        "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
        "type": "Ed25519VerificationKey2018"
      },
      {
        "controller": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
        "id": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key4",
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "P-256",
          "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
          "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
        },
        "type": "JsonWebKey2020"
      }
    ],
    "authentication": [
      "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key3",
      "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key4"
    ]
  },
  "didDocumentMetadata": {
    "versionId": "uEiDlcPcvrVs73BSJWFFrTHyKy7u6gYxw2rA1hmxn5dAA2g",
    "canonicalId": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
    "equivalentId": [
      "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
      "did:orb:hl:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQUlTcUVXb0d3WGdLRXR4N0hndDFHWEpraDBqUHBxM3c2Ylo2XzhGSW81bkE:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg"
    ],
    "method": {
      "updateCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "recoveryCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "published": true,
      "anchorOrigin": "http://orb1.local",
      "publishedOperations": [
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6ImtleTEiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiRWQyNTUxOSIsImt0eSI6Ik9LUCIsIngiOiJvMWJHMVU3RzNDTmJ0QUxNYWZVaUZPcThPRHJhVHlWVG1QdFJETzFRVVdnIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJFZDI1NTE5VmVyaWZpY2F0aW9uS2V5MjAxOCJ9LHsiaWQiOiJrZXkyIiwicHVibGljS2V5SndrIjp7ImNydiI6IlAtMjU2Iiwia3R5IjoiRUMiLCJ4IjoiYkdNOWFOdWZwS05QeGxreWFjVTFoR2hRWG1fYUM4aEl6U1ZlS0Rwd2pCdyIsInkiOiJQZmRtQ090SWRWWTJCNnVjUjRvUWt0NmV2UWRkWWhPeUhvRFlDYUkyQkpBIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJKc29uV2ViS2V5MjAyMCJ9XX0seyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJzdmMzIiwicHJpb3JpdHkiOjEsInJlY2lwaWVudEtleXMiOlsia2V5MyJdLCJyb3V0aW5nS2V5cyI6WyJrZXkzIl0sInNlcnZpY2VFbmRwb2ludCI6eyJyb3V0aW5nS2V5cyI6WyJrZXkzIl0sInVyaSI6Imh0dHBzOi8vZXhhbXBsZS5jb20ifSwidHlwZSI6InR5cGUzIn1dfV0sInVwZGF0ZUNvbW1pdG1lbnQiOiJFaURzcTd1eVFEVFlKc3M4SkFWNjhFY1ZiMXNKSFhnVWw4a25FU2J6dk40Uld3In0sInN1ZmZpeERhdGEiOnsiYW5jaG9yT3JpZ2luIjoiaHR0cDovL29yYjEubG9jYWwiLCJkZWx0YUhhc2giOiJFaUR6Ml8wSm1xU3BaRm5kalluYjJUVkZHX2lqdjc3anRIdDVjbTg1R0ZUNmVnIiwicmVjb3ZlcnlDb21taXRtZW50IjoiRWlBVnRqWHgtV0hZZldjQXp6Z2JIZGNBRGhGMUtWU3pweTVUYl94bnUxY2J6QSJ9LCJ0eXBlIjoiY3JlYXRlIn0=",
          "transactionTime": 1653410667,
          "type": "create",
          "anchorOrigin": "http://orb1.local",
          "canonicalReference": "uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA",
          "equivalentReferences": [
            "hl:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQUlTcUVXb0d3WGdLRXR4N0hndDFHWEpraDBqUHBxM3c2Ylo2XzhGSW81bkE"
          ]
        },
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJyZW1vdmUtcHVibGljLWtleXMiLCJpZHMiOlsia2V5MSIsImtleTIiXX0seyJhY3Rpb24iOiJyZW1vdmUtc2VydmljZXMiLCJpZHMiOlsic3ZjMyJdfSx7ImFjdGlvbiI6ImFkZC1wdWJsaWMta2V5cyIsInB1YmxpY0tleXMiOlt7ImlkIjoia2V5MyIsInB1YmxpY0tleUp3ayI6eyJjcnYiOiJFZDI1NTE5Iiwia3R5IjoiT0tQIiwieCI6Im8xYkcxVTdHM0NOYnRBTE1hZlVpRk9xOE9EcmFUeVZUbVB0UkRPMVFVV2cifSwicHVycG9zZXMiOlsiYXV0aGVudGljYXRpb24iXSwidHlwZSI6IkVkMjU1MTlWZXJpZmljYXRpb25LZXkyMDE4In0seyJpZCI6ImtleTQiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiUC0yNTYiLCJrdHkiOiJFQyIsIngiOiJiR005YU51ZnBLTlB4bGt5YWNVMWhHaFFYbV9hQzhoSXpTVmVLRHB3akJ3IiwieSI6IlBmZG1DT3RJZFZZMkI2dWNSNG9Ra3Q2ZXZRZGRZaE95SG9EWUNhSTJCSkEifSwicHVycG9zZXMiOlsiYXV0aGVudGljYXRpb24iXSwidHlwZSI6Ikpzb25XZWJLZXkyMDIwIn1dfV0sInVwZGF0ZUNvbW1pdG1lbnQiOiJFaUFWdGpYeC1XSFlmV2NBenpnYkhkY0FEaEYxS1ZTenB5NVRiX3hudTFjYnpBIn0sImRpZFN1ZmZpeCI6IkVpQUVqUkVDWHB3RmFxRll3b0pUNVh1dVVUcXdpZ1NOck9PbHE0c1pLbHJuSmciLCJyZXZlYWxWYWx1ZSI6IkVpREZiaFZsZk5Qb3FFS3N4Ul9LRW1PdzJjYlZHcGtvcnFxWnRER0FnU3kwdmciLCJzaWduZWREYXRhIjoiZXlKaGJHY2lPaUpGWkVSVFFTSXNJbXRwWkNJNklqZzVObVppWlRreExXVXpNelF0TkRjNE5DMWlZV1EzTFdFd09HRm1ZemRpWkRNMVppSjkuZXlKa1pXeDBZVWhoYzJnaU9pSkZhVUZtVUhOUVUzWkNNemM1UlVGTlpWWXpVMjU0WkhGTmVrUmphM28zZG01b1RWSXpXR3c1Ykd0bGJXOW5JaXdpZFhCa1lYUmxTMlY1SWpwN0ltTnlkaUk2SWtWa01qVTFNVGtpTENKcmRIa2lPaUpQUzFBaUxDSjRJam9pTTBaTFJUSnlPVFJNYVZJd1IyWnhWRlJVT1hkMGRFMTVWV1pGVVd0Zk4zbE5kRWN5U2tseGVscDVTU0lzSW5raU9pSWlmWDAuUDR3NS1YdnE0YVlheWZ5dWNaWm9mTlg0ckNpaXZsb0gwb0Fja2pVd1ludmFTNGNlTkpaYjh1TXpNZWowTmZQWko5WHdXeVMyTFlwcUR0aTUwajhRQ1EiLCJ0eXBlIjoidXBkYXRlIn0=",
          "transactionTime": 1653410813,
          "type": "update",
          "canonicalReference": "uEiDlcPcvrVs73BSJWFFrTHyKy7u6gYxw2rA1hmxn5dAA2g",
          "equivalentReferences": [
            "hl:uEiDlcPcvrVs73BSJWFFrTHyKy7u6gYxw2rA1hmxn5dAA2g:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpRGxjUGN2clZzNzNCU0pXRkZyVEh5S3k3dTZnWXh3MnJBMWhteG41ZEFBMmc"
          ]
        }
      ]
    }
  }
}
```

Resolve the DID on orb2:

```commandline
orb-cli did resolve --domain=http://orb1.local --did-uri=did:orb:${ANCHOR_HASH}:${DID_SUFFIX} --verify-resolution-result-type=all | jq
```

Response:

```json
{
  "@context": [
    "https://w3id.org/did-resolution/v1"
  ],
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1",
      "https://w3id.org/security/suites/jws-2020/v1"
    ],
    "id": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
    "verificationMethod": [
      {
        "controller": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
        "id": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key3",
        "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
        "type": "Ed25519VerificationKey2018"
      },
      {
        "controller": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
        "id": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key4",
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "P-256",
          "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
          "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
        },
        "type": "JsonWebKey2020"
      }
    ],
    "authentication": [
      "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key3",
      "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg#key4"
    ]
  },
  "didDocumentMetadata": {
    "versionId": "uEiDlcPcvrVs73BSJWFFrTHyKy7u6gYxw2rA1hmxn5dAA2g",
    "canonicalId": "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
    "equivalentId": [
      "did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg",
      "did:orb:hl:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQUlTcUVXb0d3WGdLRXR4N0hndDFHWEpraDBqUHBxM3c2Ylo2XzhGSW81bkE:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg"
    ],
    "method": {
      "updateCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "recoveryCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "published": true,
      "anchorOrigin": "http://orb1.local",
      "publishedOperations": [
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6ImtleTEiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiRWQyNTUxOSIsImt0eSI6Ik9LUCIsIngiOiJvMWJHMVU3RzNDTmJ0QUxNYWZVaUZPcThPRHJhVHlWVG1QdFJETzFRVVdnIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJFZDI1NTE5VmVyaWZpY2F0aW9uS2V5MjAxOCJ9LHsiaWQiOiJrZXkyIiwicHVibGljS2V5SndrIjp7ImNydiI6IlAtMjU2Iiwia3R5IjoiRUMiLCJ4IjoiYkdNOWFOdWZwS05QeGxreWFjVTFoR2hRWG1fYUM4aEl6U1ZlS0Rwd2pCdyIsInkiOiJQZmRtQ090SWRWWTJCNnVjUjRvUWt0NmV2UWRkWWhPeUhvRFlDYUkyQkpBIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJKc29uV2ViS2V5MjAyMCJ9XX0seyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJzdmMzIiwicHJpb3JpdHkiOjEsInJlY2lwaWVudEtleXMiOlsia2V5MyJdLCJyb3V0aW5nS2V5cyI6WyJrZXkzIl0sInNlcnZpY2VFbmRwb2ludCI6eyJyb3V0aW5nS2V5cyI6WyJrZXkzIl0sInVyaSI6Imh0dHBzOi8vZXhhbXBsZS5jb20ifSwidHlwZSI6InR5cGUzIn1dfV0sInVwZGF0ZUNvbW1pdG1lbnQiOiJFaURzcTd1eVFEVFlKc3M4SkFWNjhFY1ZiMXNKSFhnVWw4a25FU2J6dk40Uld3In0sInN1ZmZpeERhdGEiOnsiYW5jaG9yT3JpZ2luIjoiaHR0cDovL29yYjEubG9jYWwiLCJkZWx0YUhhc2giOiJFaUR6Ml8wSm1xU3BaRm5kalluYjJUVkZHX2lqdjc3anRIdDVjbTg1R0ZUNmVnIiwicmVjb3ZlcnlDb21taXRtZW50IjoiRWlBVnRqWHgtV0hZZldjQXp6Z2JIZGNBRGhGMUtWU3pweTVUYl94bnUxY2J6QSJ9LCJ0eXBlIjoiY3JlYXRlIn0=",
          "transactionTime": 1653410667,
          "type": "create",
          "anchorOrigin": "http://orb1.local",
          "canonicalReference": "uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA",
          "equivalentReferences": [
            "hl:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQUlTcUVXb0d3WGdLRXR4N0hndDFHWEpraDBqUHBxM3c2Ylo2XzhGSW81bkE"
          ]
        },
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJyZW1vdmUtcHVibGljLWtleXMiLCJpZHMiOlsia2V5MSIsImtleTIiXX0seyJhY3Rpb24iOiJyZW1vdmUtc2VydmljZXMiLCJpZHMiOlsic3ZjMyJdfSx7ImFjdGlvbiI6ImFkZC1wdWJsaWMta2V5cyIsInB1YmxpY0tleXMiOlt7ImlkIjoia2V5MyIsInB1YmxpY0tleUp3ayI6eyJjcnYiOiJFZDI1NTE5Iiwia3R5IjoiT0tQIiwieCI6Im8xYkcxVTdHM0NOYnRBTE1hZlVpRk9xOE9EcmFUeVZUbVB0UkRPMVFVV2cifSwicHVycG9zZXMiOlsiYXV0aGVudGljYXRpb24iXSwidHlwZSI6IkVkMjU1MTlWZXJpZmljYXRpb25LZXkyMDE4In0seyJpZCI6ImtleTQiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiUC0yNTYiLCJrdHkiOiJFQyIsIngiOiJiR005YU51ZnBLTlB4bGt5YWNVMWhHaFFYbV9hQzhoSXpTVmVLRHB3akJ3IiwieSI6IlBmZG1DT3RJZFZZMkI2dWNSNG9Ra3Q2ZXZRZGRZaE95SG9EWUNhSTJCSkEifSwicHVycG9zZXMiOlsiYXV0aGVudGljYXRpb24iXSwidHlwZSI6Ikpzb25XZWJLZXkyMDIwIn1dfV0sInVwZGF0ZUNvbW1pdG1lbnQiOiJFaUFWdGpYeC1XSFlmV2NBenpnYkhkY0FEaEYxS1ZTenB5NVRiX3hudTFjYnpBIn0sImRpZFN1ZmZpeCI6IkVpQUVqUkVDWHB3RmFxRll3b0pUNVh1dVVUcXdpZ1NOck9PbHE0c1pLbHJuSmciLCJyZXZlYWxWYWx1ZSI6IkVpREZiaFZsZk5Qb3FFS3N4Ul9LRW1PdzJjYlZHcGtvcnFxWnRER0FnU3kwdmciLCJzaWduZWREYXRhIjoiZXlKaGJHY2lPaUpGWkVSVFFTSXNJbXRwWkNJNklqZzVObVppWlRreExXVXpNelF0TkRjNE5DMWlZV1EzTFdFd09HRm1ZemRpWkRNMVppSjkuZXlKa1pXeDBZVWhoYzJnaU9pSkZhVUZtVUhOUVUzWkNNemM1UlVGTlpWWXpVMjU0WkhGTmVrUmphM28zZG01b1RWSXpXR3c1Ykd0bGJXOW5JaXdpZFhCa1lYUmxTMlY1SWpwN0ltTnlkaUk2SWtWa01qVTFNVGtpTENKcmRIa2lPaUpQUzFBaUxDSjRJam9pTTBaTFJUSnlPVFJNYVZJd1IyWnhWRlJVT1hkMGRFMTVWV1pGVVd0Zk4zbE5kRWN5U2tseGVscDVTU0lzSW5raU9pSWlmWDAuUDR3NS1YdnE0YVlheWZ5dWNaWm9mTlg0ckNpaXZsb0gwb0Fja2pVd1ludmFTNGNlTkpaYjh1TXpNZWowTmZQWko5WHdXeVMyTFlwcUR0aTUwajhRQ1EiLCJ0eXBlIjoidXBkYXRlIn0=",
          "transactionTime": 1653410813,
          "type": "update",
          "canonicalReference": "uEiDlcPcvrVs73BSJWFFrTHyKy7u6gYxw2rA1hmxn5dAA2g",
          "equivalentReferences": [
            "hl:uEiDlcPcvrVs73BSJWFFrTHyKy7u6gYxw2rA1hmxn5dAA2g:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpRGxjUGN2clZzNzNCU0pXRkZyVEh5S3k3dTZnWXh3MnJBMWhteG41ZEFBMmc"
          ]
        }
      ]
    }
  }
}
```

Shut down the environment:

```commandline
docker-compose -f docker-compose-cli.yml -f ../docker/docker-compose-dev.yml down
```

## Orb with VCT

Start two Orb instances (orb1.local and orb2.local), [VCT](vct.html#verifiable-credential-transparency-vct)
(includes the Google Trillian server and signer) and a command-line container:

```commandline
cd orb/samples/tutorial

docker-compose -f docker-compose-cli.yml -f ../docker/docker-compose-dev-vct.yml up&
```

In another terminal, open an interactive Docker shell:

```commandline
docker exec -ti cli /bin/bash

cd ./orb
```

Add a VCT log to orb1:

```commandline
orb-cli log update --url http://orb1.local/log --log http://orb.vct:8077/maple2022
```

Response:

```commandline
Domain log has successfully been updated.
```

Create a DID at orb1:

```commandline
orb-cli did create --domain=http://orb1.local --publickey-file=./create_publickeys.json --service-file=./create_services.json --recoverykey-file=./recover_publickey.pem --updatekey-file=./update_publickey.pem --did-anchor-origin=http://orb1.local | jq
```

Response:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/jws-2020/v1",
    "https://w3id.org/security/suites/ed25519-2018/v1"
  ],
  "id": "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg",
  "verificationMethod": [
    {
      "controller": "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg",
      "id": "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg#key2",
      "publicKeyJwk": {
        "kty": "EC",
        "crv": "P-256",
        "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
        "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
      },
      "type": "JsonWebKey2020"
    },
    {
      "controller": "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg",
      "id": "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg#key1",
      "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
      "type": "Ed25519VerificationKey2018"
    }
  ],
  "service": [
    {
      "id": "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg#svc1",
      "priority": 1,
      "recipientKeys": [
        "key1"
      ],
      "routingKeys": [
        "key1"
      ],
      "serviceEndpoint": {
        "uri": "https://example.com",
        "routingKeys": [
          "key1"
        ]
      },
      "type": "type1"
    },
    {
      "id": "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg#svc2",
      "priority": 2,
      "recipientKeys": [
        "key2"
      ],
      "routingKeys": [
        "key2"
      ],
      "serviceEndpoint": {
        "uri": "https://example.com",
        "routingKeys": [
          "key2"
        ]
      },
      "type": "type2"
    }
  ],
  "authentication": [
    "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg#key2",
    "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg#key1"
  ]
}
```

Resolve the DID at orb1 to get the anchor hash:

**_NOTE:_**  Set DID_SUFFIX to the DID suffix in the _did create_ response.

```commandline
export DID_SUFFIX=EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg

orb-cli did resolve --domain=http://orb1.local --did-uri=did:orb:uAAA:${DID_SUFFIX} --verify-resolution-result-type=all | jq
```

Response:

```json
{
  "@context": [
    "https://w3id.org/did-resolution/v1"
  ],
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/jws-2020/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1"
    ],
    "id": "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg",
    "verificationMethod": [
      {
        "controller": "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg",
        "id": "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg#key2",
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "P-256",
          "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
          "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
        },
        "type": "JsonWebKey2020"
      },
      {
        "controller": "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg",
        "id": "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg#key1",
        "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
        "type": "Ed25519VerificationKey2018"
      }
    ],
    "service": [
      {
        "id": "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg#svc1",
        "priority": 1,
        "recipientKeys": [
          "key1"
        ],
        "routingKeys": [
          "key1"
        ],
        "serviceEndpoint": {
          "uri": "https://example.com",
          "routingKeys": [
            "key1"
          ]
        },
        "type": "type1"
      },
      {
        "id": "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg#svc2",
        "priority": 2,
        "recipientKeys": [
          "key2"
        ],
        "routingKeys": [
          "key2"
        ],
        "serviceEndpoint": {
          "uri": "https://example.com",
          "routingKeys": [
            "key2"
          ]
        },
        "type": "type2"
      }
    ],
    "authentication": [
      "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg#key2",
      "did:orb:uAAA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg#key1"
    ]
  },
  "didDocumentMetadata": {
    "versionId": "uEiBtg3HASdLSUHLYds6givvkjJX_DEb8cqONqpKuq_Y_aA",
    "canonicalId": "did:orb:uEiBtg3HASdLSUHLYds6givvkjJX_DEb8cqONqpKuq_Y_aA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg",
    "equivalentId": [
      "did:orb:uEiBtg3HASdLSUHLYds6givvkjJX_DEb8cqONqpKuq_Y_aA:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg",
      "did:orb:hl:uEiBtg3HASdLSUHLYds6givvkjJX_DEb8cqONqpKuq_Y_aA:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQnRnM0hBU2RMU1VITFlkczZnaXZ2a2pKWF9ERWI4Y3FPTnFwS3VxX1lfYUE:EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg"
    ],
    "method": {
      "updateCommitment": "EiDsq7uyQDTYJss8JAV68EcVb1sJHXgUl8knESbzvN4RWw",
      "recoveryCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "published": true,
      "anchorOrigin": "http://orb1.local",
      "publishedOperations": [
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6ImtleTIiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiUC0yNTYiLCJrdHkiOiJFQyIsIngiOiJiR005YU51ZnBLTlB4bGt5YWNVMWhHaFFYbV9hQzhoSXpTVmVLRHB3akJ3IiwieSI6IlBmZG1DT3RJZFZZMkI2dWNSNG9Ra3Q2ZXZRZGRZaE95SG9EWUNhSTJCSkEifSwicHVycG9zZXMiOlsiYXV0aGVudGljYXRpb24iXSwidHlwZSI6Ikpzb25XZWJLZXkyMDIwIn0seyJpZCI6ImtleTEiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiRWQyNTUxOSIsImt0eSI6Ik9LUCIsIngiOiJvMWJHMVU3RzNDTmJ0QUxNYWZVaUZPcThPRHJhVHlWVG1QdFJETzFRVVdnIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJFZDI1NTE5VmVyaWZpY2F0aW9uS2V5MjAxOCJ9XX0seyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJzdmMxIiwicHJpb3JpdHkiOjEsInJlY2lwaWVudEtleXMiOlsia2V5MSJdLCJyb3V0aW5nS2V5cyI6WyJrZXkxIl0sInNlcnZpY2VFbmRwb2ludCI6eyJyb3V0aW5nS2V5cyI6WyJrZXkxIl0sInVyaSI6Imh0dHBzOi8vZXhhbXBsZS5jb20ifSwidHlwZSI6InR5cGUxIn0seyJpZCI6InN2YzIiLCJwcmlvcml0eSI6MiwicmVjaXBpZW50S2V5cyI6WyJrZXkyIl0sInJvdXRpbmdLZXlzIjpbImtleTIiXSwic2VydmljZUVuZHBvaW50Ijp7InJvdXRpbmdLZXlzIjpbImtleTIiXSwidXJpIjoiaHR0cHM6Ly9leGFtcGxlLmNvbSJ9LCJ0eXBlIjoidHlwZTIifV19XSwidXBkYXRlQ29tbWl0bWVudCI6IkVpRHNxN3V5UURUWUpzczhKQVY2OEVjVmIxc0pIWGdVbDhrbkVTYnp2TjRSV3cifSwic3VmZml4RGF0YSI6eyJhbmNob3JPcmlnaW4iOiJodHRwOi8vb3JiMS5sb2NhbCIsImRlbHRhSGFzaCI6IkVpQUE0clRFcTdIUFFIdVNIVGFTSlVmWTFMV2x3cmkzTVlGZUtyYVZBWUN6OUEiLCJyZWNvdmVyeUNvbW1pdG1lbnQiOiJFaUFWdGpYeC1XSFlmV2NBenpnYkhkY0FEaEYxS1ZTenB5NVRiX3hudTFjYnpBIn0sInR5cGUiOiJjcmVhdGUifQ==",
          "transactionTime": 1653411114,
          "type": "create",
          "anchorOrigin": "http://orb1.local",
          "canonicalReference": "uEiBtg3HASdLSUHLYds6givvkjJX_DEb8cqONqpKuq_Y_aA",
          "equivalentReferences": [
            "hl:uEiBtg3HASdLSUHLYds6givvkjJX_DEb8cqONqpKuq_Y_aA:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQnRnM0hBU2RMU1VITFlkczZnaXZ2a2pKWF9ERWI4Y3FPTnFwS3VxX1lfYUE"
          ]
        }
      ]
    }
  }
}
```

Get hash of anchor from the _resolve_ response (from the "canonicalId" field in the metadata) and set the ANCHOR_HASH environment variable.
Verify that the proof(s) in the anchor linkset are in the VCT log:

**_NOTE:_**  Set ANCHOR_HASH to the anchor hash of the DID in the _did resolve_ response.

```commandline
export ANCHOR_HASH=uEiBtg3HASdLSUHLYds6givvkjJX_DEb8cqONqpKuq_Y_aA

orb-cli vct verify --cas-url=http://orb1.local/cas --anchor=${ANCHOR_HASH}
```

Response:

```json
[
  {
    "domain": "http://orb.vct:8077/maple2022",
    "proofValue": "t5WHClGc4DJLx-BpiBHPdBRbqtTOHVaN5IpRAq_AsHfT1DTHVRLSRIfTnEa35BbIYsK7O5Bqdn2O0_oOs7fcDQ",
    "found": true,
    "leafIndex": 0,
    "auditPath": null
  }
]
```

You'll notice that _leafIndex_ is 0 and _auditPath_ is empty since there's only one entry in the Merkle tree.

Create another DID at orb1 and use `orb-cli vct verify` to verify that the proofs are in the log.

```json
[
  {
    "domain": "http://orb.vct:8077/maple2022",
    "proofValue": "JnP2nE4jLiHsmR65q1a6Tp-HUKZanjpCstm7qAljIE3Z6Y2mEcNarOESiu1gFlsonz1soQJe69piE7qZnTqrAw",
    "found": true,
    "leafIndex": 1,
    "auditPath": [
      "rgWgpjyv6227zFbcpyRv3YR1vbLbXaOuCeYoF7uDaY4="
    ]
  }
]
```

You'll now notice that _leafIndex_ is 1 and _auditPath_ has one entry.

Shut down the environment:

```commandline
docker-compose -f docker-compose-cli.yml -f ../docker/docker-compose-dev-vct.yml down
```
