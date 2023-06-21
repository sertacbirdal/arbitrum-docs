---
title: "A gentle introduction: Orbit chains"
sidebar_label: "A gentle introduction: Orbit chains"
description: "Launch your own Arbitrum Orbit chain under the Arbitrum Nitro codebase's new licensing model. Settle to Arbitrum's L2 chains via bridge contracts on the underlying L2 chain (Goerli for now, One or Nova soon). No need for permission from the Arbitrum DAO or Offchain Labs to create your Orbit chain or modify its instance of the Nitro codebase."
author: oliviaJ3388
target_audience: technical decision-makers, people familiar with web3 who will decide to use Orbit on behalf of their organizations
sidebar_position: 1
---

This document is for developers and decision-makers who want to learn more about **Arbitrum Orbit chains**, a new product offering that lets you create your own self-managed <a data-quicklook-from="arbitrum-rollup-protocol">Arbitrum Rollup</a> and <a data-quicklook-from="arbitrum-anytrust-protocol">AnyTrust</a> chains. 

If you'd prefer to learn by doing, see the [Orbit quickstart](./orbit-quickstart) for step-by-step instructions that walk you through the process of configuring and launching your own Orbit chain.

import PublicPreviewBannerPartial from './partials/_orbit-public-preview-banner-partial.md'; 

<PublicPreviewBannerPartial />


### In a nutshell:

- Arbitrum Orbit lets you **create your own dedicated chain** that settles to one of Arbitrum's <a data-quicklook-from='layer-2-l2'>Layer 2 (L2)</a> chains: <a data-quicklook-from='arbitrum-one'>Arbitrum One</a>, <a data-quicklook-from="arbitrum-nova">Arbitrum Nova</a>, or **Arbitrum Goerli**.
- **You own your Orbit chain** and can customize its privacy, permissions, fee token, governance, and more.
- Examples of possibilities that this unlocks:
    - **Launch a decentralized Nitro-powered blockchain network**, and that benefits from <a data-quicklook-from='arbitrum-nitro'>Nitro</a>'s fraud proofs, advanced compression, [EVM+ compatibility via Stylus](https://offchain.medium.com/hello-stylus-6b18fecc3a22), and continuous improvements.
    - **Offer gas price reliability** to your end-users thanks to the dedicated throughput and traffic isolation that Orbit chains offer by default.
    - **Permission access** to control who can read your chain's data and who can deploy smart contracts to your chain. Your chain can be completely permissionless like Ethereum and <a data-quicklook-from="arbitrum-one">Arbitrum One</a>, or you can implement your own permissions policies.
    - **Collect fees using a token that you choose** to rapidly iterate on domain-specific mechanism designs and value capture opportunities.


### What's Orbit?
 
import { MermaidWithHtml, Nodes, Node, Connection, NodeDescriptions, NodeDescription } from '/src/components/MermaidWithHtml/MermaidWithHtml';

<MermaidWithHtml>
  <Nodes>
    <Node id="1">Orbit chain</Node>
    <Node id="2">Orbit chain</Node>
    <Node id="3">Orbit chain</Node>
    <Node id="4">Arbitrum (L2 - Goerli, One, Nova)</Node>
    <Node id="5">Ethereum (L1 - Goerli, Mainnet)</Node>
    <Connection from="1" to="4"/>
    <Connection from="2" to="4"/>
    <Connection from="3" to="4"/>
    <Connection from="4" to="5"/>
  </Nodes>
  <NodeDescriptions>
    <NodeDescription for='1,2,3'><strong>Arbitrum Orbit</strong> is a new product offering that lets you create your own chain that settles to one of Arbitrum's public Layer 2 (L2) chains.</NodeDescription>
    <NodeDescription for='4'><strong>Arbitrum One</strong>, <strong>Arbitrum Nova</strong>, and <strong>Arbitrum Goerli</strong> are public Layer 2 (L2) chains that settle to Ethereum.</NodeDescription>
    <NodeDescription for='5'><strong>Ethereum</strong> is a public Layer 1 (L1) chain.</NodeDescription>
  </NodeDescriptions>
</MermaidWithHtml>


- You can think of Orbit chains as **deployable, configurable forks of Arbitrum's L2 Nitro technology stack** that are tightly coupled to Arbitrum's L2 chains.
- You can also think of them as **tailored chains** - chains tailored precisely to **your exact use-case and business needs**.
- This gives you another way to **progressively decentralize** your applications and **incrementally adopt** the properties and security assumptions of Ethereum's base layer.
- It also lets you continuously integrate improvements made to the <a data-quicklook-from="arbitrum-nitro">Arbitrum Nitro</a> stack (the code that powers the nodes that support Arbitrum's L2 and Orbit chains).
- Note that <a data-quicklook-from="arbitrum-one">Arbitrum One</a> and <a data-quicklook-from="arbitrum-nova">Arbitrum Nova</a> implement the <a data-quicklook-from='arbitrum-rollup-protocol'>Arbitrum Rollup</a> and <a data-quicklook-from='arbitrum-anytrust-protocol'>AnyTrust</a> protocols, respectively. Every Orbit chain can be configured to use either Rollup or AnyTrust — it's your choice![^1]
- Arbitrum One and Arbitrum Nova are owned and governed by the [Arbitrum DAO](https://docs.arbitrum.foundation/concepts/arbitrum-dao). With Orbit chains, *you* determine the way that your chain is governed. 
  - The [Arbitrum DAO](https://docs.arbitrum.foundation/concepts/arbitrum-dao) owns and governs the L2 chains that your Orbit chain can settle to. Refer to the [Arbitrum DAO docs](https://docs.arbitrum.foundation/new-arb-chains) to learn more about the Arbitrum DAO's purpose, protocols, and role with regard to Orbit chains.


### What problem does Orbit solve?

The Ethereum ecosystem is supported by a **decentralized network of nodes** that each run Ethereum's Layer 1 (L1) client software. Ethereum's block space is in high demand, so users are often stuck waiting for the network to become less congested (and thus, less expensive).

<!-- v.previous

  - attempting to enhance clarity/precision while building more explicitly upon the section's introduction.
  - would love to spend more time iterating on this section, but we may be at diminishing returns.
  - happy to revert
  
~~

Layer 2 (L2) scaling solutions like Arbitrum Rollup and AnyTrust solve this problem by optimizing their use of Ethereum. They put just enough data on Ethereum to inherit its security but move a lot of the heavy lifting off-chain.

While both Rollup and AnyTrust protocols offload lots of transaction data from Ethereum, there are important differences. Rollup chains put all of the raw transaction data on Ethereum, while AnyTrust chains settle to Ethereum but use a <a data-quicklook-from='data-availability-committee-dac'>data availability committee (DAC)</a> to store raw transaction data. The choice between Rollup (Arbitrum One implements this one) and AnyTrust (Arbitrum Nova implements this one) represents a tradeoff between decentralization and performance.

While Arbitrum One and Nova can help most applications scale on top of Ethereum — and are home to thousands of apps and millions of users — shared public chains aren't for everyone. Some projects can benefit from their own AnyTrust or Rollup chains that afford the same security, but with a higher degree of control over the chain's features and governance.

You can think of an Orbit appchain as a **dedicated priority lane on Ethereum that you manage**. Each of these priority lanes have many times the capacity of Ethereum, but piggyback on Ethereum for security.

-->

Arbitrum's <a data-quicklook-from="arbitrum-rollup-protocol">Rollup</a> and <a data-quicklook-from="arbitrum-anytrust-protocol">AnyTrust</a> protocols address this challenge by offloading some of the Ethereum network's heavy lifting to **another decentralized network of nodes** that support the <a data-quicklook-from="arbitrum-one">Arbitrum One</a> and <a data-quicklook-from="arbitrum-nova">Arbitrum Nova</a> L2 chains, respectively.

There are important differences between these chains. The choice between Rollup and AnyTrust represents a **tradeoff** between decentralization and performance:

 - **Arbitrum One** implements the Rollup protocol, which stores raw transaction data on Ethereum L1, while 
 - **Arbitrum Nova** implements the AnyTrust protocol, which uses a <a data-quicklook-from='data-availability-committee-dac'>data availability committee (DAC)</a> to store raw transaction data, expediting settlement and reducing costs by introducing a security assumption.

**These two public chains will meet most projects' needs** - they already support thousands of apps and millions of users! But shared public chains aren't for everyone. Some projects can benefit from their own AnyTrust or Rollup chains that afford the same security, but with a higher degree of control over the chain's features and governance (remember, these public L2 chains and their protocols are governed by the [Arbitrum DAO](https://docs.arbitrum.foundation/gentle-intro-dao-governance)).

Orbit chains give you the ability to create your own AnyTrust and Rollup chains using your own infrastructure. You can think of your Orbit chain as a **self-managed priority lane on Ethereum**. Each Orbit chain is capable of supporting many times the capacity of Ethereum, all while benefitting directly from Ethereum's security.

Said simply:

 - **Arbitrum One** and **Arbitrum Nova** chains unlocked **two public, DAO-managed contract deployment options** that scale Ethereum and meet most projects' needs.
 - **Arbitrum Orbit** chains unlock an **[infinite garden](https://ethereum.foundation/infinitegarden) of self-managed contract deployment options** that scale Ethereum even further, with each individual Orbit chain being tailored precisely to its owner's needs.


### How does Orbit help me build decentralized apps?


| Benefit                             | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Dedicated throughput**            | You may need dedicated throughput if your dApp demands high-performance or consistent resource availability. Running your dApp on its own Orbit chain significantly increases resource availability, so you don’t need to compete for computation and storage resources.                                                                                                                                                                                                                                                                     |
| **EVM+ compatibility**              | Orbit chains will benefit the same EVM+ compatibility that [Stylus](https://offchain.medium.com/hello-stylus-6b18fecc3a22) introduces. This means that you'll be able to deploy EVM-compatible smart contracts using Solidity, C, C++, and Rust - no need to migrate away from the language and toolchain that you're already using!                                                                                                                                                                                                         |
| **Independent product roadmap**     | If you want to decouple your app chain's roadmap from that of Ethereum and/or Arbitrum, Orbit makes this possible. This lets you implement cutting-edge features like account abstraction ahead of projects following Ethereum's public roadmap.                                                                                                                                                                                                                                                                                             |
| **Increased gas price reliability** | Many types of dApps rely on predictable transaction costs. Because Orbit chains are isolated from Arbitrum L2 and Ethereum L1 traffic, using Orbit chains means that you won't be significantly affected by other apps' on-chain activity, allowing your dApp's users to enjoy more reliable gas prices.                                                                                                                                                                                                                                     |
| **Account abstraction**             | Predictable gas prices make it easy to model and predict business costs, which makes it easier to experiment with traditionally cost-prohibitive mechanisms like **transaction fee subsidization**. This makes it easier to further abstract the technical complexity of decentralized apps away from end-user experiences, allowing you to deliver decentralized experiences that feel familiar to nontechnical audiences (who may not understand or care about implementation details).                                                    |
| **Customizable fee token**          | Orbit chains can use any token you choose as the fee token, facilitating seamless integration with your dApp's ecosystem.                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **Customizable protocol logic**     | You may need to modify the logic of your chain's settlement, execution, or governance protocols in order to meet specific requirements. Orbit's chains let you do this, while benefiting from the security of Ethereum, through Arbitrum's DAO-governed L2 chains.                                                                                                                                                                                                                                                                           |
| **Nitro extensibility**             | Orbit chains will have access to all Nitro code upgrades, feature additions, and improvements, giving your Orbit chain the option to stay up-to-date and incorporate the latest and greatest in Ethereum scaling technology.                                                                                                                                                                                                                                                                                                                 |
| **Decentralization options**        | You can build an <a data-quicklook-from='arbitrum-rollup-protocol'>Arbitrum Rollup</a> chain that uses Ethereum for data availability, or you can build an <a data-quicklook-from='arbitrum-anytrust-protocol'>Arbitrum AnyTrust</a> chain that uses a <a data-quicklook-from='data-availability-committee-dac'>Data Availability Committee (DAC)</a> to expedite the settlement of transactions to your Orbit chain's base chain, making things even cheaper for you and your end-users. Orbit chains can use either of these technologies. |
| **Low prototyping costs**           | Orbit chains can be easily created. See the [Orbit Quickstart](./orbit-quickstart) for step-by-step instructions.                                                                                                                                                                                                                                                                                                                                                                                                                            |
| **Security**                        | Arbitrum technology powers the most secure L2s, and you can use this same mature technology stack for your Orbit chain.                                                                                                                                                                                                                                                                                                                                                                                                                      |
| **Flexible technology options**     | Orbit lets you choose between Rollup, AnyTrust, or custom technology stacks. This makes Ethereum and Arbitrum technologies more adoptable by allowing you to incorporate only the elements of the technologies that you need.                                                                                                                                                                                                                                                                                                                |
| **Permissioned access**             | Orbit gives you the freedom to choose which contracts are deployed on your chain. You can keep it as open and permissionless as Ethereum, restrict contract deployment so that only your app can be deployed on this chain, or anything in between!                                                                                                                                                                                                                                                                                          |


### How does Orbit help the Ethereum ecosystem?

Orbit helps Ethereum move towards a **multi-chain future**. This is valuable for the following reasons:

| Value add                           | Description                                                                                                                                                                                                                                                                                                               |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Scalability**                     | Multiple chains help overcome scaling bottlenecks by dividing activity into opt-in environments with separate resource management.                                                                                                                                                                                        |
| **Flexible security models**        | Different chains can experiment with different security models, allowing for tradeoffs. For example: Arbitrum One and Arbitrum Nova are both L2 chains, with Arbitrum Nova giving developers the ability to optimize for lower fees. With Arbitrum Orbit, extending the technology and experimenting is easier than ever. |
| **Flexible execution environments** | Different chains can experiment with more-or-less restrictive execution environments. For example, although Arbitrum chains are fully EVM compatible, Orbit chains can restrict smart contract functionality to optimize for your project's needs.                                                                        |
| **Flexible governance**             | Orbit chains let you define your own governance protocols.                                                                                                                                                                                                                                                                |
| **Increased adoption of dApps**     | Orbit's optionality reduces the cost of progressive decentralization for devs, unlocking a new category of decentralized apps that are built under more autonomous, self-managed conditions.                                                                                                                              |


### Are Orbit chains the same thing as "app chains"?

It depends on your definition of "app chain". Orbit chains aren't *just* for decentralized apps (dApps):

 - You can use your Orbit chain to host the smart contracts that support one dApp, two dApps, an ecosystem of dApps, or no dApps at all.
 - You can use your Orbit chain to host a private, centralized service's arbitrary EVM contracts.
 - Your Orbit chain can be special-purpose, general-purpose, and everything in-between.
 - You could even build a dApp that uses multiple Orbit chains to support strange new forms of redundancy, high availability, and trustlessness.

Orbit chains can be used as application-specific chains (often referred to as "appchains"). But **they aren't just for apps**. They're for **hosting EVM-compatible smart contracts using self-managed infrastructure that isolates compute and storage resources away from Arbitrum's public L2 chains**, while **settling to one L2 chain in particular** based on your unique needs.


<!-- v.original

### How will Orbit chains interact with each other?

By launching an Orbit chain into the Arbosphere, you’re not launching an isolated blockchain; you’re joining a growing ecosystem of interconnected Arbitrum Orbit chains, all of which are running the most advanced technology available today to scale blockchain infrastructure. 

Orbit developers have full access to the Arbitrum Nitro stack today, allowing them to choose from either of Arbitrum’s market-leading scaling technologies — Arbitrum Rollup and Arbitrum Anytrust. Both of these technologies run on the Arbitrum Nitro stack meaning that they have full EVM-equivalence and application developers will have the same EVM experience as on  Ethereum or any other Arbitrum chain, and you’ll be able to directly tap into Ethereum’s and Arbitrum’s existing developer communities and resources. 

Orbit developers can customize their chain however they’d like. Moreover, with exciting new developments in the pipeline, Orbit chains have access to not only the Arbitrum Nitro stack today, but also to future improvements. Looking forward a few months, Arbitrum Stylus will allow application developers to augment the EVM experience with smart contracts in other programming languages (Rust, C, and C++ to start), and this powerful technology will be fully available to all Orbit chains. 

When it comes to interoperability tech, Orbit will lead the way in offering the most advanced and secure technology on the market. We’re actively working on fundamental research and development to build the most interconnected experience for Orbit chains with the overarching goal of creating a user experience that allow for seamless integration between Orbit chains, allowing users to quickly and securely move assets between Orbit chains and build secure and intuitive cross-chain experiences. 

As always, all features we ship will come fully-baked with security proofs enabled. That’s the Arbitrum difference. Over the coming months, the process of launching these chains, and building connections between them will become more robust, and natively integrated (for a sneak peak, you can read about fast confirmations in the table below) and we’re excited to share more details around these developments as they’re ready.

The Arbitrum Nitro stack is the most advanced, secure, and mature Ethereum scaling technology to date. It already powers Ethereum’s leading L2 chains, and with Orbit this technology is now available to power your chain as well. Welcome to the Arbosphere **🚀**

-->

<!-- v.edited - happy to revert

 - Blending the header title into the general flow

 - Reduce unfamiliar jargon (is "arbosphere" a canonical term? if so, does it belong in this document? The (current) established purpose of gentle intros is to onboard, not to sell - although this can change!)

 - Relating this content to the previous section's content, so it doesn't feel like it was authored in isolation

 - Explain in a style that's consistent with editorial norms (may not be better or worse - consistency is the objective)

 - Reduce information that doesn't help us answer the question for the reader (we can add another section to address things like security proofs, developer reach, etc if we want to convey that information in the gentle intro - but that seems unrelated to interop IMO - could be misunderstanding)

 - Reduce promises, focus on intent, engagement with our audience

 - Reduce bold claims, keep it objective-yet-friendly

 - I'd recommend maintaining an editorial separation of concerns between marketing collateral and docs - gentle intros have a particular purpose, staying power, target audience, structure, reader assumptions, style, voice, etc that will become difficult to maintain if we combine marketing language with the developer docs language. Happy to discuss if we're feeling any strong pushback 

  - one option is to reframe the original version (containing marketing language) within a marketing blog post a la stylus - targeted at readers who vibe with that language, directing traffic to the gentle intro and quickstart via marketing channels. 

 - This document is being built under the assumption that its target readers prefer more succinct, objective, no-nonsense, matter-of-fact product onboarding vs bold promises, predictions, persuasion, and the like.

 - It might be fruitful to align on our interop story, what concrete developer capabilities we're aiming at, etc. This would be the "align" step of content production. I took a stab at a revision below, but we might want to iterate / collaborate on this a bit more.

 -->


### Can my Orbit chain talk to other Orbit chains?

Yes! All Orbit chains are powered by self-managed nodes running their own instance of <a data-quicklook-from="arbitrum-nitro">Arbitrum Nitro</a>'s node software. This software implements both **AnyTrust** and **Rollup** protocols; your Orbit chain can be configured to process and settle transactions using one or the other[^2].

This means that your Orbit chain **isn't a completely isolated blockchain network**. When you launch an Orbit chain, you’re joining an ecosystem of connected chains that can exchange information.

Our small-but-mighty team is hard at work developing tools and patterns that make it easy to launch natively interoperable Orbit chains. Interop features haven't been released just yet, but let us know if you need them - we'd like to learn from you as this capability matures.

Orbit's product roadmap is firmly aligned with Ethereum's vision of a decentralized web - one that makes it easy for users to carry their digital swords, spells, skins, art, tokens, and other assets across digital boundaries of all kinds, without having to worry about security, censorship, or UX friction.


### What should I know about Orbit's licensing?

<!-- v.prev - note that this is a destructive edit - is the change from "Nova chains" -> "Orbit chains" correct? If not, may want to align on what we mean by Nova chains precisely, and why One chains are excluded.

Arbitrum Orbit chains can be built on top of Arbitrum One or Arbitrum Nova, and developers are granted full permission to modify and adapt the Arbitrum Nitro codebase to meet their custom needs. Moreover, the license granted for Arbitrum Nova chains is both perpetual and recursive:

- perpetual — so nobody can ever take the software license away from you.
- recursive — so your Arbitrum Orbit chains can itself host Arbitrum chains on top of it.

The Arbitrum Orbit program does not automatically include chains that settle to non Arbitrum DAO-governed chain. So if you want to launch an Arbitrum Nitro chain as an independent L2 on Ethereum, you’ll need to get a custom license, for which there are two options. Offchain Labs, as the initial developer of the Arbitrum Nitro codebase, is the licensor of the software, and can grant custom licenses. With the launch of the Arbitrum DAO, the DAO was also given co-licensor rights to approve additional L2s on Ethereum that do not settle to a DAO-governed chain. To utilize this mechanism, you’ll need to submit a proposal to the Arbitrum DAO, and it’s at the DAO’s discretion whether it would like to grant the license for an additional L2 chain.

-->

You're granted full permission to modify and adapt the Arbitrum Nitro codebase to meet your needs. The license granted for Orbit chains is both **perpetual** and **recursive**:

- **Perpetual** — nobody can ever take the software license away from you
- **Recursive** — your Orbit chain can itself host other chains governed by the same license

Note that the Arbitrum Orbit license doesn't automatically include chains that settle to a non-Arbitrum-DAO-governed chain. So if you want to launch an Arbitrum Nitro chain as an independent L2 on Ethereum, you'll need to get a custom license. There are two options for this: 

 1. **Ask Offchain Labs**: Offchain Labs, as the initial developer of the Arbitrum Nitro codebase, is the licensor of the software, and can grant custom licenses.
 2. **Propose to the Arbitrum DAO**: With the launch of the Arbitrum DAO, the DAO was also given co-licensor rights to approve additional L2s on Ethereum that do not settle to a DAO-governed chain. To utilize this mechanism, you’ll need to [submit a proposal to the Arbitrum DAO](https://docs.arbitrum.foundation/how-tos/create-submit-dao-proposal), and the DAO will democratically decide whether or not to grant the license for your proposed L2 chain.


### I'd love to tinker with Orbit! What should I do next?

Visit the [Orbit Quickstart](./orbit-quickstart.md), start tinkering, and let us know how it goes - we're excited to learn and grow with you! 🚀


[^1]: Although your Orbit chain will be able to exchange information with other Orbit chains (and the L2 chain that it settles to) by default, you're free to modify your Orbit chain's code as much as you'd like. You can even intentionally make your Orbit chain *incompatible* with other Orbit chains and L2s.
[^2]: Note that Orbit chains can settle to **one** of either Arbitrum Goerli, Arbitrum One, or Arbitrum Nova. This selection is usually made pre-deployment, while you're initially configuring your Orbit chain on the [Orbit chain deployment portal](https://orbit.arbitrum.io/deployment). Orbit chains aren't really meant to "hot swap" between networks; changing the L2 chain that your Orbit chain settles to post-deployment isn't explicitly supported. But you can totally experiment with this use-case.