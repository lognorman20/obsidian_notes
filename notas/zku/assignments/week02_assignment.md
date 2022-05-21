# Week 2 Assignment

lognorman20@gmail.com
theqo#6869
***
## Part 1 Hashes and Merkle Tree

1) Rank the four hashes and provide explanations in four different aspects: gas cost, capacity, proof generation efficiency, and proof size.

When comparing the hash functions SHA256, MiMC, Poseidon, and Pedersen, the following ranking holds true in the various categories:

#### Gas Cost
SHA256 < Poseidon < Pedersen < MiMC

Explanation/References:
* https://ethresear.ch/t/gas-and-circuit-constraint-benchmarks-of-binary-and-quinary-incremental-merkle-trees-using-the-poseidon-hash-function/7446/3

#### Capacity (the number of inputs it can take in circomlib tempaltes)
MiMC ~~ Poseidon > Pedersen > SHA256

* https://ethresear.ch/t/gas-and-circuit-constraint-benchmarks-of-binary-and-quinary-incremental-merkle-trees-using-the-poseidon-hash-function/7446/3
* https://eprint.iacr.org/2019/458.pdf

#### Proof generation efficiency
SHA256 < Poseidon < Pedersen ~~ MiMC

SHA256 is not efficient in zkSNARKS because it does lots of boolean operations whereas the circuit required for a zk Circuit is an arithmetic circuit.

* https://ethresear.ch/t/gas-and-circuit-constraint-benchmarks-of-binary-and-quinary-incremental-merkle-trees-using-the-poseidon-hash-function/7446/3
* https://z.cash/technology/jubjub/

#### Proof size
SHA256 > MiMC > Poseidon ~~ Pedersen

* https://ethresear.ch/t/gas-and-circuit-constraint-benchmarks-of-binary-and-quinary-incremental-merkle-trees-using-the-poseidon-hash-function/7446/3
* https://z.cash/technology/jubjub/

***
## Part 2 Tornado Cash

1) How is Tornado Cash Nova different from Tornado Cash Classic? What are the key upgrades/imporvements and what changes in the technical design make these possible?

First, funds are directly linked to a given wallet address. There is no private note or key. Users can access their funds by connecting to the pool with the appropriate address. SEcond custody is either acquired by the act of depositing tokens int the pool or by registering in the pool and receiving shielded transfers from another address. In order to accomplish these things, a random area of bytes is computed using the Pedersen Hash, then sends the token and the 20 MiMC hash to the smart contract. The contract is then inserted into the Merkle Tree.

To process a withdrawal, the same area of bytes is split int two sepate parts: the secret on one side and the nullifier on the other side. The nullifier is a public input that is sent on-chain to be checked with the smark contract and the Merkle tree data and prevents double spending.

2) What is the role of the relayers in the Tornado Cash protocols? Why are relayers needed?

The relayers are used to pay gas at withdrawal and allows for more decentralization in the network. In a way, relayers are staking their coins to verify transactions on the networ.

***
## Part 3 Semaphore

1) What is Semaphore? Explain in 4-8 sentences how it works.

It is a set of tools that forms a system that allows an authorized user to act on certain information without revealing any information about the user. It is a system that allows any user to signal their endorsement of an arbitrary string, revealing only that they have been previously approved to do so, and not their specific identity. In order for this process to take place, some proofs must be completed. It must be proved that the user is authorized to do the signaling and that they signal is unique, depending on the use case. This is done through smart contracts and circuits utilizing a Merkle Tree.

2) How does Semaphore prevent double signing (or double withdrawal in the case of mixers)? Explain the mechanism in 4-8 sentences.

A Semaphore has multiple mechanisms to prevent double signing or double withdrawal. First is the Merkle tree structure. If any transaction cannot be verified inside of the Merkle tree, then it must false, thus nullifying the possibility of a double sign. Further, the user trying to authenticate is a registered user, meaning that they have a unique authorization token. This token can be used to track the users transactions and understand their true identity and where their tokens have been used.

3) A lot of applications have already been built based on derivations from Semaphore, such as for voting, survey or opinion, and authentication. Can you suggest two more ideas for ZK applications that can be built upon Semaphore?

Resale transaction verification, social media verification.