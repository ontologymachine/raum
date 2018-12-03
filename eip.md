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

## Specification
Inspired by:
https://github.com/MetaMask/eth-contract-metadata
https://github.com/eth-registry/EIPs/blob/EIP-1357-Address-Metadata/EIPS/eip-1456.md

===========================
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

// SmartWallet (SmartAgent?)
- method for querying what functionality the wallet has...

```solidity
public queryInterface(uint256 hBytecode) returns(address) {
  // lookup hBytecode in functionality mapping...
  // if it exists, return address of implementing contract
}
```

// some DAO, new agent wants to join...
// upon agent asking to join, the DAO will make sure the agent's wallet satisfies its requirements, for example:
- attestation service for rep
- share personal data like name and email etc

// The DAO would be responsible for managing which fields are required to be published to the json blob on IPFS. Differnet implementations could 

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
