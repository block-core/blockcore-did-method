<p align="center">
  <p align="center">
    <img src="https://user-images.githubusercontent.com/5221349/72841405-93c2ce80-3c96-11ea-844b-3e1ff782b1ae.png" height="100" alt="Blockcore" />
  </p>
  <h3 align="center">
    Blockcore Identity
  </h3>
  <p align="center">
    Blockcore DID Method specification
  </p>
</p>

# Blockcore DID Method specification

Version: 0.0.1

[DID Specification Registries](https://w3c.github.io/did-spec-registries/#did-methods)

## Summary

Decentralized identifiers (DIDs) are a new type of identifiers that enables verifiable, self-sovereign digital identity. Blockcore supports decentralized identities through the Blockcore Storage feature.

This specification describes how the Blockcore Identity framework aligns with the DID specification and how the Blockcore Universal Resolver works.

This specification conforms to the requirements specified in the [DIDs specification](https://www.w3.org/TR/did-core/) currently published by the W3C Credentials Community Group.

The Blockcore Identity registry is a permissionless and borderless runtime for identities.

## Blockcore DID Method Name

The namestring that shall identify this DID method is: `is`.

A DID that uses this method **MUST** begin with the following prefix: `did:is`. Per this DID specification, this string **MUST** be in lowercase.

The remainder of the DID, after the prefix, can either be additional sub-prefix or the profile identifier.

## Profile Identifier / KeyID

The profile identifier is derived as an P2PKH address (e.g. like Bitcoin address) with the [prefix](https://en.bitcoin.it/wiki/List_of_address_prefixes) of pubkey address of 55 (P, uppercase P) and script address of 117 (p, lowercase p).

This means that the latter part of the identifier always starts with capital `P`.

## Example

A valid Blockcore DID might be:

```
did:is:PLBNc1Ph6whu1vbQGEuRywTTHCEnfDDuXh
```

## Hierarchical Deterministic Key Generation

The keys used for Blockcore Identity is based upon the same scheme such as regular wallets. You can generate unique private keys for each of your identities and manually manage them, or you can rely on a HD scheme ([BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)).

The [BIP44 prefix](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) for Blockcore Identity derived keys is 302. This is up for revision before finalization of the implementation.

Keys used for Blockcore Identity only supports secp256k1 algorithm, identifies as "ES256K" in the JOSE libraries.

## Operations

The following methods is available for publishing, updating and deleting an Blockcore Identity.

Blockcore Identities resides on all public and private Blockcore-based blockchain nodes that has the Blockcore Storage feature enabled. The Blockcore Identities are cross-chain compatible across all Blockcore-derived blockchains.

### Create (Register)

You can create identities in any programming language, and it relies on open standards to create and sign the identity profile documents.

Foundations for the runtime is Javascript Object Signing and Encryption (JOSE), which allows cross-platform signed documents (JSON Web Signature).

Blockcore provides these libraries to help simplify the creation of identities:

* [Blockcore Javascript Object Signing and Encryption (JOSE) for .NET](https://github.com/block-core/blockcore-jose)

* [Blockcore Message](https://github.com/block-core/blockcore-message), Library that helps signing messages in the browser.

Step by step guide:

1. Create your identity JSON structure according to the latest schema.
1. Encode the JSON structure using JSON Web Signature. This is a JWT with a signature attached.
1. Publish the JWT using the latest API method, either on local Blockcore node or public public (also known as Blockcore Hub).

Example of signed [JWS](https://tools.ietf.org/html/rfc7515):
```
eyJhbGciOiJFUzI1NksiLCJ0eXAiOiJKV1QiLCJraWQiOiJQTEJOYzFQaDZ3aHUxdmJRR0V1Unl3VFRIQ0VuZkREdVhoIn0.eyJpZGVudGlmaWVyIjoiZGlkOmlzOlBMQk5jMVBoNndodTF2YlFHRXVSeXdUVEhDRW5mRER1WGgiLCJpYXQiOjE1OTg4MDMxODA0MzgsIkB0eXBlIjoiaWRlbnRpdHkiLCJAc3RhdGUiOjAsIm5hbWUiOm51bGwsInNob3J0bmFtZSI6bnVsbCwiYWxpYXMiOm51bGwsInRpdGxlIjpudWxsLCJlbWFpbCI6bnVsbCwidXJsIjpudWxsLCJpbWFnZSI6bnVsbCwiaHVicyI6bnVsbH0.HwQuFSNgMLJuHUaBY0edliaY0FJHD9O4hpSYabAd3y2KNBuR1ChAO9IpvikJNDHIyYgmWjqj8Kt2IgK3hwM4VBM
```

The token can be decoded using web sites such as https://jwt.ms/.

The above token decodes to the following:

```
{
  "alg": "ES256K",
  "typ": "JWT",
  "kid": "PLBNc1Ph6whu1vbQGEuRywTTHCEnfDDuXh"
}.{
  "identifier": "did:is:PLBNc1Ph6whu1vbQGEuRywTTHCEnfDDuXh",
  "iat": 1598803180438,
  "@type": "identity",
  "@state": 0,
  "name": null,
  "shortname": null,
  "alias": null,
  "title": null,
  "email": null,
  "url": null,
  "image": null,
  "hubs": null
}.[Signature]
```

The only required parts of the identity profile is the `identifier`. Users (and machines) can have a public profile of only the identifier and no other metadata attached.

### Read (Resolver)

Blockcore DID's associated DID document (also known as Identity Profile) can be looked up using the REST API of a node or a hub.

Blockcore hosts a public hub that can be used to publish and read identities.

https://www.did.is/

Example of result from REST API:

```
{
  "id": "did:is:PLBNc1Ph6whu1vbQGEuRywTTHCEnfDDuXh",
  "version": 4,
  "header": "eyJhbGciOiJFUzI1NksiLCJ0eXAiOiJKV1QiLCJraWQiOiJQTEJOYzFQaDZ3aHUxdmJRR0V1Unl3VFRIQ0VuZkREdVhoIn0",
  "payload": "eyJpZGVudGlmaWVyIjoiZGlkOmlzOlBMQk5jMVBoNndodTF2YlFHRXVSeXdUVEhDRW5mRER1WGgiLCJpYXQiOjE1OTg4MDMxODA0MzgsIkB0eXBlIjoiaWRlbnRpdHkiLCJAc3RhdGUiOjAsIm5hbWUiOm51bGwsInNob3J0bmFtZSI6bnVsbCwiYWxpYXMiOm51bGwsInRpdGxlIjpudWxsLCJlbWFpbCI6bnVsbCwidXJsIjpudWxsLCJpbWFnZSI6bnVsbCwiaHVicyI6bnVsbH0",
  "signature": "HwQuFSNgMLJuHUaBY0edliaY0FJHD9O4hpSYabAd3y2KNBuR1ChAO9IpvikJNDHIyYgmWjqj8Kt2IgK3hwM4VBM",
  "content": 
  {
    "identifier": "did:is:PLBNc1Ph6whu1vbQGEuRywTTHCEnfDDuXh",
    "type": "identity",
    "state": 0,
    "timestamp": "1598803180438"
  }
}
```

To validate this result, the header, payload and signature must be combined with an `.` seperator. Use one of the .NET or JavaScript libraries provided by Blockcore, or another JOSE-library.

### Update (Replace)

The identities on Blockcore Identity registry is always upsert, meaning that there are no difference between new created identities and updates.

There is one requirement for an update to succeed, and that is for the "iat" timestamp field to be updated with a higher value.

### Delete (Replace/Revoke)

Deleting an identity is similar to create/update, but all fields should be set to null and the `@state` must be set to 999.

Known states:

- 0: Active Identity Profile
- 999: Deleted Identity Profile

A deleted identity can be recreated at a later time. It is left empty in the registry to avoid older nodes coming online, re-adding the identity to the decentralized node storage.

## Security and Privacy Considerations

There are no limits to the amount of identity profiles that a user can create. This means they can improve their privacy by acting under different identities for different purposes.

Your identities can easily be correlated by a node, so don't expect identities in the same wallet to be completely sepearate.

The private key used to sign the identities, has the same security as Bitcoin and Blockcore-based blockchain wallet addresses.

Identities that are published to nodes, can potentially be stored forever. Delete of an identity is only a request to nodes to remove the information, a node can potentially archieve and index all published identities.
