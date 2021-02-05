---
id: basics-orientation
title: Orientation
sidebar_label: Orientation
---

You are currently in the Basics section of the docs, which is meant to provide a reference for important tools and concepts.

If you are ready to dive straight into application and smart contract development, we recommend skimming this page and then heading directly to [the Develop section](/docs/roles/developer/quickstart).

If you are **not** an app developer, we recommend the following paths instead:

1. **For the non-technical person or NEAR noob,** see the [Beginner's Guide to NEAR](https://near.org/blog/the-beginners-guide-to-the-near-blockchain/).
2. **To run a validating node,** which means taking part in the operation of the core network itself, see [running a testnet node](/docs/local-setup/running-testnet) or, for a full staking overview, see [the staking overview](/docs/validator/staking-overview). This is not necessary for developing smart contracts or apps but it can be a good way to feel connected to the "bare metal" of how the network works.
3. **To integrate with NEAR,** for example if you are running an exchange, a wallet, or other project that needs to integrate on a deeper level, please see [integrating with NEAR](/docs/roles/integrator/exchange-integration).
4. **To contribute to the core code base yourself,** see [contributing to NEAR](/docs/contribution/contribution-overview). To suggest changes to these docs, [view them on Github](https://github.com/near/docs) or just click the "Edit" button in the top to get started submitting your pull request.

These docs will typically assume that you are familiar with common code practices and concepts unless otherwise noted, though the general goal is to be beginner-friendly wherever possible.



<!-- UNCOMMENT WHEN PAGE COMPLETE -->
<!-- #### For reference and code samples, have a look at -->
<!-- * [Common Code Patterns](code-patterns/token-issuance) -->

## Overview of Sections

### "Getting Started"

This section provides brief explanations of the key tools you'll use during development and a quick tutorial for setting up your account on TestNet.


### "Key Concepts"

This section provides plain explanations of important concepts which apply to NEAR and to blockchains in general. It is a good reference.

If anything is unclear, please use the "Edit" button to file a Github issue or PR that fixes it!


### [Removed] "Tutorials"

**important** The former "Tutorials" section is currently deprecated and has been moved [here](/docs/roles/developer/tutorials/introduction). Please have a look at our [example applications](https://examples.near.org/) that showcase different smart contract functionality instead.


## Additional reference: API Documentation

Here are the reference docs for all of our supported libraries. We actively support an AssemblyScript runtime (TypeScript -&gt; WASM compiler), a Rust runtime, the base RPC and a JavaScript library. These docs are auto-generated and less user-friendly! They should be used for advanced users and those who need to look up how something works.

* [AssemblyScript Contracts](/docs/roles/developer/contracts/assemblyscript) -- covers the same topic, but for AssemblyScript
* [RPC](/docs/api/rpc) -- covers the actual core API responses and actions you can call
* [near-api-js](/docs/api/near-api-js) -- is autogenerated documentation for the JavaScript integration library
* [Rust Contracts](/docs/api/near-sdk-rs) -- covers the API for creating apps with Rust

## Support & Community

If you have any questions about NEAR, you can get direct access to the team behind it and other members of the community through [Discord](http://near.chat).

To find more about on how YOU can get involved, please checkout our [community section](/docs/contribution/nearcore)!

>Got a question?
<a href="https://stackoverflow.com/questions/tagged/nearprotocol">
  <h8>Ask it on StackOverflow!</h8></a>