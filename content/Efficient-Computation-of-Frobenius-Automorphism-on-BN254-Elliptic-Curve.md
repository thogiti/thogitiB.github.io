+++
title = "Efficient Computation of Frobenius Automorphism on BN254 Elliptic Curve"

[taxonomies]
tags = ["zk", "ZKP", "Writing Zero Knowledge Proofs", "Frobenius Automorphism", "BN254", "Elliptic Curve", "BN254 Elliptic Curve", "sagemath", ]
+++

In this blog post, we will explore an efficient method to compute the Frobenius automorphism for the BN254 elliptic curve. The BN254 curve is a pairing-friendly elliptic curve that is widely used in cryptographic applications [hackmd.io/@jpw](https://hackmd.io/@jpw/bn254). We will exploit the fact that $(p−1)/2$ is odd to compute the Frobenius automorphism efficiently.

- [What is Frobenius Automorphism?](#what-is-frobenius-automorphism)
- [Efficient Computation Using Exponentiation by Squaring](#efficient-computation-using-exponentiation-by-squaring)
  - [Step 1: Define the prime $p$ and the curve constant $b$](#step-1-define-the-prime-p-and-the-curve-constant-b)
  - [Step 2: Construct the elliptic curve $E$ over $F\_p$](#step-2-construct-the-elliptic-curve-e-over-f_p)
  - [Step 3: Frobenius automorphism using exponentiation by squaring technique](#step-3-frobenius-automorphism-using-exponentiation-by-squaring-technique)
  - [Step 4: Apply the Frobenius automorphism to a point on the curve](#step-4-apply-the-frobenius-automorphism-to-a-point-on-the-curve)


# [What is Frobenius Automorphism?](#what-is-frobenius-automorphism)

The Frobenius automorphism is an endomorphism that maps a point $(x,y)$ on an elliptic curve defined over a prime field $F_p$ to $(x^p,y^p)$. It is an important property of elliptic curves over finite fields and has various applications in cryptography [math.stackexchange.com](https://math.stackexchange.com/questions/4377341/explanation-of-frobenius-endomorphism-on-elliptic-curves).

# [Efficient Computation Using Exponentiation by Squaring](#efficient-computation-using-exponentiation-by-squaring)

To compute the Frobenius automorphism efficiently, we will use the exponentiation by squaring technique. This method allows us to compute $x^p$ and $y^p$ more efficiently by exploiting the fact that $(p−1)/2$ is odd.

Let's walk through the implementation step by step:

## [Step 1: Define the prime $p$ and the curve constant $b$](#step-1-define-the-prime-and-the-curve-constant)

We first define the prime $p$ and the curve constant $b$, which are the parameters for the BN254 elliptic curve.

```python
p = 16798108731015832284940804142231733909889187121439069848933715426072753864723
b = 3
```

## Step 2: Construct the elliptic curve $E$ over $F_p$

Next, we construct the elliptic curve $E$ over the finite field $F_p$.

```python
F_p = GF(p)
E = EllipticCurve(F_p, [0, b])
```

## [Step 3: Frobenius automorphism using exponentiation by squaring technique](#step-3-frobenius-automorphism-using-exponentiation-by-squaring-technique)

We define the Frobenius automorphism by using the property of the prime field and the exponentiation by squaring technique. The `exponentiation_by_squaring` function computes the result of $x^n % mod$ efficiently.

```python
def exponentiation_by_squaring(x, n, mod):
    result = 1
    while n > 0:
        if n % 2 == 1:
            result = (result * x) % mod
        x = (x * x) % mod
        n //= 2
    return result

def frobenius_on_curve_efficient(P):
    x, y = P.xy()
    x_frob = exponentiation_by_squaring(x, p, p)
    y_frob = exponentiation_by_squaring(y, p, p)
    return E.point([x_frob, y_frob])
```

## [Step 4: Apply the Frobenius automorphism to a point on the curve](#step-4-apply-the-frobenius-automorphism-to-a-point-on-the-curve)

Finally, we apply the Frobenius automorphism to a random point $P$ on the elliptic curve.

```python
P = E.random_point()
frob_P_efficient = frobenius_on_curve_efficient(P)

print(f"Point P: {P}")
print(f"Frobenius(P) (efficient): {frob_P_efficient}")
```

Here is the complete code [github.com/thogiti](https://github.com/thogiti/ECC-BN254-Frobenius/blob/main/ECC-BN254-Frobenius.sage) for the efficient computation of Frobenius Automorphism on BN254 elliptic curve:

```python
# BN254 curve is a pairing-friendly curve defined over a prime field $F_p$, where $p$ is a 254-bit prime. 
# The curve equation is $y^2=x^3+b$, where $b$ is a curve constant.

# Define the prime $p$ and the curve constant $b$
p = 16798108731015832284940804142231733909889187121439069848933715426072753864723
b = 3

# Construct the elliptic curve $E$ over $F_p$
F_p = GF(p)
E = EllipticCurve(F_p, [0, b])

# Define the Frobenius automorphism for the elliptic curve using the property of the prime field and the exponentiation by squaring technique
def exponentiation_by_squaring(x, n, mod):
    result = 1
    while n > 0:
        if n % 2 == 1:
            result = (result * x) % mod
        x = (x * x) % mod
        n //= 2
    return result

def frobenius_on_curve_efficient(P):
    x, y = P.xy()
    x_frob = exponentiation_by_squaring(x, p, p)
    y_frob = exponentiation_by_squaring(y, p, p)
    return E.point([x_frob, y_frob])

# Apply the Frobenius automorphism to a point on the curve
P = E.random_point()
frob_P_efficient = frobenius_on_curve_efficient(P)

print(f"Point P: {P}")
print(f"Frobenius(P) (efficient): {frob_P_efficient}")

```
