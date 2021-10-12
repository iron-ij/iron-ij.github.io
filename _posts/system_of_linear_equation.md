# 2.1 System of Linear Equation

- 일상 생활속의 많은 문제들이 선형 방정식(linear equation)으로 정의되며, 선형대수학은 이것을 풀 수 있는 방법을 제시하는 학문임
- 실생황을 연결지어보면 일상의 많은 문제들이 선형 방정식으 ㅣ일부라는 것을 알 수 있다, 아래와 같은 문제 역시 선형방정식의 한 예이다

```
Ex1.
가령 내가 투자할 수 있는 돈의 총액이 10만원 일때, 7만원짜리 A주식과 1만원짜리 B주식 5천원 짜리 C주식이 있다면 어떤 비율로 주식을 살 것인가?
```

- 기억속 어딘가에서 배웠던 선형방정식의 해에 대한 조건을 꺼내보자

- 해가 존재하지 않는 케이스

$$x_1 + x_2 + x_3 = 3 \\ x_1 - x_2 + 2x_3 = 2 \\ 2x_1 + 3x_3 = 1$$

- 유일한 해가 존재

$$x_1 + x_2 + x_3 = 3 \\ x_1 -x_2 +2x_3 = 2 \\ x_2 + x_3 = 2$$

- 무수히 많은 해가 존재

$$x_1 + x_2+x_3 = 3 \\ x_1 - x_2 +2x_3 = 2 \\ 2x_1 + 3x_3 = 5$$

### Remark (Geometric Interpretation of Systems of Linear Equations)

- 2개의 변수($x_1, x_2)$를 가지는 2개의 선형방정식은 각각 $x_1x_2-plane$의 정의됨

$$4x_1 + 4x_2 = 5\\
2x_1 - 4x_2 = 1$$

위 식의 해는 $(x_1, x_2) = (1, 1/4)$ 임

- 다음 노테이션은 동치임

$$x_1\begin{bmatrix} a_{11} \\ \vdots \\ a_{m1} \end{bmatrix}+x_{2}\begin{bmatrix} a_{12} \\ \vdots \\ a_{m2} \end{bmatrix}+\ldots +x_{n}\begin{bmatrix} a_{1n} \\ \vdots \\ a_{mn} \end{bmatrix}=\begin{bmatrix} b_1, \\ \vdots \\ b_{n} \end{bmatrix}$$

# 2.2 Matrices

- 선형 대수의 기본적인 표현인 매트릭스에 대해 알아보자

Definition 2.1 (Matrix) with $m,n\in \mathbb{N}$ a real-valued $(m,n)$ matrix A is an m,n-tuple of elements $a_{i,j}$, 

i = 1,...,m j = 1,...,n which is ordered according to a rectangular scheme consisting of m rows and n columns:

$$A=\begin{bmatrix} a_{11} & a_{21} & \ldots & a_{1n} \\ a_{21} & a_{22} & \ldots & a_{2n} \\ \vdots & \vdots & & \vdots \\ a_{m1}, & a_{m2} & \ldots & a_{mn} \end{bmatrix}, \ a_{ij}\in \mathbb{R}$$

by convention (1,n)-matrixces are called **rows** and (m,1)-matrices are called **columns**. these special matrices are also called row/column vectors

- $\mathbb{R} ^{m\times n}$ 은 실수원소의 (m,n) 매트릭스를 의미함
- $A\in \mathbb{R} ^{m\times n}, \ a\in \mathbb{R} ^{mn}$ 은 동일한 표현임

## 2.2.1 Matrix Addition and Multiplication

- 덧셈은 element-wise sum! (즉, 항끼리 더해진다는 의미임)
    
    $$A+B:=\begin{bmatrix} a_{11}+b_{11}, & \ldots a_{1n}+b_{1n} \\ \vdots & \vdots \\ a_{m1},+b_{m1}, & \ldots a_{mn}+b_{mn} \end{bmatrix}\in \mathbb{R} ^{m\times n}$$
    
- 곱셈은 다르게 정의됨 (element-wise sum이 아님!)

$$c_{ij}=\sum ^{n}_{l=1}a_{il}b_{lj} \\ i=1,\ldots ,m, \ j=1,\ldots ,k$$

- 다른 단원에서 내적(dot-product)을 배울텐데 이 경우 명시적으로 dot을 표기함
- 행렬 곱에서 두 벡터의 demension이 다르면, 곱셈한 후에 AB & BA의 사이즈가 다름
- 프로그래밍에서 사용되는 element-wise 곱은 Hadamard product라고도 불림

Definition 2.2 (Identity Matrix). In $\mathbb{R}^{n\times n}$, we define the identity matrix

as the n x n-matrix containing 1 on the diagonal and 0 everywhere else.

$$I_{n}:=\begin{bmatrix} 1 & 0 & \ldots & 0 \\ 0 & 1 & \ldots & 0 \\ \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & \ldots & 1 \end{bmatrix}\in \mathbb{R} ^{n\times n}$$

- Associativity(결합법칙):

$$\begin{aligned}\forall A\in \mathbb{R} ^{m\times n},B\in \mathbb{R} ^{m\times p},C\in \mathbb{R} ^{p\times q}\ :\left( AB\right) C=C\left( AB\right) \end{aligned}$$

- Distributivity(분배법칙):

$$\begin{aligned}\forall A,B\in \mathbb{R} ^{m\times n},C,D\in \mathbb{R} ^{n\times p},\\ \left( A+B\right) C=AC+BC\\ A\left( C+D\right) =AC+AD\end{aligned}$$

- Multiplication with the identity matrix:

$$\forall A\in \mathbb{R} ^{m\times n} \ : \ I_{m}A=AI_{m}=A$$

- note that $I_{m}\neq I_{n}$ for $m \neq n$.

## 2.2.2 Inverse and Transpose

Definition 2.3 (Inverse). Consider a square matrix $A\in \mathbb{R} ^{n\times n}$ . Let matrix $B\in \mathbb{R} ^{n\times n}$have the property that $AB=I_{n}=BA$.  $B$ is Called the inverse of $A$ and denoted by $A^{-1}$

- 모든 행렬들이 역행렬(Inverse matrix)를 가지지 않으며, 역행렬이 존재하면 **regular / Invertible / nonsingular** 라고 부름 (반대로) singular / noninvertible.
- 역행렬

$$\begin{aligned} A^{-1}=\dfrac{1}{a_{11}a_{22}-a_{12}a_{21}}\begin{bmatrix} a_{22} && -a_{12} \\ -a_{21}&& a_{11} \end{bmatrix}\end{aligned}$$

- $a_{1},a_{22}-a_{12}a_{21} \neq 0$ 인 조건에서 2 by 2 matrix의 역행렬이 성립함

Definition 2.4 (Transpose Matrix). for $A\in \mathbb{R} ^{m\times n}$ the matrix $B\in \mathbb{R} ^{n\times m}$ with $a_{ij} = b_{ji}$ is called transpose of A. we write $B = A^{T}$

- invertible & transpose 한 matrix는 다음 성질을 가짐

$$\begin{aligned}AA^{-1}=I=A^{-1}A\\ \left( AB\right) ^{-1}=B^{-1}A^{-1}\\ \left( A+B\right) ^{-1}=A^{-1}+B^{-1}\\ \left( A^{T}\right) ^{T}=A\\ \left( A+B\right) ^{T}=A^{T}+B^{T} \\ \left( AB\right) ^{T}=B^{T}A^{T}\end{aligned}$$

Definition 2.5 (Symmetric Matrix). A matrix $A$ is symmetric if $A = A^{T}$

- Symmetric 한 두 matrix 의 합은 symmetric이지만, 두 matrix의 곱은 반드시 Symmetric 하지는 않음

$$\begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix}\begin{bmatrix} 1 & 1 \\ 1 & 1 \end{bmatrix}=\begin{bmatrix} 1 & 1 \\ 0 & 0 \end{bmatrix}$$

## 2.2.3 Multiplication by a Scalar

- 상수와 Matrix의 곱에는 다음과 같은 성질이 존재함
- let $A\in \mathbb{R} ^{m\times n}$ and $\lambda \in \mathbb{R}$. Then $\lambda A = K$, $K_{ij} = \lambda a_{ij}$.  $\lambda$ scales each elememt of $A$. 
for $\lambda ,\psi \in \mathbb{R}$ 이라고 하면 다음을 만족함
- Associativity

$$\left( \lambda \psi \right) C=\lambda \left( \psi C\right) ,C\in \mathbb{R}^{m\times n} \\ 
\begin{aligned}\lambda \left( BC\right) =\left(\lambda B\right) C=B\left(\lambda C\right) =\left( BC\right) \lambda \\ B\in \mathbb{R} ^{m\times n},C\in \mathbb{R} ^{n\times k}\end{aligned} \\ \left( \lambda C\right) ^{T}=C^{T}\lambda ^{T}=C^{T}\lambda =\lambda C^{T}$$

since $\lambda = \lambda^{T}$ for all $\lambda \in \mathbb{R}$

- Distributivity

$$\begin{aligned}\left( \lambda +\psi \right) c=\lambda C+\psi C\\ C\in \mathbb{R} ^{m\times n}\end{aligned}$$

$$\begin{aligned}\lambda \left( B+C\right) =\lambda B+\lambda C\\ B,C\in \mathbb{R}^{m\times n} \end{aligned}$$

## 2.2.4 Compact Representations of Systems of Linear Equations

- 다음 선형방정식을 아래와 같이 간결하게 표현할 수 있음

$$2x_1 + 3x_2 + 5x_3 = 1 \\ 4x_1 - 2x_2 - 7x_3 = 8 \\ 9x_1 + 5x_2 - 3x_3 = 2 $$

$$
\begin{aligned} \begin{bmatrix} 2 & 3 & 5 \\ 4 & -2 & -7 \\ 9 & 5 & -3 \end{bmatrix}\begin{bmatrix} x_{1} \\ x_{2} \\ x_{3} \end{bmatrix}=\begin{bmatrix} 1 \\ 8 \\ 2 \end{bmatrix}\end{aligned}$$