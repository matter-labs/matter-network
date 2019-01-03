# Presenting Ignis: Plasma of Fire

## Ethereum scaling solution with up to 500 transactions per second goes live on testnet

Today is a very special date. [Exactly 10 years ago](https://www.blockchain.com/btc/block/000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f) the launch of Bitcoin network has started a new era in technology and economics, leading to the creation of a mind-blowing multi-billion dollar industry and a huge paradigm shift in people’s minds: the spirit of decentralization and radical self-ownership is here to stay. It is remarkable how powerful and world-changing a single idea can be. And it’s a great reminder that the progress of human race is shaped much sharper by Black Swans and innovative breakthroughs than by incremental development.

10 years of blockchain history have proven the latter point over an over again. Ethereum is another awesome example of a zero-to-one leap. Yet, as one prominent detective from London would have noted, oftentimes things missing speak much more eloquently than things present. Diverse aspects of cryptocurrencies and blockchain technology, in general, have improved over time, but one thing has shown incredible stubbornness and hasn’t changed much since Satoshi’s invention, despite lots of effort: scaling.

We need a little clarification here. Many projects claim to have solved the scaling problem. It is indeed trivial to do so: a single Postgres database can handle millions of transactions easily. The real challenge is: scaling without compromising decentralization and security. This is much, much trickier.

Bitcoin is capable of 5, Ethereum —  of 15 transactions per second. Visa does 2000 TPS on average, with peaks reaching tens of thousands TPS. Take a second and think about it: how would space look like if blockchains could maintain the parity? If big industries and institutional investors expected payments and contractual operations on blockchain to be smooth and reliable on any volumes, and not be easily smashed by something like a cute video game? Where would we be now?

A breakthrough is long overdue.

Well, not exactly. This breakthrough is lurking around: Zero-Knowledge Proofs. It’s a family of ingenious mathematic techniques, allowing us to replace the trust in the correctness of computations by consensus with the trust in Math, the Queen of sciences. You can find the introduction into ZKP and more info about them [here](https://github.com/gluk64/awesome-zero-knowledge-proofs) or Vitalik's [introduction](https://medium.com/@VitalikButerin/zk-snarks-under-the-hood-b33151a013f6).

### Live Demo

Today, we’re happy to present you the first ever 100% functional [alpha version of ZK-SNARKs-driven Plasma](https://ignis.thematter.io/#/wallet), launched live on Rinkeby testnet. We called it *Ignis*: the Fire.

*if you want to have your transactions included immediately - send at least 8 of them in correct sequence otherwise you have to wait until other users fill it.*

For highly technical details one can check out this [article](https://ethresear.ch/t/on-chain-scaling-to-potentially-500-tx-sec-through-mass-tx-validation/3477) from Vitalik. It describes how one can make a system similar to *Ignis*.

*Special note: for the sake of clarity we should note that Ignis is not Plasma in its core due to a different approach to data availability, but for ease of explanation we've used this term.*

![Aether-Ignis Alchemy](https://i.imgur.com/AkN7u2Z.jpg)

## Technical description and design philosophy

We see *Ignis* as a decentralized solution with a multi-operator model from day one. At the current state, the technical demo focuses on the demonstration of a zkSNARKs related workflow and primitive operations, while the multi-operator part is work in progress.

Under the hood, *Ignis* is an L2 solution with *account model* and *verifiable* state transitions. We call "the state" a current balance of all the accounts, so in this case, verifiable state transition means that for every transaction between accounts, for example, following checks are performed and proved by zkSNARK:

- "Sender" had enough funds to make a transfer
- "Sender" has provided a valid signature
- The transaction is well-formed, has proper nonce and validity date (should be included not later than a block N)
- "Sender's" balance was properly reduced and a nonce increased
- "Recipient's" balance was properly increased, and no other information about the recipient's account was changed
- If there was some fee paid for this transaction - this fee was correctly added to the total fee collected over this block

### Workflow from the user's perspective

User's interaction with *Ignis* can take form in four different operations.

- `Registration/deposit` - user can register his public key and obtain a free account number in *Ignis*
- `Transfer` - user can either send a part of his funds to another user or can send them to the special address of zero. Transfer to zero is considered as a partial exit (different from complete exit described below), so as soon as a block with such transaction is verified, the user can withdraw a corresponding amount using the smart-contract in Ethereum network
- `Exit` - this is what we call a "complete exit". Such operation withdraws a complete user's balance and un-registers his public key
- `Emergency exit` - in the case, if *Ignis* network experiences a long-term downtime (all participants are not processing transactions), the smart-contract goes in "emergency mode" and allows every user to exit full balances using the smart-contract

### Multi-operator model

To support efficient multi-operator model we propose the following technical solutions:

- Use a form of delegated Proof-of-Stake model to assemble a pool of M operators that will get some timeframes to make blocks
    - Some form of delegation is required because bookkeeping and proof generation requires some computational power (that's much lower than any form of mining anyway)
- Use a commit-verify approach to allow effective signaling to other operators about the content of the next block
- If an operator that should make blocks in this timeframe experiences technical difficulties with proof generation, a smart-contract may allow the next operator to step in

Operators work in a pipelined mode, so there is some delay between commitment to the block and publication of the proof. Such a procedure is ideal to have a latency of proof generation not to affect a throughput.

![](https://i.imgur.com/fCBqdXA.jpg)

## Next steps
- Move from the "proof of concept" to the production-grade software that would allow anyone to become an operator with ten lines of configuration
- Push the efficiency of the proof-generation software further
- Do the trusted setup! It's an essential part for any zkSNARKs based solution and the main stopping factor for the release on the Ethereum mainnet
- Complete an economic model and delegated PoS for the multi-operator approach

- We post **status update by [link](https://github.com/matterinc/ignis/blob/master/progress/README.md
)**

## Credits
- [ZCash](https://z.cash) for their various contributions to zkSNARKs environment
- Sean Bowe for his [awesome code](https://github.com/zkcrypto/bellman)
- [Karl Floersch](https://karl.tech) for his energy!
- [Vitalik](https://vitalik.ca) for his educational blog posts and technical ones on [ethresear.ch](https://ethresear.ch)
- [BarryWhitehat](https://github.com/barryWhiteHat) and [HarryR](https://github.com/HarryR) for their work on integrating zkSNARKs into Ethereum
- [Ethereum Foundation](https://www.ethereum.org/foundation) for their support


## Build and contribute
- All our work on zkSNARKs is public on the [github](https://github.com/matterinc)
- Code examples can be found in our ETH Singapore repository - [Plasma Winter](https://github.com/matterinc/plasma-winter) and [Plasma Cash history compression](https://github.com/matterinc/plasma_cash_history_snark)

Stay tuned!

Alex Vlasov, Matter Inc.

Petr Korolev, Matter Inc.

Alex Gluchowski, Ethereum Foundation

[Matter Inc.](https://thematter.io/)

[Ignis Demo](https://ignis.thematter.io/#/wallet)
