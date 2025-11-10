---
layout: post
title: Fast Matrix Multiplication as Tensor Decomposition
date: 2022-10-26
tags: Linear-Algebra
categories: Linear-Algebra
pseudocode: true
---

This post introduces how matrix mulplication operation itself is encoded as a tensor. And the connection between Fast MatMul (e.g., Strassen) algorithm and tensor decompoistion.

# Matrix Mulplication
Following the notation in this area, we denote the matrix multiplication of M × K
and K × N matrices by <M, N, K>. The below computation can be denoted as <2, 2, 2>. 

$$
\left(\begin{array}{cc} 
C_{11} & C_{12} \\
C_{21} & C_{22}
\end{array}\right)=
\left(\begin{array}{cc} 
A_{11} & A_{12} \\
A_{21} & A_{22}
\end{array}\right)
\left(\begin{array}{cc} 
B_{11} & B_{12} \\
B_{21} & B_{22}
\end{array}\right)
$$ 

The classical Matrix Mulplication (MatMul for short) Algorithm computes the results as 

<!-- $$
M_1 = A_{11}B_{11}, M_2 = A_{12}B_{21}, M_3 = A_{11}B_{12}, M_4 = A_{12}B_{22},\\
M_5 = A_{21}B_{11}, M_6 = A_{22}B_{21}, M_7 = A_{21}B_{12}, M_8 = A_{22}B_{22}, \\
C_{11} = M_1 + M_2, C_{12} = M_3 + M_4, \\
C_{21} = M_5 + M_6, C_{22} = M_7 + M_8
$$  -->

$$
\begin{aligned}
M_1 &= A_{11}B_{11}, &\quad M_2 &= A_{12}B_{21}, &\quad M_3 &= A_{11}B_{12}, &\quad M_4 &= A_{12}B_{22}, \\
M_5 &= A_{21}B_{11}, &\quad M_6 &= A_{22}B_{21}, &\quad M_7 &= A_{21}B_{12}, &\quad M_8 &= A_{22}B_{22}, \\
C_{11} &= M_1 + M_2, &\quad C_{12} &= M_3 + M_4,
&\quad C_{21} &= M_5 + M_6, &\quad C_{22} &= M_7 + M_8
\end{aligned}
$$

For <2,2,2>, the classic algorithm involves 8 multiplication and 4 addition. For problem <4, 4, 4>, we can solve it recurively by applying tiled matrix mulplication. Namely, for <4, 4, 4>, each $A_{ij}$ and $B_{ij}$ are 2 x 2 matrix instead of a scaler.
Further, for any problem <N, N, N> where N is the power of 2, we can apply divide-and-conquer scheme to solve it recursively. Namely, for <N, N, N> problem, we decompose it to 8 <N/2, N/2, N/2> problem ($M_1-M_8$), following by 4 element-wise matrix addition. Here we use $T(N)$ to denote the Floating Point Operations per Second (FLOPs) required to solve <N, N, N> problem. By applying divide-and-conquer, we have $T(N)=8T(N/2)+4(N/2)^2$ (The tailing term obtains after 4 element-wise matrix addition between two N/2 x N/2 matrix). Thus by [Master Theorem](https://en.wikipedia.org/wiki/Master_theorem_(analysis_of_algorithms)), the FLOPs of classic matmul is $2N^3-N^2$.

Without loss of generality, **if we can find a way to solve <$k,k,k$> problem with #$a$ mulplications and #$b$ additions, then we can solve any <N, N, N> problem where N is the power of k, by decomposing it to <k, k, k> problem. Then we have $T(N)=aT(N/k)+b(N/k)^2$.**

# Fast Matrix Mulplication

By inspecting the equation $T(n)=8T(n/2)+4$, **we can found that the Big-O complexity is only determined by the number of multiplications.**
We can generalize the above matrix mulplication analysis beyond <2, 2, 2>. Specifically, if we can find a computation scheme to solve <P, P, P> problem with $m$ multiplications, then for any problem <N, N, N>, we can reduce it to $m$ <N/P, N/P, N/P> problem and solve it recursively. Again by Master Theorem, the Big-O complexity $O(n^{log_P M})$.
The idea of fast matrix multiplication algorithms is to trade more matrix additions in return for fewer recursive matrix multiplications. Below I use the Strassen Algorithm as an example:

### Strassen’s $$O(N^{2.87})$$ Matmul Algorithm 

[The Strassen algorithm (1969)](https://en.wikipedia.org/wiki/Strassen_algorithm) was the first to show that matrix multiplication can be done in fewer than $$O(N^3)$$ operations. For the <2,2,2> case it reduces the number of recursive multiplications from **8 to 7** by trading in more additions/subtractions.

We compute seven intermediate matrices:

$$
\begin{aligned}
M_1 &= (A_{11} + A_{22})(B_{11} + B_{22}) \\
M_2 &= (A_{21} + A_{22})B_{11} \\
M_3 &= A_{11}(B_{12} - B_{22}) \\
M_4 &= A_{22}(B_{21} - B_{11}) \\
M_5 &= (A_{11} + A_{12})B_{22} \\
M_6 &= (A_{21} - A_{11})(B_{11} + B_{12}) \\
M_7 &= (A_{12} - A_{22})(B_{21} + B_{22})
\end{aligned}
$$

Then assemble the result \( C = AB \):

$$
\begin{aligned}
C_{11} &= M_1 + M_4 - M_5 + M_7, \\
C_{12} &= M_3 + M_5, \\
C_{21} &= M_2 + M_4, \\
C_{22} &= M_1 - M_2 + M_3 + M_6.
\end{aligned}
$$
