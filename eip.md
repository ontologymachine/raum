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
like DAOs which need to treat contract logic like law. And thus, a human-readable metadata structure will help both intelligent agents and humans make sense of the actions and events that the DAO
engages in. Below you will find a specification that is derived from an example ontology for the [DAOstack contracts](https://github.com/daostack/infra). Entities can be described either through JSON-LD itself or using RDF.

We intend to show through this example a potential path to an interoperability between DAOs by defining a semantic interface that can be one day be formally verifiable using the K Framework.
This will take significant time for additional research and development. In the meantime, we can provide a compact spec that provides basic capabilities for any organization to create their own schemas they can share
with other smart contracts utilizing IPFS Swarm.

The DAOs contracts would only be responsible for setting the respective URL field to retrieve the data for the scheme associated with that address, denoting its context and type. If we take an agent-centric approach to
creating these DAO ontologies, contract implementers will be responsible for describing themselves and registries can be leveraged to expose and validate implementations against the schema. This would be done off-chain until we
are able to generate a translation between the formal semantics of the computation through the EVM + Solidity, into one that's generalizable for the semantic web. Although, this approach raises the governance issue of
contract creators maliciously uploading schemas that are buggy/exploitative since the verification and publishing would happen offchain. Until this can be resolved using formal verification techniques, we will have to rely on
community diligence to blacklist contracts that are deemed to be unsafe. An immediate workaround could be to use ENS and set a whitelist of trusted domains that guarantee up-to-date schemas on registered contracts.

We can still provide these tools to developers however to discover meaningful metrics and features across different DAO implementations and within their own. Everything that a DAO can do will have at the very least
semantic value that is beneficial for both humans and machines. It will also lay the groundwork for discoverability and interoperability between these sets of contracts/agents. DAOs will be able to build meaningful
narratives for not only the communities they serve, but for other apps it interacts with in the wider ecosystem. Projects like the graphprotocol could be used by extending their GraphQL engine to also support SPARQL. This would
alleviate a lot of the hurdles trying to retrieve data off the blockchain since they already provide a robust caching solution.

## Specification
Inspired by:
https://solidity.readthedocs.io/en/v0.4.25/metadata.html?highlight=swarm
https://github.com/MetaMask/eth-contract-metadata
https://github.com/eth-registry/EIPs/blob/EIP-1357-Address-Metadata/EIPS/eip-1456.md
https://github.com/ethereum/EIPs/issues/820

If we look at DAOstack and we wanted to track an Avatars voting record where we denote each vote by the binary bit 1 (yes) and 0 (no), and give a proposalID that is the block hash of when the proposal voting period completed
you could imagine something like the following:

```json
{
  // the protocol is more or less arbitrary, but a
  // Swarm URL is recommended to the JSON schema
  "@context": "bzzr://56ab...",
  "@type": "DAOStackAvatar",
  "url": "bzzr://22ef...",
}
```

Now when someone retrieves the JSON schema stored at the URL and checks it’s context you’ll retrieve a blob that could look something like this:

```json
{
  "name": "Jane Doe",
  "address": " 0xB563300A3BAc79FC09B93b6F84CE0d4465A2AC27"
  "nativeToken": "GEN"
  "nativeReputation": "REP"
  “proposals”: [{ proposalID: “3aa2..”, vote: 1 }]
}
```

Or even in RDF format:
```rdf
```

Another example is with the DemEarth platform for identity attestation:
TODO
Eventually we’d like to validate this schemas on chain, but that would require a translation from the formal semantics of the computation through the EVM + Solidity, into one that is generalizable for all DAOs. 

We use an example the DAOstack Avatar to illustrate these semantics [definitions](https://github.com/daostack/subgraph/blob/master/src/mappings/Avatar/schema.graphql).

```json
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

Swarm hash of the file that we can use to verify a correct implementation of a registered contract.
0xa1 0x65 'b' 'z' 'z' 'r' '0' 0x58 0x20 <32 bytes swarm hash> 0x00 0x29

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


```

To further explore the potentials of the semantic web and smart contracts:

// some DAO, new agent wants to join...
// upon agent asking to join, the DAO will make sure the agent's wallet satisfies its requirements, for example:
- attestation service for rep
- share personal data like name and email etc

## Real-world Example
TODO


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
