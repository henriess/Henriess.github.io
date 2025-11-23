---
title: "Linear Algebra 1"
nav_order: 3
---

# Linear Algebra 1 

## Gaussian Elimination 
The system of linear equations : 
1x - 3y + 0z = 2
-1x + y + 5z = 2
2x - 5y + z = 0
can be written as 
$$
\begin{bmatrix}
1 & -3 & 0 & 2 \\
-1 & 1 & 5 & 2 \\
2 & -5 & 1 & 0 \\
\end{bmatrix}
$$

## Operations on a matrix
`1 : Multiply a row by a non-zero constant`
`2 : Interchange 2 rows`
`3 : Add a multiple of one row to another row`

## Row-Echelon Form
`1. Any row that consists entirely of 0s must be at the bottom of the matrix `
`2. The leading number of any non-zero row has to be 1`
`3. The leadning number of any non-zero row has to be to the right of the leading number of the row above it`
## Reduced Row-Echelon Form
`1. In addition to the above 3 properties, any column that has a 1 must have a 0 everywhere else in that column`

To solve system of linear equations, we aim to reduce the matrix to at least the Row-Echelon form 

$$
A = \begin{bmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{n1} & a_{n2} & \cdots & a_{nn}
\end{bmatrix}
$$
The trace of a square matrix is defined as the sum of all entires along the main diagional, that is a_{11} + a_{22} + ... + a_{nn}

## Addition of Matrices 
(A+B)_{ij} = A_{ij} + B_{ij}
(A-B)_{ij} = A_{ij} - B_{ij}


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
