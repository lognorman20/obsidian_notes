# Week 2 Notes

## Merkle Trees

[Merkle Tree Youtube Video](https://www.youtube.com/watch?v=n6nEPaE7KZ8&t=1s&ab_channel=SmartContractProgrammer)
[Merkle Tree Article](https://decentralizedthoughts.github.io/2020-12-22-what-is-a-merkle-tree/)

### What is a Merkle Tree?

![[Pasted image 20220521023239.png]]

Merkle Trees are used to prove that a transaction was included in a block.

Key concepts of Merkle trees:
* A merkle tree is a collision-resistant hash function that outputs the root hash that can be used to verify transactions
* A verifier who has the root and an element i in the tree can verify the validity of the whole tree by recreating the root via a Merkle proof

Some examples of problems that Merkle trees solve are:
1) Maintaining integrity of files stored on Dropbox.com or file outsourcing
2) Proving a transaction between Alice and Bob occured, or set membership
3) Transmitting files over unreliable channels, or anti-entropy

Process to build a Merkle Tree:
1) Start w/ an array of hashes of items
2) For every two items, add the two items and hash the sum. That is the parent node of those two items
3) Repeat step 2 until the root node is reached

The number of elements in the array must be a power of two in order for the tree to be created. In the case that there is an odd number of elements in the array, the trick is to duplicate the last element and compute the hash of that:

![[Pasted image 20220520233453.png]]

A merkle tree can be used to prove that a transaction is within a block is to take the transactions someone wants to verify, turn them into a merkle tree, and then check if the root nodes are equal.

Another way is to add up all the hashes of all the transactions within the block; however, the issue with this approach is that every single transaction on the blockchain is needed in order to verify whether or not the transaction is legitimate. This can be timely, infeasible, or expensive. By using merkle trees, one only needs to use log(k) transactions in order to verify if a transaction is within a block.

### Code Implementation of Merkle Proof

```
pragma solidity ^0.5.11;

contract MerkleProof {
  function verify (
    bytes32[] memory proof, bytes32 root, bytes32 leaf, uint index
  ) public pure returns (bool) {
    bytes32 hash = leaf;

    // recompute merkle root
    for (uint i = 0; i < proof.length; ++i) { 
      if (index % 2 == 0){
        hash = keccak256(abi.encodePacked(hash, proof[i]));
      } else {
        hash = keccak256(abi.encodePacked(proof[i], hash));
      }

      index = index / 2;
    }
    return hash == root;
  }
}
```

### Hash Function Benchmarks

#### Background
The deeper a Merkle tree, the more gas is required to initialize and append elements to it. On top of that, there are various constraints required by a zk-SNARK circuit to verify a Merkle proof that increases with the tree depth, thus increasing the proving key size, proving time, and circuit file size.

In order to combat this, hash functions are used to generate commitments.

An incremental Merkle tree is optimized for efficient finds as well as being able to verify transactions within a block. A merkle tree's structure permits for anyone to verify a small part of a tree in order to avoid having to recreate cheese entire tree to find one transaction.

The main tradeoffs within in zk merkle trees are between 

1) tree capacity and gas costs for tree initialization and insertions. This impacts the users because they must spend more gas to insert a value into a tree (for instance, to deposit into a mixer).
2) tree capacity and zk-SNARK circuit size which impacts those who need to generate zk-Proofs of set membership (for instance, to withdraw funds from a mixer).

#### Benchmarks
![[Pasted image 20220520233537.png]]
![[Pasted image 20220520233709.png]]
![[Pasted image 20220520233717.png]]
![[Pasted image 20220520233726.png]]
![[Pasted image 20220520233734.png]]![[Pasted image 20220520233743.png]]
![[Pasted image 20220520233753.png]]
![[Pasted image 20220520233801.png]]
![[Pasted image 20220520233810.png]]
Resources:
* https://ethresear.ch/t/gas-and-circuit-constraint-benchmarks-of-binary-and-quinary-incremental-merkle-trees-using-the-poseidon-hash-function/7446
* https://ethresear.ch/t/performance-of-rescue-and-poseidon-hash-functions/7161
* https://medium.com/aztec-protocol/plonk-benchmarks-2-5x-faster-than-groth16-on-mimc-9e1009f96dfe
* https://medium.com/aztec-protocol/plonk-benchmarks-ii-5x-faster-than-groth16-on-pedersen-hashes-ea5285353db0

## ZK Applications Tornado Cash & Semaphore

[Tornado Cash](https://www.youtube.com/watch?v=XSYHDi3KjiE&t=1s&ab_channel=HarmonyProtocol)
[Semaphore](https://medium.com/coinmonks/to-mixers-and-beyond-presenting-semaphore-a-privacy-gadget-built-on-ethereum-4c8b00857c9b)
* https://github.com/TosinShada/anonyvote
* https://github.com/ChubbyCub/survey-on-chain-hh
* https://github.com/violetwee/zkAsk
* https://github.com/interep-project/reputation-service
* https://github.com/tomoima525/continuum
* https://github.com/Harsh8196/Harmony-ZKU/tree/main/ZKPayroll/Backend