# Week 1 Assignment

## Part 1 Theoretical Background of zk-SNARKS and zk-STARKS

1) Write down two types of SNARK proofs

Two types of SNARK proofs are PHGR13 and Groth16 SNARKs. They follow the construction flow of computation -> arithmetic circuit -> rank 1 constrain system -> quadratic arithmetic program -> SNARK.

2) Explain in 2-4 sentences why SNARK requries a trusted setup while STARK doesn't

A trusted setup, is defined as "the initial creation event of the keys that are used to create the proofs required for private transactions and the verification of those proofs." This trusted setups allows for efficient verification by a trusted third party after the process of setting up the proof has been completed. On the contrary, STARKs rely on the random oracle model that uses hash functions as random oracles, so the verification process can be done in a post-quantum secure manner; however, STARKs are much less efficient than STARKs and take up more space for this very reason.

3) Name two more differences between SNARK and STARK proofs

Another difference between SNARK and STARK proofs is that STARKs are faster to generate than SNARKs. Further, the cryptographic assumptions of STARKs are less, thus the initial requirements for STARKs are reduced. On top of that, STARKs have collision resistant hashes but SNARKs do not.

## Part 2 Getting started with Circom and snarkjs

1) What does the circuit in HelloWorld.circom do?

The circuit checks that c is the multiplication of a and b. Varibales a and b are signal inputs and c is the output. 

2) Lines 7-12 of compile-HelloWorld.sh download a file called powersOfTau28_hez_final_10.ptau for Phase 1 trusted setup. Read more about how this is generated [here](https://blog.hermez.io/hermez-cryptographic-setup/). What is a Powers of Tau ceremony? Explain why this is important in the setup of zk-SNARK applications.

A Powers of Tau ceremony is