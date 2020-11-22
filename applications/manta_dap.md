# Open Grant Proposal

> This document is referenced in the terms and conditions and therefore needs to contain all the required information. Don't remove any of the mandatory parts presented in bold letters or as headlines! See the [Open Grants Program Process](https://github.com/w3f/Open-Grants-Program/blob/master/README_2.md) on how to submit a proposal.

* **Project:** Manta Decentralized Anonymous Payment Scheme
* **Proposer:** [Manta-Network](https://github.com/Manta-Network/Manta-Whitepaper/blob/main/manta-whitepaper.pdf)
* **Payment Address:** bc1qcnghrzsfwnnv5fuh8095a6g3cf820c9y7t432q


## Project Overview :page_facing_up:
Manta DAP is a decentralized anonymous payment scheme for Polkadot and Parachain assets.

### Overview

[Manta Network](www.manta.network) team aims to develop a private preserving decentralized exchange on Polkadot eco-system using zkSNARK. The cryptographic construction paper can be found [here]((https://github.com/Manta-Network/Manta-Whitepaper/blob/main/manta-whitepaper.pdf)). Below is the Manta architecture:

    ------------------------------------------
    | Decentralzied Anonymous eXchange (DAX) |
    ------------------------------------------
    | Decentralized Anonymous Payment (DAP)  |  <---- This grant
    ------------------------------------------
    | Polkadot and Parachain Assets          |
    ------------------------------------------

Manta DAP's design is similar to ZCash, the difference is that it is designed to support Manta DAX scheme. The detailed protocol can be find in Section 3 of the 
[white paper](https://github.com/Manta-Network/Manta-Whitepaper/blob/main/manta-whitepaper.pdf). It will based on the following cryptographic primitives:
* A statiscally-hidding non-interative commitment scheme, we are planning to use Pedersen hash in the actual construction.
* zkSNARK, we are planning to use [arkworks snark construction](https://github.com/arkworks-rs/snark) based on Groth16.
* Public key encryption, we are planning to use the ZK friendly ElGamal scheme.

### Project Details

Manta DAP has support two kinds transactions:
* `tx_mint`: mint private coins from public coins (base coins)
* `tx_transfer`: transfer private coins to private/public coins

#### Mint private coins from base coins

To mint a new private coin, a user `u` needs to initiate a coin minting transaction
`tx_mint` with the deposit of base coin, more specically:
1. `u` samples a random number, which is a secret value that determines the private coins serial number (Note: `tx_mint` didn't include this number).
2. `u` commits her public key `a_pk`, the value of the coin `v`, and the random secret `s` that she sampled in the last step in a two stage commitments.
3. `u` thus mint a private coin and only put the value, the first stage commitment, the random secret for the second stage commitment, and the final commitment.
4. The ledger verifies that the `u` indeed deposits base coin of value `v` and add the final commitment to the merkle tree that represent ledger state.

#### Transfer private coins

Private coins can be spent and transferred by `tx_transfer`, which takes a set of input private coins to be consumed, and transfers their total value to a set of output coins: which could be either private or public.

To transfer a private coin, a user need to provide a zero knowledge proof of she knows 
the old coins, new coins, and the secret key of old coin such that:
1. both the new coins and old coins are well formed
2. the address secret key matches the public key
3. the old coin's commitment appears as a leaf of the merkle tree representing ledger state
4. The set of old coins and the set of new coins have the same total value (minus transaction fee).

The new coin will be posted on chain using public key encryption.

### Implementation 

Manta DAP will be a substrate module. The ledger state will be a list of Merkle roots, 
i.e. `(L_A, L_B, ..., L_N)`, where each merkle root represents the commitment list of a base coin. Manta DAP also support registering new coins that is compatiable with Manta DAP interface.  

### Ecosystem Fit

As the first decentralized anonymous payment that could support existing assets, Manta DAP will be a great addition to Polkadot eco-system since Polkadot and Parachain assets such as Acala can be transferred and spent privately. We already talked to Polkadot eco-system members such as Acala and Equilibrium. They showed strong interests of integrating with Manta. Also Manta DAP will be the an important building block for Manta DAX (Decentralized Anonymous eXchange) scheme that enables privacy preserving AMM style decentralized exchange using zkSNARK.  

## Team :busts_in_silhouette:

### Team members

* **Shumo Chu, Co-Founder**.
* **Victor Ji, Co-Founder**.
* **Zhenfei Zhang, Researcher**.
* **Qiudong  Xia, Cryptographic System Engineer**.

Detailed experience see **Team's experience section**.


### Team Website
* Website: https://www.manta.network/
* Whitepaper: https://github.com/Manta-Network/Manta-Whitepaper/blob/main/manta-whitepaper.pdf

### Legal Structure
BVI company.

### Team's experience

* Shumo Chu: Shumo is an assistant professor at UCSB. He obtained a Ph.D. in Computer Science and Engineering from University of Washington. Before UCSB, he served as a research scientist in a crypto startup Algorand. He helped algorand developed [PyTeal](https://github.com/algorand/pyteal), a smart contract DSL in Python. He published more than 10 peer-reviewed papers in top conferences in database systems and programming languages such as SIGMOD, VLDB, and PLDI and won the SIGMOD Best Demo Award.

* Victor Ji: Victor is a Harvard University Economics Master. Binance Evangelist. Advisor to Unifi Protocol. He previously served as the Chair of Harvard Kennedy School Blockchain and Cryptocurrency PIC. He is a columnist of Chainnews, 8Btc, Mars finance. Before Harvard, he was the executive partner of BitBlock Capital and worked at Ontology.

* Zhenfei Zhang: Zhenfei obtained his Ph.D. in computer science at University of Wollongong, Australia. He was the director of cryptographic research at OnBoard Security, and a cryptographic engineer at Algorand. Zhenfei has published over 25 papers in cryptography; contributed to multiple proposals to NIST post-quantum cryptography competition; and co-drafted the BLS IETF Internet-draft.

* Qiudong Xia, Crypto System Engineer: Master student at University of Science and Technology of China. Qiudong has been working actively on access control and security of networking systems and published papers in top networking conferences and journals such as IEEE INFOCOM, ACM Trans. Networking, and ICC.


### Team Code Repos
* https://github.com/Manta-Network/Stingray

### Team LinkedIn Profiles
* https://www.linkedin.com/in/shumo-chu-a1722416/
* https://www.linkedin.com/in/canghai-victor-ji-cpa-37688a5b/
* https://www.linkedin.com/in/zhenfeizhang/
* https://www.linkedin.com/in/qiudong-xia-2935761aa

## Development Roadmap :nut_and_bolt:


### Overview
* **Total Estimated Duration:** 2.5 month
* **Full-time equivalent (FTE):**  2
* **Total Costs:** 1.8 BTC

### Milestone 1 — Manta DAP Protocol Prototype
* **Estimated Duration:** 6 weeks
* **FTE:**  2
* **Costs:** 0.9 BTC

| Number | Deliverable | Specification |
| ------------- | ------------- | ------------- |
| 0a. | License | Apache 2.0 |
| 1. | Stingray::Algebra Rust Strate | We will create Stingray Rust crate that contains the finite fields and operations, generic elliptic curves, FFT, and pairings with API documentations. |
| 2. | Micro-benmark | micro-benchmark results of the above primitives and compared them with their counterparts in [arkworks::algebra] |
| 3. | Docker | We will provide a dockerfile to show that we can use Stingray::Algebra to generate substrate pallet. |


### Milestone 2 — Manta DAP Client and Integration

* **Estimated Duration:** 6 weeks
* **FTE:**  2
* **Costs:** 0.9 BTC

| Number | Deliverable | Specification |
| ------------- | ------------- | ------------- |
| 0a. | License | Apache 2.0 |
| 1. | Stingray::Curve Rust crate | implementation of BLS12-381 and BLS12-377 and their Edward version with API documentations |
| 2. | Stingray::Schnorr | implementation of Schnorr signature scheme on Edward embedded curves, provided by Stingray::Curve | 
| 3. | Stingray::BLS-Signature Rust crate | implementation of [BLS signature scheme](https://datatracker.ietf.org/doc/draft-irtf-cfrg-bls-signature/?include_text=1) on BLS12-381 curve provided by Stingray::Curve with API documentation  |
| 4. | Stingray::Pedersen Rust crate | implementation of Pedersen hash on both curves provided by Stingray::Curve with API documentation |
| 5. | Stingray::ElGamal Rust crate | implementation of ElGamal encryption scheme on both curves provided by Stingray::Curve with API documentation |
| 6. | Docker | A docker file to show that all these crate are substrate compatible |
| 7. | Benchmark | micro-benmark results of the Stingray::curves primitives and compared them with their counterparts in [arkworks::curves], benchmark results of BLS-Sigature, Pedersen, and ElGamal|

### Community engagement

As part of the Program, we plan to publish several medium articles/tutorials:
* general guide on how to build substrates compatible cryptographic primitives
* tutorials of using each libraries

## Future Plans

Stingray is part of [Manta](https://manta.network) project. We plan to finish Stingray stack and keep adding new features to Stingray, including:
* stingray::R1CS and stingray::snark crates
* common gadgets implemented using stingray::snark
* a ceremony tool to do decentralized trusted setup using MPC (secure multi-party computation)
* lead a community effort to create a open standard for DApp using zkSNARK on Polkadot

## Additional Information :heavy_plus_sign:

Fun fact: all Manta projects are named after sea creatures.
