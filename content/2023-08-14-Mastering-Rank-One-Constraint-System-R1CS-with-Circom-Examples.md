+++
title = "Mastering Rank One Constraint System R1CS with Circom Examples"

[taxonomies]
tags = ["R1CS", "ZKP", "Zero knowledge proofs", "Circom", "Constraint system", "Non-linear constraints" ]

[extra]
katex = true

+++

**In Draft Mode**


# [Overview](#overview)

Rank-1 Constraint Systems, or R1CS, are a critical component in cryptographic protocols, especially those related to zero-knowledge proofs such as zk-SNARKS. They are a system of equations used to represent computations, and are particularly useful in scenarios where we want to prove that a certain computation was done correctly, without revealing any other information about the inputs or the computation itself.

In the context of zk-SNARKS, an arithmetic circuit is converted into a R1CS. Each constraint in the R1CS corresponds to one logic gate in the circuit. The "rank-1" refers to the rank of the matrix produced in this conversion process.

In the [previous](https://thogiti.github.io/writing-zero-knowledge-proofs-and-circuits-in-four-languages/#process-flow-of-a-zero-knowledge-proof) blog post, we saw a process flow of creating a Zero Knowledge Proof system. Here is the high level view of this flow:

![Zero Knowledge Proof Process Flow](https://raw.githubusercontent.com/thogiti/thogiti.github.io/master/content/images/20230313/zkp-process-flow-diagram-2023-04-13-150046.png)


Constraint systems are collections of arithmetic constraints over a set of variables. They play an essential role in computational problems, particularly in the realm of cryptographic protocols and zero-knowledge proofs. In a constraint system, there are two types of variables: high-level variables (the secret inputs) and low-level variables (the internal inputs and outputs of the multiplication gates).


## [Understanding the Role of R1CS in Zero-Knowledge Protocols]

Zero-Knowledge (ZK) protocols, which are commonly used in cryptography, require provers to demonstrate that they know a solution to a computational problem. This problem is often expressed as a Rank-1 Constraint System (R1CS). 

An R1CS is essentially a collection of non-linear arithmetic constraints over a set of variables, also referred to as signals. The security of an R1CS primarily relies on its non-linear components, as these are the parts of the system that are challenging for an attacker to solve. 

In contrast, the linear (additive) constraints, despite being part of the system, do not significantly contribute to its security. This is because these constraints are relatively straightforward to solve, making them less effective as a defense against potential attacks.


## [Definition and Explanation of R1CS]

A Rank-1 Constraint System (R1CS) is a specific type of constraint system. It consists of two sets of constraints: multiplicative constraints and linear constraints. Each multiplicative constraint takes the form `a_L * a_R = a_O`, while each linear constraint takes the form` W_L * a_L + W_R * a_R + W_O * a_O = W_V * (v + c)`. Here, `a_L`, `a_R`, and `a_O` represent the left input, right input, and output of each gate in the circuit. `W_L`, `W_R`, `W_O`, and `W_V` are weights applied to their respective inputs and outputs.


## [Relation to Circuits of Logical Gates]

In the context of zk-SNARKs, an arithmetic circuit is converted into an R1CS. Each constraint corresponds to one logic gate in the circuit. This conversion is crucial because zk-SNARKs require computational problems to be expressed as a set of quadratic constraints, which are closely related to circuits of logical gates.

## [Constructing R1CS for Zero Knowledge Proofs]

We'll delve into numerous examples illustrating how to construct Rank-1 Constraint Systems (R1CS) for given polynomial equations or statements, particularly when creating Zero Knowledge Proofs. 

We'll utilize `Circom` and `snarkjs`, two powerful tools that facilitate the representation and handling of non-linear and linear constraints. These will help us understand how circuits are compiled and how a witness is created.

Please note, to follow along with this tutorial, you need to have `snarkjs` and `Circom` installed. You can install them using Node.js as follows:

```bash
npm install snarkjs
```

For Circom installation, follow the official documentation at [docs.circom.io](https://docs.circom.io/getting-started/installation/#installing-dependencies).


# [Circom R1CS Examples]

Before we can create an r1cs, our polynomials and constraints need to be of the form

![r1cs-form](https://raw.githubusercontent.com/thogiti/thogiti.github.io/master/content/images/20230814/r1cs-eq1.jpg)


Also, Circom expects the constraints to be of the form $Aw * Bw - Cw = 0$, where $A$ is the left hand side of matrix, $B$ is the right hand side of matrix and $C$ is the output matrix forms. The variable $w$ is witness vector. Here the witness $w$ will be of the form $[1, out, x, y, ...]$

The matrices $A$, $B$, and $C$ have the same number of columns as the witness venctor $w$, and each column represents the same variable the index is using.


We will see and verify this form by Circom when we compiled the Circom circuits and see the info on the constraints.

## [Example 1]

**Circuit** $out = x*y$

Since our equation is already in the form of quadratic constraint, $out = x * y$  or $A*B-C=0$, where $x$ is the left hand side matrix $A$, $y$ is the right hand side matrix $B$ and $out$ is the results matrix.

The witness $w$ will be $[1, out, x, y]$.

Remember that the matrices $A$, $B$, and $C$ will share the same columns as the witness vector $w$.

$A$ is [0, 0, 1, 0], because only $x$ is present, and no other variables in the polynomial.
$B$ is [0, 0, 0, 1] because the variables in the right hand side are just $y$.
$C$ is [0, 1, 0, 0] because we only have the $out$ variable.

Now, we can verify that $Aw * Bw - Cw = 0$.

You can verify the above calculations of the matrices by running the below sagemath code.

```python

# define the matrices
C = matrix(ZZ, [[0,1,0,0]])
A = matrix(ZZ, [[0,0,1,0]])
B = matrix(ZZ, [[0,0,0,1]])

# witness vector
witness = vector([1, 99, 11, 9])

# Calculate the results
A_witness = A * witness
B_witness = B * witness
C_witness = C * witness
AB_witness = vector([A_witness.dot_product(B_witness)])

# Print the results
print(f"A * witness = {A_witness}")
print(f"B * witness = {B_witness}")
print(f"C * witness = {C_witness}")
print(f"(A * witness) * (B * witness) = {AB_witness}")

# Check the equality
result = C_witness == AB_witness
print(f"result: ",result)

# Check that the equality is true
assert result, "result contains an inequality"

```


## [Example 2]

**Circuit** $out = {x * y * u * v}$

Remember that in R1CS, you can only have at most one multiplication in the constraint. The above equation has three multiplications. We use intermediate variables to *flatten* the polynomial like below.

$$u1 = x * y$$
$$u2 = u * v$$
$$out = u1 * u2$$

The witness vector will be $[1, out, x, y, u, v, u1, u2]$. This makes our matrices $A$, $B$, and $C$ will have eight columns, the same columns as $w$. 

We have three constraints, hene we will have three rows in the matrices $A$, $B$, and $C$. Each row representing the corresponding constraint as written above.


![r1cs-ex2](https://raw.githubusercontent.com/thogiti/thogiti.github.io/master/content/images/20230814/r1cs-ex2.jpg)


**Constructing Matrix A from left hand terms**

The witness vector is $[1, out, x, y, u, v, u1, u2]$.

The matrix A will be of the form:


```math
A_{3,8} = 
\begin{pmatrix}
a_{1,1} & a_{1,2} & a_{1,3} & a_{1,4} & a_{1,5} & a_{1,6} & a_{1,7} & a_{1,8} \\

a_{2,1} & a_{2,2} & a_{2,3} & a_{2,4} & a_{2,5} & a_{2,6} & a_{2,7} & a_{2,8} \\

a_{3,1} & a_{3,2} & a_{3,3} & a_{3,4} & a_{3,5} & a_{3,6} & a_{3,7} & a_{3,8} 
\end{pmatrix}
```

Now, let's fill the matrix $A$ for the first row (first constraint). 

$$u1 = x * y$$


Since, there is only $x$ in the left hand terms. the first row of A will $[0, 0, 1, 0, 0, 0, 0, 0]$.

Hence, after the first constraint, we get


```math 
A_{3,8} = 
\begin{pmatrix}
0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 \\

a_{2,1} & a_{2,2} & a_{2,3} & a_{2,4} & a_{2,5} & a_{2,6} & a_{2,7} & a_{2,8} \\

a_{3,1} & a_{3,2} & a_{3,3} & a_{3,4} & a_{3,5} & a_{3,6} & a_{3,7} & a_{3,8} 
\end{pmatrix}
```


Now, let's fill the matrix $A$ for the second row (second constraint). 


$$u2 = u * v$$


Since there is $u$ in the left hand side term, we get the second row of A to be $[0, 0, 0, 0, 1, 0, 0, 0]$.

Hence, after the second constraint, we get

```math 
A_{3,8} = 
\begin{pmatrix}
0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 \\

0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\

a_{3,1} & a_{3,2} & a_{3,3} & a_{3,4} & a_{3,5} & a_{3,6} & a_{3,7} & a_{3,8} 
\end{pmatrix}
```

Now, let's fill the matrix $A$ for the third row (third constraint). 

$$out = u1 * u2$$

Since there is $u1$ in the left hand side term, we get the third row of A to be $[0, 0, 0, 0, 0, 0, 1, 0]$.

Hence, after the third constraint, we get the final form of matrix $A$.


```math
A = 
\begin{array}{c}
\begin{matrix}
1&out & x & y & u & v & u1 & u2
\end{matrix}\\
\begin{bmatrix}
    0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 \\
    0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\
    0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 
\end{bmatrix}
\color{red}\begin{matrix}R_1\\R_2\\R_3\end{matrix}\hspace{-1em}\\
\end{array}
 ```
 

$$ 
\begin{array}{c}
\begin{matrix}
1&out & x & y & u & v & u1 & u2
\end{matrix}\\\
\begin{bmatrix}
    0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 \\\
    0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\\
    0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 
\end{bmatrix}
\color{red}\begin{matrix}R_1\\R_2\\R_3\end{matrix}\hspace{-1em}\\\
\end{array}
$$


$$
\begin{bmatrix}
    0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 \\\
    0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\\
    0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 
\end{bmatrix}
$$


Here is the final matrix A in the table form:

|      | 1  | out| x  | y  | u  | v  | u1 | u2 |
| ---- | -- | -- | -- | -- | -- | -- | -- | -- |
| R1   |  0 |  0  |  1  |  0  |  0  |  0  |  0  |  0 |
| R2   |  0 |  0  |  0  |  0  |  1  |  0  |  0  |  0 |
| R3   |  0 |  0  |  0  |  0  |  0  |  0  |  1  |  0 |



$$
\begin{bmatrix}
   a & b \\\ 
   c & d
\end{bmatrix}
$$



**Constructing Matrix B from right hand terms**


**Constructing Matrix C from output terms**



## [Example 3]

**Circuit** $out = x*y + 2$


## [Example 4]

**Circuit** $out = 2x^2 + y$


## [Example 5]

**Circuit** $out = 3x^2 + 5xy - x - 2y + 3$


## [Example 6]

**Circuit** $out = x^2 + y$


## [Example 7]

**Circuit** $out = x + y$

