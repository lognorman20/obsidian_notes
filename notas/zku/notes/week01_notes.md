# Week 1 Notes

## [Intro to ZKP - Elena ](https://www.youtube.com/watch?v=BT88s7_VtC8)

[Link to slides](https://docs.google.com/presentation/d/10JmV3-VxPtdHlrX4MSu-ERH82IonZeLrDdLZ1lJ6Wlc/edit)

### What is a ZKP?
ZK proofs must have three things:
1. Completeness: If the proof is done correctly, it must return the correct result to the verifier and the prover
2. Soundness: If the proof is not done correctly, the proof should fail
3. ZK: both parties should not learn anything other than the fact that the statement in question has been verified or not

Interactive protocols mean that two parties have to go back and forth. A ZK-SNARK is a non-interactive version of this.

ZK-SNARK construction flow:
1. Computation
2. Arithmetic Circuit
3. R1CS (rank 1 constraint system)
4. QAP (quadratic arithmetic program)
5. SNARK


For the first four steps, check out this [tutorial](https://blog.decentriq.ch/zk-snarks-primer-part-one/). For the final step, check out this [tutorial](https://arxiv.org/pdf/1906.07221.pdf).

To understand the BLS12-381 curve, check out [this article](https://hackmd.io/@benjaminion/bls12-381#BLS12-381-For-The-Rest-Of-Us).

zkSNARKS transform a statement into langauge of polynomials. There is a prover, a verifier, and a challange which is the statement that is in question. In order to make the challenge non-interactive, there is a hard coded common reference string (CRS) or structured reference string (SRS) which is part of the trusted setup

#### What is a trusted setup?

[Recommended article from slides](https://medium.com/qed-it/diving-into-the-snarks-setup-phase-b7660242a0d7)

[This article](https://xord.com/research/the-trusted-setup-of-zk-snark/) explains that since the prover might cheat the verifier with a fake proof, someone could give the full representation of a statement to a verifier; however, this is slow and not feasible if we want to make a verifier a stateless node. The artcle notes three different ways that the verifer knows what is being proven:
1. Full statement - the verifier get s a full representation of the relation, runs in linear time to the relation size. This can be done through bulletproofs. They produce a URS (uniform random string) which is a kind of CRS whihc does not encode the relation thus not proviiding succinctness
2. Succinct statement - the verifier get sa succinct and basic representation of a computation whihc is being repeated many times. For example, proving the computation of 100 committments will require only one committment. Examples of this are the BCGTV12 and FRI-based STARK which then further generates a URS with pre-processing that is sublinear of the size of the relation
3. Pre-processing - The vverifier is provided with a summary fo the relation, meaning that the pre-processing changes the relation into some short string. If one wants to have a short verifier running time with a relation which is not uniform then one cannot get rid of the pre-processing of the SNARKS

The trusted setup depends on some non-public randomness to avoid the backgoor of generating proofs. This leads to the argument that pre-processing kills the purpose of ZK proofs, but to accommodate this, SNARKS that have secret reference string are best couple with multi-party computation (MPC). MPC is where even if only one party behaves honestly, then the system will remain secure.

<mark>What is a stateless node?</mark>

The SRS is encrypted in order for it to be reused which requires the use of elliptic curve pairings.

ZKPs are graded on prover time, proof size, verification time. 

<mark>How is proof size calculated?</mark>

zkSnarks (Groth16) have a fairly efficient prover time, constant proof size (192 bytes) and constant/fast verification time, which is why they are so popular.

[Collection of "awesome zkps"](https://github.com/matter-labs/awesome-zero-knowledge-proofs)

***

## [Theoretical Background - ZKU video](https://www.youtube.com/watch?v=9je336QIqAQ)

Usually in application, ZK applications only need two of the three ZK properties. For exmaple,
* Completeness && Soundness - send w in the clear
* Soundness && ZK - the verifier always rejects 
* Completeness && ZK - the verifier always accepts

#### Properties of ZKPs

A *trusted setup* means there is a common file between the prover and the verifier that can be used to help verify the proof that was built by a trustworthy entity.Implies that there must be another protocol that involves multiple people to make sure no one person cannot cheat. Means only one person needs to be honest for the whole protocol is trusted.

A *universal setup* means the setup is built once and can be reused until a certain point. Otherwise the setup might need to change. Increases efficiency. 

*Interaction* is not favorable cuz it implies state. Information must be saved in order for the protocol to work, a lot of information might have to be stored which takes up more time and space than necessary.

*Succinctness* is all about the proof size, the verification speed, and whether the proof's speed is concrete or amortized. Ideally proofs are small or constant. It also must be acknowledged that one can use ZK in order to hide the witness but not actually use ZK because the time complexity of each verification, proof, or whatever is now expensive if a lot of calls are made.

All of these properties allow for the applications of ZKs to work. Examples of this are privacy, mixers, etc..


Extraction and zero knowledge are both hypothetical arguments. In all cases "real world" operation does not allow one property to violate the other. Further, the order of operations doesn't happen. Need to ensure that the real world does not have any leaks.

<mark>Schnorr Signatures</mark>

***

## [Theoretical background of zk-SNARKS and zk-STARKS](https://consensys.net/blog/blockchain-explained/zero-knowledge-proofs-starks-vs-snarks/)

Runtimes of SNARKs vs STARKs vs Bulletproofs

![[Pasted image 20220520233105.png]]
![[Pasted image 20220520233117.png]]

#### SNARKs

At their base, SNARKs depend upon elliptic curves for their security. In addition to that, they reuire a trusted setup, which refers to "the initial creation event of the keys that are used to create the proofs required for private transactions and the verification of those proofs".

When the keys are first created, there is a hidden parameter linked between the verification key and the keys sending private transactions. After the keys are created, the secret used to create those keys must be destroyed. If they are not destroyed, then the secrets could be utilized to forge transactions by false verifications and allowing the holder the ability to create new tokens out of thin air. On top of that, there would be no way to verify the tokens created out of thin air were created out of thin air.

There are various criticisms of SNARKs. The first being that the network must rely on the fact that the trusted setup was done correctly (the secrets within the trusted setup were destroyed). Another is that they are not quantum resistant.

SNARKs have more developemnt and history than STARKs do given there are projects like Zcash, Loopring, and others. This means that there are more [developer libraries](https://zkp.science/) for it than STARKs.

On top of that, SNARKs are cheaper than STARKs. SNARKs "are estimated to require only 24% of gas that STARKs would require" as well as taking up less space than STARKs, so they use less on chain storage. 

#### STARKs

The base technology of STARKs rely on hash functions, thus making STARKs quantum resistant as well as eliminating the need for a trusted setup. They are similar to SNARKs in that they are used to verify a circuit in a zero knowledge proof. The main difference is in the way that they have provers. STARKs do not use a trusted setup because they are able to 