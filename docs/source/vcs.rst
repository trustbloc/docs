###################################
Verifiable Credential Service (VCS)
###################################


What is a Verifiable Credential (VC)?
*************************************

We use credentials everyday. A driver's license issued by the government certify
that we are capable of operating a vehicle on the road. A Permanent Residence card
shows the immigration status of an individual.

A verifiable credential is then a document whose contents can be cryptographically proven/verified (VC-TERM_) to be true. 
A VC could hold the same data that a physical credential does.
Within the scope of TrustBloc projects, this act of verifying credentials can be done with the aid
of technology such as digital identities and signatures. The use of digital signatures adds to the integrity
of a credential when it is presented.

Holders of verifiable credentials can generate verifiable presentations and then share these 
verifiable presentations with verifiers to prove they possess verifiable credentials with certain characteristics.
Both verifiable credentials and verifiable presentations can be transmitted rapidly, making them more convenient 
than their physical counterparts when trying to establish trust at a distance. (VC-DEF_)


Edge-Service
************

TrustBloc's `Edge-Service <https://github.com/trustbloc/edge-service>`__ contains servers that handle the issuance and verification of verifiable credentials.

Configuring the service
=======================
TODO

Deploying the service
======================
TODO

Issuing a VC
============

In order to issue a Verifiable Credential, you will need to first create a profile.

1. Issue a VC
-------------

**HTTP POST /{profile}/credentials/issueCredential**

.. code:: json

   {
      "credential":{
         "@context":[
            "https://www.w3.org/2018/credentials/v1"
         ],
         "id":"http://example.edu/credentials/1872",
         "type":"VerifiableCredential",
         "credentialSubject":{
            "id":"did:example:ebfeb1f712ebc6f1c276e12ec21"
         },
         "issuer":{
            "id":"did:example:76e12ec712ebc6f1c221ebfeb1f",
            "name":"Example University"
         },
         "issuanceDate":"2010-01-01T19:23:24Z",
         "credentialStatus":{
            "id":"https://example.gov/status/24",
            "type":"CredentialStatusList2017"
         }
      },
      "options":{
         "assertionMethod":"did:trustbloc:testnet.trustbloc.local:EiAiijiRNEAflOr6ZOJN5A7BCFQD1pwFMI1MPzHr3bXezg=="
      }
   }

More details `here <https://github.com/trustbloc/edge-service/blob/master/docs/vc-rest/api_overview.md#3-issue-verifiable-credential---post-profilecredentialsissuecredential>`__.

Try it `here <https://w3c-ccg.github.io/vc-http-api/#/Issuer/issueCredential>`__.

2. Compose and Issue a VC
-------------------------

**HTTP POST /{profile}/credentials/composeAndIssueCredential**

.. code:: json

   {
      "issuer":"did:example:uoweu180928901",
      "subject":"did:example:oleh394sqwnlk223823ln",
      "types":[
         "UniversityDegree"
      ],
      "issuanceDate":"2020-03-25T19:38:54.45546Z",
      "expirationDate":"2020-06-25T19:38:54.45546Z",
      "claims":{
         "name":"John Doe"
      },
      "evidence":{
         "id":"http://example.com/policies/credential/4",
         "type":"IssuerPolicy"
      },
      "termsOfUse":{
         "id":"http://example.com/policies/credential/4",
         "type":"IssuerPolicy"
      },
      "proofFormat":"jws",
      "proofFormatOptions":{
         "kid":"did:trustbloc:testnet.trustbloc.local:EiAtPEWAphdPVRxlKpr8N43uyLMhgF-9SFmYfINVpDIzUA==#key-1"
      }
   }

More details `here <https://github.com/trustbloc/edge-service/blob/master/docs/vc-rest/api_overview.md#4-compose-and-issue-verifiable-credential---post-profilecredentialscomposeandissuecredential>`__.

Validating a VC
===============

**HTTP POST /verifier/credentials**

.. code:: json

   {
      "verifiableCredential":{
         "@context":[
            "https://www.w3.org/2018/credentials/v1",
            "https://www.w3.org/2018/credentials/examples/v1"
         ],
         "credentialSchema":[

         ],
         "credentialStatus":{
            "id":"http://issuer.vc.rest.example.com:8070/status/1",
            "type":"CredentialStatusList2017"
         },
         "credentialSubject":{
            "degree":{
               "degree":"MIT",
               "type":"BachelorDegree"
            },
            "id":"did:example:ebfeb1f712ebc6f1c276e12ec21",
            "name":"Jayden Doe",
            "spouse":"did:example:c276e12ec21ebfeb1f712ebc6f1"
         },
         "id":"http://example.gov/credentials/3732",
         "issuanceDate":"2020-03-16T22:37:26.544Z",
         "issuer":{
            "id":"did:example:oakek12as93mas91220dapop092",
            "name":"University"
         },
         "proof":{
            "created":"2020-04-09T15:35:35Z",
            "jws":"eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..kN1srfFqoiejHJwxM8Y0Y9yIonAvFeF2Aoiv6_LTkPqcNXc2rXwT94-uO_PQJbxWJgTD78MvpfCJWsUSRvgCBw",
            "proofPurpose":"assertionMethod",
            "type":"Ed25519Signature2018",
            "verificationMethod":"did:trustbloc:testnet.trustbloc.local:EiD3KVRkHAHt6aLO4Kp5PSO3pNhAY_GPZXuKUekVk1uboQ==#key-1"
         },
         "type":[
            "VerifiableCredential",
            "UniversityDegreeCredential"
         ]
      },
      "options":{
         "checks":[
            "proof"
         ]
      }
   }

More details `here <https://github.com/trustbloc/edge-service/blob/master/docs/vc-rest/api_overview.md#1-verify-credential---post-verifiercredentials>`__.

Try it `here <https://w3c-ccg.github.io/vc-http-api/#/Verifier/verifyCredential>`__.

Requesting a VC
===============
TODO

Connecting to the TestNet
*************************
TODO

Using Edge-Service
******************

To use the demo, navigate to the `Demo Issuer <https://demo-issuer.sandbox.trustbloc.dev>`__ homepage.

Then follow the steps in the videos below for their respective demonstrations.

Issue a Credit Score Report
===========================

.. raw:: html

         <iframe
                width="560" 
                height="315"
                src="https://www.youtube.com/embed/X2i1mwyryYc"
                frameborder="0"
                allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                allowfullscreen>
        </iframe>
    

Issue a Driver's License
========================

.. raw:: html

         <iframe
                width="560" 
                height="315"
                src="https://www.youtube.com/embed/Riv48wZuAcM"
                frameborder="0"
                allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                allowfullscreen>
        </iframe>


References
**********

.. [VC-DEF] Manu Sporny; Grant Noble; Dave Longley; David Chadwick, `"Verifiable Credentials Data Model 1.0" <https://www.w3.org/TR/vc-data-model/#what-is-a-verifiable-credential>`_,
          November 2019

.. [VC-TERM] Manu Sporny; Grant Noble; Dave Longley; David Chadwick, `"Verifiable Credentials Data Model 1.0" <https://www.w3.org/TR/vc-data-model/#terminology>`_,
          November 2019
