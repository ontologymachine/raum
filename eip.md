---
eip: TBD
title: TBD
authors: Jordan Ellis <jelli@dorgtech.com>, Cory Dickson <cory.dickson94@gmail.com>
category: ERC
status: Draft
created: 2018-12-02
discussions-to: TBD
---
## EIP Title goes here

## Summary
A standard interface for publishing and validating JSON specifications associated with ethereum addresses

## Abstract
Expanding on metadata fields in EIP-1357:
> Ethereum addresses have related information (metadata) useful to users and developers. This metadata is captured by third parties and not accessible
> through an on-chain mechanism.
>
> ETH Registry makes address metadata available to applications and users through a registry contract that maps the addres to a JSON file stored on IPFS.
> A working prototype is available at: https://ethregistry.org.

This simple access control interface is for other contracts to make modifications to JSON files associated with an ethereum address.
Said wallet can give consent to a contract to write or read a set of fields.

## Motivation
Building a psuedo-anonymous reputation system where users can own the data they provide to the services they use is a critical part of a fair decentralized future.
The ability to share metadata transparently also makes the feature set of dapps richer across the whole ecosystem. This "smart" wallet inteface facilitates these goals by giving developers tools
to publish data related to a particular address for record keeping purposes.

Extending these ideas into the semantic web allows us to run complex queries across our blockchain data and create more interactive transparent experiences. This is critical in organizations
like DAOs which need to treat contract logic like law. And thus, a human-readable metadata structure will help both intelligent agents and humans make sense of the possible narratives playing out
in its ecosystem. Below you will find a specification that is derived from an example ontology for the [DAOstack contracts](https://github.com/daostack/infra).

We intend to show through this example a potential path to an interoperability between DAOs by defining a semantic interface that can be formally verifiable using the K Framework. This will take
significant time to complete. In the meantime, we can provide a compact spec that provides basic capabilities for any organization to create their own ontologies they can share with other
smart contracts and DAOs.

## Specification
Inspired by:
https://solidity.readthedocs.io/en/v0.4.25/metadata.html?highlight=swarm
https://github.com/MetaMask/eth-contract-metadata
https://github.com/eth-registry/EIPs/blob/EIP-1357-Address-Metadata/EIPS/eip-1456.md
https://github.com/ethereum/EIPs/issues/820

We use an example the DAOstack Avatar to illustrate these semantics [definitions](https://github.com/daostack/subgraph/blob/master/src/mappings/Avatar/schema.graphql).

```JSON-LD
{
  // the protocol is more or less arbitrary, but a
  // Swarm URL is recommended to the JSON schema
  "@context": "bzzr://56ab...",
  "@type": "DAOStackAvatar",
  "name": "Jane Doe",
  "address": " 0xB563300A3BAc79FC09B93b6F84CE0d4465A2AC27"
	"nativeToken": "GEN"
	"nativeReputation": "REP"
}
```
This JSON file would be constructed through the formal semantics of the set of contracts that implement this @entity which schema is stored on IPFS Swarm. As such, the contract is responsible for
its correctness and implementation details.

Here we will need an `ERC820Registry` that contains the mappings of address that can implement the interfaces for each of the entities described with the JSON metadata. This is a crucial step
in ensuring that the schemas are valid and not comprimised by a bad actor. Also, it can be used as a trust system in the future for other Dapps that want to see the semantic data of the memebers
inside of an organization.

```solidity
// ERC 20
interface ERC20 {
  public someFunction(bool value) returns(uin256);
}

// Compiles to
bytecode: 0x475ACD83....
hBytecode: 0x5 (ERC20 GUID)

contract Implementation implements ERC20 {
  public someFunction(bool value) returns(uint256) {
    // stuff
  }
}

Swarm hash of the file that we can use to verify a correct implementation of a registered contract.
0xa1 0x65 'b' 'z' 'z' 'r' '0' 0x58 0x20 <32 bytes swarm hash> 0x00 0x29

```

To further explore the potentials of the semantic web and smart contracts:

// some DAO, new agent wants to join...
// upon agent asking to join, the DAO will make sure the agent's wallet satisfies its requirements, for example:
- attestation service for rep
- share personal data like name and email etc

// The DAO would be responsible for managing which fields are required to be published to the json blob on IPFS. Differnet implementations could 

Ideally, there will be three methods for this interface contract interacting with the data. This would be the responsibility of the 
implementers to do the following:
- verification (ensure that the schema inside the json is valid for the contract interface)
- write a new field / add a new entity to the context
- Update an existing field with new data

## Real-world Example
TODO


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
