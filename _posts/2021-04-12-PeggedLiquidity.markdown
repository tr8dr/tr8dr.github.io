---
author: tr8dr
comments: true
date: 2021-04-12 11:00:00+00:00
layout: post
title: Uniswap v3 & Liquidity Provision
subtitle:
categories:
- strategies
---
I had a look at Uniswap's upcoming V3 protocol [overview](https://uniswap.org/blog/uniswap-v3/) and 
[white paper](https://uniswap.org/whitepaper-v3.pdf).  There are some substantial improvements above the 
current V2, such as:

- ability to offer __"concentrated" liquidity__ (we'll discuss this below)
- ability to introduce pools with varying fee structure
- advances in oracle efficiency and new oracle types  
- ..

For a market maker, the most important of these is the ability to "concentrate" liquidity.  What does this 
mean?  As of V2, one's __liquidity was uniformly distributed across all price levels__, as opposed to being distributed
near the prevailing price.  In limit-order book
terms, this would be equivalent to offering 1/k coins at each price level (given a maximum of K price levels).
This is __very capital inefficient__ as most trading occurs near the prevailing market price.

Here is a diagram from the whitepaper illustrating the liquidity allocation of V2 versus possible distributions of
liquidity in the uniswap V3 pool:

<img src="/assets/2021-04-12/concentration.png" width="800" height="350" />

In V3, through the use of range position contracts, one can effect any distribution of liquidity desired. 

## Problems
Being able to center liquidity around the mid price (or whatever price is advantageous for inventory) is an
indispensable tool for market makers (and yield farmers for that matter).   However as nice as this seems on the
surface, has some significant issues:

- transactions in the ethereum blockchain __take 10 - 20 minutes to transact__
- __gas fees are expensive__ and progressing higher due to the large # of ERC tokens on Ethereum

A traditional __market maker will adjust orders (liquidity positions) at high frequency__ to remain competitive and keep
liquidity near the prevailing market price.  With a transaction overhead on the Ethereum blockchain measured in 10 - 20 
minutes, would require the market maker to __spread liquidity across a wider price range__ in order to maintain participation 
in the market, diluting capital efficiency and incurring more risk.

The second issue relates to cost.  Centralized exchange, both traditional and crypto, do not charge fees for order
placement.  The gas fees inherent in updating the liquidity position range on Uniswap would be prohibitive for
traditional market making.

## A Solution: Pegged Liquidity Contract
Uniswap should introduce a __Pegged Liquidity__ order (contract).  The contract would indicate the following:

- __allocate K coins__ according to some distribution __relative to the prevailing price__
- __express the distribution__ as:
  * x coins at level price +/- offset1
  * y coins at level price +/- offset2
  * ...
  * up to a sum of K coins
- __limit the lowest (highest) price__ will offer on 
  * this avoids following the price in a flash crash or just some level below which the market maker considers
    uneconomical
    
As the prevailing market price moves (as determined by the oracle), the __liquidity automatically moves with the price__.  This will
allow the market maker to participate in as many transactions as possible without having to issue a new directive
on the chain to move the liquidity position when the market moves.  This avoids both the ultra-high latency (10 - 20mins) and
the costs (gas) of moving the liquidity to meet the price.

One can imagine other variations of this where the liquidity positioning only changes when the price increments 
by more than k%, keeping the liquidity/price offering constant for smaller moves.
    
The proposed contract solves some of the concentration problem, however, it cannot go as far in offering the
granularity of control that is possible with centralized order books.  With a faster and cheaper blockchain
we can start approaching the efficiency of a central LOB.

## Future of Uniswap
Uniswap's Achilles heel is that it is based on the Ethereum blockchain.  This has served it well until recently as many of the
coins traded on Uniswap are ERC-20 tokens, avoiding the need for Ethereum-transplanted proxy coins.  However, Ethereum
presents substantial problems for a DEX:

- non-deterministic transactions (required K blocks to verify transaction, much like bitcoin)
  * by non-deterministic mean that addition of a transaction to a block does not guarantee the transaction;
    rather the transaction can only be assured after a certain number of blocks are added on the branch with
    the transaction.
  * this increases latency substantially
- scalability concerns (perhaps mitigated with side-chains)
  * however side-chains are not the most elegant solution and may present new attack vectors
- high costs (gas fees)

The future for DEFI / DEX's is likely to be on one of the newer blockchains, ones that:

- provide atomic transactions (ideally)
- higher scalability
- lower transaction costs
- interop with other chains

In this regard, __Cosmos__ looks quite attractive.  There are other blockchains and DEFI projects in competition
for the DEFI space (I will not enumerate them here).  The Ethereum Blockchain is innovating quickly (unlike the Bitcoin 
chain).  The momentum and the early lead of the Ethereum ecosystem could still win out in the end, in spite of the
attractive properties of these newer chains.  

Perhaps, though, the answer is not one L1 chain technology, but rather the projects that allow an __inter__-net
of block chains - allow multiple chains to coexist and seemlessly interoperate.  __Cosmos__ is playing in this direction.


