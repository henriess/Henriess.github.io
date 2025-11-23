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
2 & -5 & 1 & 0
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

RREF additionally requires:

4. Each column containing a leading 1 has **0s everywhere else**  

---

## Solving Linear Systems

To solve a system of linear equations, we reduce its augmented matrix to **REF**, or ideally **RREF**.

---

## General Matrix

A general \( n \times n \) matrix is:

$$
A = \begin{bmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{n1} & a_{n2} & \cdots & a_{nn}
\end{bmatrix}
$$

The **trace** of a square matrix is:

$$
\text{tr}(A) = a_{11} + a_{22} + \cdots + a_{nn}
$$

---

## Addition and Subtraction of Matrices

For matrices \(A\) and \(B\):

$$
(A + B)_{ij} = A_{ij} + B_{ij}
$$

$$
(A - B)_{ij} = A_{ij} - B_{ij}
$$

---

## Multiplication by a Constant

$$
(kA)_{ij} = k \cdot A_{ij}
$$

---

## Matrix Multiplication

For the product \(AB\) to be defined,  
the number of **columns of \(A\)** must equal the number of **rows of \(B\)**.

Let

$$
A = \begin{bmatrix}
3 & 0 \\
-2 & 2 \\
1 & 1
\end{bmatrix}
\qquad
B = \begin{bmatrix}
1 & 4 & 2 \\
3 & 1 & 5
\end{bmatrix}
$$

We fill the product matrix **column by column**.

### Example: First Column of \(AB\)

1. **Row 1 of A × Column 1 of B**

$$
AB_{11} = 3(1) + 0(3) = 3
$$

2. **Row 2 of A × Column 1 of B**

$$
AB_{21} = (-2)(1) + 2(3) = 5
$$

3. **Row 3 of A × Column 1 of B**

$$
AB_{31} = 1(1) + 1(3) = 4
$$


Repeating this for each column of \(B\), we obtain:

$$
AB = \begin{bmatrix}
3 & 12 & 6 \\
5 & -2 & 8 \\
4 & 5 & 7
\end{bmatrix}
$$

---

## Transpose of a Matrix

If \(A\) is a matrix, the **transpose** \(A^T\) is obtained by swapping rows and columns.

For example:

$$
A = \begin{pmatrix}
2 & 3 \\
1 & 4 \\
-5 & 6
\end{pmatrix}
$$

Then:

$$
A^T = \begin{pmatrix}
2 & 1 & -5 \\
3 & 4 & 6
\end{pmatrix}
$$

---

## Matrix Laws 
`A + B = B + A`\
`A + (B + C) = (A + B) + C`\
`A(BC) = (AB)C`\
`A(B+C) = AB + AC`

## Transpose Laws

$$
(A^T)^T = A
$$

$$
(A + B)^T = A^T + B^T
$$

$$
(kA)^T = kA^T
$$

$$
(AB)^T = B^T A^T
$$

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
