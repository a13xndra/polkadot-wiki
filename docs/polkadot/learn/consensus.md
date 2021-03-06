# Polkadot Consensus

## Why do we need consensus?

Consensus is a method for coming to agreement over a shared state. In order for the state of the blockchain to continue to build and move forward, all nodes in the network must agree and come to consensus. It is the way that the nodes in a decentralized network are able to stay synced with each other. Without consensus for the decentralized network of nodes in a blockchain, there is no way to ensure that the state one node believes is true will be shared by the other nodes. Consensus aims to provide the _objective_ view of the state amid participants which each have their own _subjective_ views of the network. It is the process for which these nodes communicate and come to agreement, and are able to build new blocks.

## What are PoW and PoS?

Proof of Work (PoW) and Proof of Stake (PoS) have been used as short hand to refer to consensus mechanisms of blockchains, but that does not capture the full picture. PoW is the method for agreeing on a block author and part of the fuller [Nakamoto consensus](#nakamoto-consensus) that also encompasses a chain selection algorithm (longest chain rule in Bitcoin). Similarly, PoS is a set of rules for selecting the validator set and does not specify a chain selection rule or how a chain might reach finality. PoS algorithms have traditionally been paired with an algorithm for coming to Byzantine agreement between nodes. For example, [Tendermint](./comparisons/cosmos.md) is a practical Byzantine fault tolerant algorithm which uses PoS as its validator set selection method.

## Probable vs. provable finality

A pure Nakamoto consensus blockchain which runs PoW is only able to achieve the notion of _probable finality_ and reach _eventual consensus_. Probable finality means that under some assumptions about the network and participants, if we see a few blocks building on a given block, we can estimate the probability that it is final.  Eventual consensus means that at some point in the future, all nodes will agree on the truthfulness of one set of data.  This eventual consensus may take a long time and will not be able to be determined how long it will take ahead of time.  However, finality gadgets such as GRANDPA or Ethereum's Casper FFG are designed to give stronger and quicker guarantees on the finality of blocks - specifically, that they can never be reverted after some process of byzantine agreements has taken place. The notion of irreversible consensus is known as _provable finality._

In the GRANDPA paper, it is phrased in this way:

> We say an oracle A in a protocol is _eventually consistent_ if it returns the same value to all participants after some unspecified time.

## What is GRANDPA/BABE?

### Hybrid Consensus

There are two acronyms we use when we talk about the consensus protocol of Polkadot, GRANDPA and BABE. We talk about both of these acronyms because Polkadot uses what is known as **hybrid consensus**. Hybrid consensus splits up the finality gadget from the block production mechanism. This allows for blocks to be produced more rapidly and the slower finality mechanism to run in a separate process to finalize blocks without risking slower transaction processing. 

Hybrid consensus has been proposed in the past. Notably, it was proposed (now defunct) as a step in Ethereum's transition to proof of stake in [EIP 1011](http://eips.ethereum.org/EIPS/eip-1011) which specified [Casper FFG](#casper-ffg). 

### GRANDPA: Finality Gadget

GRANDPA (GHOST-based Recursive ANcestor Deriving Prefix Agreement) is the finality gadget that is implemented for the Polkadot relay chain.

It works in partially synchronous network model as long as 2/3 of nodes are honest and can cope with 1/5 Byzantine nodes in an asynchronous setting.

A notable distinction is that GRANDPA reaches agreements on chains rather than blocks.

For details on GRANDPA, please see the [GRANDPA page](http://research.web3.foundation/en/latest/polkadot/GRANDPA/) on the W3F Research pages.

### "Polite" GRANDPA

Polite GRANDPA is an implementation method which allows for better network performance among nodes. It is mentioned here because it should be considered part of the specification and replicated by independent teams building clients.

### BABE: Block Production

BABE (Blind Assignment for Blockchain Extension) is the block production mechanism that runs between the validator nodes and determines the authors of new blocks.

BABE assigns block production slots to validators according to stake and using the Polkadot [randomness cycle](./randomness.md).

BABE is comparable as an algorithm to Ouroboros Praos, with some key differences in chain selection rule and slot time adjustments.

For details on BABE please see the [working draft](http://research.web3.foundation/en/latest/polkadot/BABE/Babe/).

## Comparisons

### Nakamoto consensus

Nakamoto consensus consists of the longest chain rule using Proof of Work as its sybil resistance mechanism and leader election.

Nakamoto consensus only gives us probable finality. The most that Nakamoto consensus can ensure that a block in the past will not be reverted is dependent on the property of probable finality. Probable finality states that a block in the past is only as safe as the number of confirmations it has, or the number of blocks that have been built on top of it. As the more blocks are built on top of a specific block in a Proof of Work chain, the more computational work has been expended behind this particular chain. However, it does not guarantee that the chain containing the block will always remain the agreed-upon chain, since an actor with unlimited resources could potentially build a competing chain and expend enough computational resources to create a chain that did not contain a specific block, and the longest chain rule employed in Bitcoin and other Proof of Work chains would move to this new chain as the canonical one.

### PBFT / Tendermint

Please see the [relevant section](./comparisons/cosmos.md#consensus) in the Cosmos comparison article.

<!-- ### HoneyBadgerBFT -->

### Casper FFG

The two main differences between GRANDPA and Casper FFG (Friendly Finality Gadget) are:

 - in GRANDPA, different voters can cast votes simultaneously for blocks at different heights
 - GRANDPA only depends on finalized blocks to affect the fork-choice rule of the underlying block production mechanism

### Casper CBC

_Coming soon!_

<!-- ### Avalanche -->

## Why not PoW?

Although effective in coming to a decentralized consensus, PoW consumes an incredible amount of energy, has no economic finality, and has no effective strategy in resisting cartels.

## Resources

- [GRANDPA paper](https://github.com/w3f/consensus/blob/master/pdf/grandpa.pdf) - The academic description of the GRANDPA finality gadget. Contains formal proofs of the algorithm.
- [Rust implementation](https://github.com/paritytech/finality-grandpa) - The reference implementation and the accompanying [Substrate runtime module](https://github.com/paritytech/substrate/blob/master/srml/grandpa/src/lib.rs).

<!-- ## Consensus in Polkadot

### Block Production

### Finality Gadget

### NPoS -->
