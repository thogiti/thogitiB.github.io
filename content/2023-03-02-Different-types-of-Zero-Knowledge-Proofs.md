+++
title = "Different types of Zero Knowledge Proofs (Interactive and Non-interactive)"

[taxonomies]
tags = ["zk", "ZKP", "Understanding Zero Knowledge Proofs", "Different types of Zero Knowledge Proofs"]
+++

# Overview

Zero knowledge proofs (ZKP) are cryptographic protocols that enable one party (the prover) to prove to another party (the verifier) the truth of a statement without revealing any additional information beyond the statement's validity. ZKPs are incredibly useful in scenarios where confidentiality is paramount, such as digital identity verification, electronic voting systems, and secure communication protocols.

There are two types of zero-knowledge proofs: interactive and non-interactive. In this article, we will explore these two types in-depth, including their differences, advantages, and applications.

# Interactive Zero Knowledge Proofs

Interactive zero-knowledge proofs require communication between the prover and the verifier. The prover and verifier engage in a back-and-forth interaction where the prover sends a series of messages to the verifier. The verifier responds with a challenge message based on the previous messages, and the prover then responds with a corresponding message. This process continues until the verifier is convinced of the statement's truth.

The interactive nature of ZKPs makes them more secure than non-interactive ones since the verifier can adapt its challenges to the prover's behavior, reducing the chance of a successful attack. The downside is that they can be more time-consuming and require more computational resources.

Here are a few examples of interactive zero-knowledge proof algorithms:

## Schnorr Protocol

The Schnorr protocol is an interactive zero-knowledge proof that enables the prover to demonstrate knowledge of a private key associated with a public key without revealing the private key itself. This protocol is used in digital signatures, where the prover must prove they have the private key without revealing it to the verifier.

The protocol works as follows:

    1. Commitment:

    The prover randomly selects a secret value r from the finite field and computes a commitment $\C = g^r$, where g is a generator of the group. The prover sends $\C$ to the verifier.

    2. Challenge:

    The verifier selects a random challenge value e from the finite field and sends it to the prover.

    3. Response:

    The prover computes the response value $\s = r + ex$, where $\x$ is the private key corresponding to a public key $\y = xg$. The prover sends $\s$ to the verifier.

    4. Verification:

    The verifier checks the validity of the proof by computing $\C' = g^s * y^(-e)$, where $\y$ is the public key corresponding to the secret key x. If $\C' = C$, the proof is considered valid.

The Schnorr protocol is a non-interactive zero-knowledge proof, meaning that it requires only one message exchange between the prover and verifier. The proof demonstrates that the prover knows the secret key x corresponding to a public key y, without revealing any information about x. The protocol is used in various applications, such as digital signatures and authentication schemes.

In summary, the Schnorr zero-knowledge proof protocol works by the prover committing to a secret value, the verifier selecting a random challenge, the prover responding with a value that incorporates both the challenge and the secret, and the verifier checking the validity of the proof using the commitment, the response, and the public key.

## Zcash Protocol

The Zcash protocol is an interactive zero-knowledge proof that allows users to prove that a transaction has taken place without revealing the sender, recipient, or transaction amount. The Zcash cryptocurrency uses this protocol to maintain privacy and anonymity in its transactions.

The protocol works as follows:

    1. Circuit setup:

    The prover and verifier agree on a public Boolean circuit that represents the computation that the prover wants to prove. The circuit is transformed into an arithmetic circuit over a finite field, using techniques such as universal circuit constructions.

    2. Input and output encoding:

    The prover encodes the input and output values of the circuit as elements of the finite field, using techniques such as binary encoding or representation as elliptic curve points. The encoding ensures that the input and output values can be processed by the arithmetic circuit.

    3. Constraint system generation:

    The prover generates a constraint system that enforces the constraints of the circuit on the input and output values, using techniques such as the Rank 1 Constraint System (R1CS). The constraint system consists of a set of equations that relate the input and output values to intermediate values computed by the circuit.

    4. Public parameters generation:

    The prover generates a set of public parameters that enable the verifier to check the validity of the proof, without revealing any information about the input and output values. The public parameters consist of a set of group elements and auxiliary data that encode the constraints of the constraint system.

    5. Proving key generation:

    The prover generates a proving key that enables them to construct a proof of correctness for any input and output values that satisfy the constraint system. The proving key consists of a set of group elements that encode the witness values of the constraint system, which are the values that satisfy the constraints.

    6. Proof construction:

    The prover constructs a proof of correctness for the input and output values, using the proving key and the public parameters. The proof consists of a set of group elements that encode the intermediate values of the constraint system, and a set of auxiliary data that enable the verifier to check the consistency of the proof.

    7. Proof verification:

    The verifier checks the validity of the proof, using the public parameters and the proof. The verification process involves checking the consistency of the intermediate values, the satisfiability of the constraints, and the knowledge of the witness values.

If the proof is valid, the verifier is convinced that the prover knows the witness values of the constraint system and has performed the computation correctly, without revealing any information about the witness values or the input and output values. Zcash achieves zero-knowledge properties by using the R1CS constraint system and public key cryptography to encode the constraints and the witness values, and by using efficient cryptographic primitives such as the Groth16 proof system to construct and verify the proofs. The protocol is used in the Zcash cryptocurrency system to enable private transactions that hide the sender, recipient, and amount of the transaction.
 
# Non-Interactive Zero Knowledge Proofs

Non-interactive zero-knowledge proofs require no interaction between the prover and verifier. The prover creates a proof based on a statement and sends it to the verifier, who verifies its validity. The proof is created by hashing the statement and using a cryptographic algorithm to generate a proof that can be verified using a public key.

Non-interactive ZKPs are faster and require fewer computational resources than interactive ones, but they are less secure since the verifier cannot adapt its challenges to the prover's behavior.

Here are a few examples of non-interactive zero-knowledge proof algorithms:

## Fiat-Shamir Transform

The Fiat-Shamir transform is a non-interactive zero-knowledge proof that enables the prover to prove the knowledge of a witness to a statement without interacting with the verifier. This proof is widely used in electronic voting systems, where a voter wants to prove that their vote was correctly recorded without revealing their identity.

The protocol works as follows:

    1. The prover selects a random number r and calculates a commitment value $\c = H(s || r)$, where s is the witness value.
    2. The prover sends $\c$ to the verifier.
    3. The verifier generates a challenge value $\e = H(c)$ and sends it to the prover.
    4. The prover calculates the response value $\z = r + e*s$ and sends $\z$ to the verifier.
    5. The verifier checks that $\c = H(s || (z - e * commitment_value))$, where $\commitment_value$ is a publicly known value.

If the equation holds, the verifier is convinced that the prover knows the witness value $\s$ without revealing it.

## Bulletproofs

Bulletproofs are non-interactive zero-knowledge proofs that are used to verify the validity of range proofs in cryptocurrency transactions. A range proof is a proof that a secret value lies within a certain range.

Bulletproofs are particularly useful in scenarios where computational resources are limited since they can generate smaller proofs than other non-interactive protocols.

The protocol works as follows:

    1. Range proof setup:

    The prover and verifier agree on a range of values that the prover wants to prove, such as the range of a secret value or the range of a transaction amount in a cryptocurrency system. They also agree on a security parameter that determines the soundness and efficiency of the proof.

    2. Pedersen commitment:

    The prover commits to a value that lies within the range, using a Pedersen commitment scheme that combines the value with a randomly chosen blinding factor. The commitment consists of two group elements, C = aG + bH, where G and H are generators of a prime order group, a is the value to be committed, and b is the blinding factor.

    3. Polynomial construction:

    The prover constructs a polynomial that represents the commitment value, using techniques such as Lagrange interpolation or polynomial division. The polynomial has a degree that is logarithmic in the size of the range.

    4. Inner product argument:

    The prover constructs an inner product argument that proves the knowledge of two vectors, consisting of the coefficients of the polynomial and the blinding factors. The argument consists of a constant number of group elements, regardless of the size of the vectors.

    5. Range proof verification:

    The verifier checks whether the commitment value lies within the agreed range, using a combination of range tests and linear equations that depend on the inner product argument. The range tests ensure that the committed value is non-negative and within the specified range, while the linear equations ensure that the commitment value is constructed correctly.

    6. Recursive proof composition:

    To prove the validity of multiple range proofs, the prover and verifier can recursively compose the inner product arguments and the range tests, by constructing a Merkle tree of the proofs and their associated commitments, and using a single proof and commitment pair as input to the next round.

If all checks pass, the verifier is convinced that the prover knows a value within the agreed range, without revealing any information about the value or the blinding factor. Bulletproofs achieve zero-knowledge properties by using polynomial constructions and inner product arguments that hide the committed value and the blinding factor, and by using recursive proof composition to reduce the size of the proofs and commitments. The protocol is widely used in privacy-preserving applications, such as ring signatures and confidential transactions in cryptocurrency systems.

## Sonic

Sonic is a non-interactive zero-knowledge proof protocol designed to verify the correctness of computations on private data, such as those performed by smart contracts in blockchain systems. It is known for its efficiency and scalability, making it suitable for large-scale applications.

The protocol works as follows:

1. Circuit setup:

The prover and verifier agree on a public Boolean circuit that represents the computation that the prover wants to prove. The circuit is transformed into an arithmetic circuit over a finite field, using techniques such as universal circuit constructions.

2. Polynomial commitment:

The prover commits to a set of polynomials that correspond to the input and output wires of the circuit, using polynomial commitment schemes such as KZG or FFT-based schemes. The commitment consists of a set of group elements that encode the coefficients of the polynomials.

3. Polynomial evaluation:

The prover evaluates the committed polynomials at random field elements, and generates a set of proof values that encode the results of the computation at each gate of the circuit. The proof values consist of group elements that are generated by evaluating linear combinations of the commitment values at the random field elements.

4. Fast Fourier transform:

The prover performs a fast Fourier transform on the proof values, in order to convert them into a form that enables efficient verification using low-degree polynomial evaluations.

5. Random sampling:

The verifier randomly samples a subset of the proof values, and sends the sample points to the prover as a challenge.

6. Polynomial interpolation:

The prover interpolates a low-degree polynomial that matches the sample points and the committed polynomials, using techniques such as Lagrange interpolation or polynomial division.

7. Polynomial evaluation check:

The verifier checks whether the polynomial interpolation matches the proof values at all other points, using techniques such as Reed-Solomon decoding or polynomial evaluation over multiple field extensions.

8. Circuit check:

The verifier checks whether the computed output values match the desired output values of the circuit, using techniques such as binary decision diagrams or straight-line programs.

If all checks pass, the verifier is convinced that the prover knows the input values of the circuit and has performed the computation correctly, without revealing any information about the input values. Sonic achieves zero-knowledge properties by using polynomial commitments to hide the input values and fast Fourier transforms to enable efficient verification. The protocol is particularly suitable for verifying computations on private data, such as those performed by smart contracts in blockchain systems.

## Groth16

Groth16 is a non-interactive zero-knowledge proof protocol that is commonly used in privacy-preserving applications, such as anonymous credential systems and private transactions in cryptocurrency systems. It is known for its high efficiency and security, making it a popular choice for large-scale applications.

The protocol works as follows:

    1. Setup:

    The prover and verifier agree on a set of elliptic curve parameters, including a prime order group $\G$ and a pairing function $\e$ that maps elements from $\G$ to the multiplicative group of a finite field. They also agree on a public statement that the prover wants to prove, such as the validity of a transaction or the possession of a private key.

    2. Key generation:

    The prover generates a private key and a corresponding public key. The public key consists of two group elements, $\P$ and $\Q$, such that $\Q = kP$ for some secret scalar $\k$.

    3. Commitment:

    The prover commits to a witness value that satisfies the public statement, such as a transaction input and output or a private key value. The commitment consists of two group elements, $\C$ and $\D$, such that $\C = rP + W$ and $\D = rQ$, where $\r$ is a randomly chosen scalar and $\W$ is the witness value.

    4. Challenge:

    The verifier generates a random challenge value, $\c$, from a secure hash function applied to the public parameters and the commitment values, $\c = H(G, e, P, Q, C, D)$.

    5. Response:

    The prover calculates a response value, $\s$, based on the challenge value and the private key, $\s = r + kc$, where $\k$ is the private key scalar.

    6. Verification:

    The verifier checks whether the commitment and response values satisfy the public statement, by verifying the following equations:

    $\e(C, P) = e(D, Q) + e(sP + cW, P)$

    This equation ensures that the commitment values are valid and that the response value corresponds to the private key scalar and witness value. If the equation holds, the verifier is convinced that the prover knows the private key and the witness value, without revealing any information about them.

Groth16 achieves zero-knowledge properties by using the pairing function to hide the witness value in the commitment value, and by using the random challenge value to prevent the prover from reusing a previous response value. This protocol is widely used in privacy-preserving applications, such as anonymous credential systems and private transactions in cryptocurrency systems.

## Aurora

Aurora is a non-interactive zero-knowledge proof protocol designed to verify the validity of computations performed on encrypted data. It is useful in scenarios where multiple parties want to perform computations on sensitive data without revealing the data to each other.

The protocol works as follows:

* The parties encrypt their data using a public key.
* Each party generates a proof of correctness for their computation on the encrypted data.
* The verifier checks the proofs to ensure that the computations were performed correctly without revealing any information about the data.

Aurora achieves zero-knowledge properties through a combination of homomorphic encryption, zero-knowledge proofs, and other advanced cryptographic techniques.
    
# Conclusion

In conclusion, zero-knowledge proofs are an essential tool in modern cryptography, providing secure and confidential communication protocols in various domains, including digital identity verification, electronic voting systems, and cryptocurrency transactions. Interactive zero-knowledge proofs are more secure but require more computational resources, while non-interactive ones are faster but less secure. It is essential to choose the appropriate type of ZKP for the specific use case, balancing security and performance.
