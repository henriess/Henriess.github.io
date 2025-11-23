---
title: "Linear Algebra 1"
nav_order: 3
---

# Linear Algebra 1

## Gaussian Elimination

The system of linear equations:

$$1x - 3y + 0z = 2$$
$$-1x + y + 5z = 2$$
$$2x - 5y + 1z = 0$$


can be written in augmented matrix form as:

$$
\begin{bmatrix}
1 & -3 & 0 & 2 \\
-1 & 1 & 5 & 2 \\
2 & -5 & 1 & 0 \\
\end{bmatrix}
$$

---

## Operations on a Matrix

The following row operations are allowed:

1. **Multiply** a row by a non-zero constant  
2. **Interchange** two rows  
3. **Add** a multiple of one row to another row  

---

## Row-Echelon Form (REF)

A matrix is in REF if:

1. Any row consisting entirely of **0s** appears at the **bottom**  
2. The **leading entry** of each non-zero row is **1**  
3. Each leading 1 is **to the right** of the leading 1 in the row above  

---

## Reduced Row-Echelon Form (RREF)

In addition to REF properties, RREF requires:

4. Each column containing a leading 1 must have **0s everywhere else**  

---

## Solving Linear Systems

To solve a system of linear equations, we reduce the augmented matrix to **REF** or ideally **RREF**.

---

## General Matrix

A general \( n \times n \) matrix is written as:

$$
A = \begin{bmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{n1} & a_{n2} & \cdots & a_{nn}
\end{bmatrix}
$$

The **trace** of a square matrix is the sum of the diagonal entries:

\[
\text{tr}(A) = a_{11} + a_{22} + \cdots + a_{nn}
\]

---

## Addition and Subtraction of Matrices

For matrices \(A\) and \(B\):

\[
(A + B)_{ij} = A_{ij} + B_{ij}
\]

\[
(A - B)_{ij} = A_{ij} - B_{ij}
\]

<script src="https://giscus.app/client.js"
        data-repo="henriess/henriess.github.io"
        data-repo-id="R_kgDOQZXWAQ"
        data-category="General"
        data-category-id="DIC_kwDOQZXWAc4CyAWa"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="light"
        data-lang="en"
        crossorigin="anonymous"
        async>
</script>
