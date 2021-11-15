---
title: 2.3 Solving system of linear equations
date: 2021-11-15 21:30:00 +0900
categories: [Math]
tags: [linear equation, mml-book]
use_math: true
---
# 2.3 Solving Systems of Linear Equations

- 이번 장에서는 linear equations의 일반적인 해와 역행렬을 구하는 알고리즘에 대해서 알아보자

## 2.3.1 Particular and General Solution

- 다음 선형시스템이 주어져있다고 가정해보자.

$$\begin{bmatrix} 1 & 0 & 8 & -4 \\ 
0 & 1 & 2 & 12 \end{bmatrix}\begin{bmatrix} x_{1} \\ 
x_{2} \\ x_{3} \\ 
x_{4} \end{bmatrix}=\begin{bmatrix} 42 \\ 
8 \end{bmatrix}$$

- 위의 식은 두개의 선형방정식과 4개의 미지수가 주어져 있음 ⇒ 이런 경우 일반적으로 무수히 많은 해가 존재
- $\sum^{4} _{i=1} x _{i} c _{i} =b$를 만족하는 $x_i$의 형태를 찾아야 함
- 가장 먼저 가정해볼 수 있는 것은 첫번째와 두번째 컬럼의 조합으로 $b$를 만족하는 케이스이며, 다음과 같음

$$b=\begin{bmatrix} 42 \\ 
8 \end{bmatrix}=42\begin{bmatrix} 1 \\ 
0 \end{bmatrix}+8\begin{bmatrix} 0 \\ 
1 \end{bmatrix}$$

- $\left[ 42,8,0,0\right] ^{T}$가 제시된 선형시스템을 만족하며, particular solution or special solution이라 불림
- 그러나 위의 해는 유일해는 아님
- 기존에 해에서 부터 0을 더해가며 special solution이 변화하지 않는 다른 해를 찾을 수 있음 ⇒ 선형시스템의 3번째 컬럼을 예를 들어보면...

$$\begin{bmatrix} 8 \\ 2 \end{bmatrix}=8\begin{bmatrix} 1 \\ 0 \end{bmatrix}+2\begin{bmatrix} 0 \\ 1 \end{bmatrix}$$

- 위의 컬럼값을 0으로 만드는 $x_3$의 값을 찾아보자

$$\begin{bmatrix} 1 & 0 & 8 & -4 \\ 0 & 1 & 2 & 12 \end{bmatrix}\left( \lambda _{1}\begin{bmatrix} 8 \\ 2 \\ -1 \\ 0 \end{bmatrix}\right) = \lambda _{1}\left( 8c_{1}+2c_{2}-c_{3}\right) =0$$

- 마찬가지로 $x_4$에도 적용해보면...

$$\begin{bmatrix} 1 & 0 & 8 & -4 \\ 0 & 1 & 2 & 12 \end{bmatrix}\left( \lambda_{2}\begin{bmatrix} -4 \\ 12 \\ 0 \\ -1 \end{bmatrix}\right) =\lambda _{2}(-4c_{1}+12c_2-c_4)=0$$

- 최종 일반해(general solution)는 다음과 같음

$$\{ x\in \mathbb{R} ^{4}:x=\begin{bmatrix} 42 \\ 8 \\ 0 \\ 0 \end{bmatrix}+\lambda_1\begin{bmatrix} 8 \\ 2 \\ -1 \\ 0 \end{bmatrix}+\lambda _{2}\begin{bmatrix} -4 \\ 12 \\ 0 \\ -1 \end{bmatrix},\lambda _{1},\lambda _{2}\in \mathbb{R} \}$$

### Remark

- 위의 해를 도출하는 과정은 다음과 같은 3가지 스텝으로 진행되었음
    1. $Ax=b$를 만족하는 해를 도출
    2. $Ax=0$을 만족하는 모든 해를 구함
    3.  1 + 2를 혼합하여 최종 일반해를 도출함

## 2.3.2 Elementary Tranformations

기본적인 행렬변환을 통해 해를 도출할때는 다음과 같은 단순 과정의 반복으로 해결이 가능함

- 두 행의 위치를 바꿈 (선형시스템의 형태로 표현된 row)
- row에 $\lambda \in \mathbb{R} \backslash {0}$를 곱함
- 두 행을 더함    

### Remark

- 행에서 첫번째로 0이 아닌 coefficient를 $pivot$이라 함
- $pivot$이 오른쪽에 위치할 수록 아래 행으로 배치가 되어야함
- 그러므로, row-echelon form 의 행렬은 계단식("staircase") 구조를 가짐

Definition 2.6(Row-Echelon Form).

⇒ 다음을 만족하는 matrix는 Row-Echelon Form이라 정의함

- 0 만 존재하는 행이라면, matrix의 가장 아래쪽에 위치함
    
    ⇒ 행에 0이 아닌 값이 존재한다면, 0만 존재하는 행보다 위에 위치함
    
- 행에서 처음으로 0이 아닌 값이 먼저 등장(=$pivot$)할 수록 matrix의 위쪽에 위치해야함

### Remark (Basic and Free Variables)

- Row-Echelon Form 의 $pivot$은 $basic \ variables$ 로 정의됨
- 다른 값들은 $free \ variables$라 불림

### Remark (Obtaining a Particular Solution)

- Row-Echelon Form 의 행렬은 특정해를 정의하기 편리한 특징이 있음

### Remark (Reduced Row Echelon Form)

다음을 만족하는 선형시스템은 Reduced Row-Echelon Form 이라 함

- Row-Echelon Form의 선형시스템임
- 모든 $pivot$이 1임
- $pivot$이 각 열에서 0이 아닌 유일한 값임

### Remark (Gaussian Elimination)

- 가우시간 엘리미네이션은 선형변환을 통해 선형시스템을 reduced row-echelon form으로 변환하는 알고리즘임
- 다음 행렬은 reduced row-echelon form을 따르고 있음

$$A=\begin{bmatrix} 1 & 3 & 0 & 0 & 3 \\ 0 & 0 & 1 & 0 & 9 \\ 0 & 0 & 0 & 1 & -4 \end{bmatrix}$$

- 위의 선형시스템의 일반해를 도출하는 키 아이디어는 $pivot$이 없는 컬럼으로 부터 $Ax = 0$ 를 만족하도록 하는 것!
- 2번째 컬럼은 1번째 컬럼의 3배수로 표현

$$\begin{bmatrix} 3 \\ 0 \\ 0 \end{bmatrix}=3\begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix}$$

- 5번째 열은 3*1번째 열 + 9*3번째 열 + -4*4번째열과 동일

$$\begin{bmatrix} 3 \\ 9 \\ -4 \end{bmatrix}=3\begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix}+9\begin{bmatrix} 0 \\ 1 \\ 0 \end{bmatrix}-4\begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix}$$

- 따라서 다음과 같이 나타낼 수 있음

$$\{ x\in \mathbb{R} ^{5}:x=\lambda _{1}\begin{bmatrix} 3 \\ -1 \\ 0 \\ 0 \\ 0 \end{bmatrix}+\lambda _{2}\begin{bmatrix} 3 \\ 0 \\ 9 \\ -4 \\ -1 \end{bmatrix},\lambda_{1},\lambda_{2}\in \mathbb{R} \}$$

## 2.3.3 The Minus-1 Trick

- $Ax=0$을 풀기위한 트릭을 소개하고자함
- $A$를 reduced row-ehcelon form 이라 가정하면 다음과 같은 형태를 띔
    
    <center><img src="https://i.ibb.co/xfh3dG4/row-equal-form.png"></center> 
    
- * 는 실수이며, pivot 이 1인 열은 이외에는 모두 0을 만족한다(rref)
- 이때 $pivot$ 아래에 $\left[ 0 \ldots 0 \ -1 \ 0\ldots 0\right]$ 을 추가하여 변형을 가해줌
    

### Inverse matrix

- 정의로부터 $AX=I_{n}$ 이면, $X=A^{-1}$ 이고, 선형시스템의 형태를 변형해가면서 역행렬을 구할 수 있음
- $[ A \vert I_{n}]$ ⇒ $[ I_{n} \vert A^{-1}]$


## 2.3.4 Algorithms for Solving a System of Linear Equations

- 우선적으로 $Ax = b$ 형태의 선형 시스템을 푸는 방법에 대해서 알아보고자 한다. (해가 존재한다고 가정한다, 해가 없는 케이스는 Chapter9 에서 다룰 예정이다)
- 특별한 경우에는 역행렬 $A^{-1}$이 정의되기 때문에 $Ax=b$ ⇒ $x = A^{-1}b$ 의 형태로 변형하여 해결할 수 있다.
- 그러나 위 케이스는 $A$가 정방행렬이고 역행렬이 존재할 때 가능하다.
- 그러므로, 약한 가정 아래($A$가 선형 독립인 열을 가져야 함)에 다음과 같은 변형을 진행해 볼 수 있다.

$$Ax=b\Leftrightarrow A^{T}Ax \Leftrightarrow A^{T}b \Leftrightarrow x = (A^{T}A)^{-1}A^{T}b$$

- Moore-Penrose pseudo-inverse $(A^{T}A)^{-1}A^{T}$ 를 이용하여 $Ax = b$를 풀 수 있다.
- 그러나 위 과정은 매우 많은 행렬 곱 연산과 역행렬을 구하는 과정이 포함되어 있기 때문에 계산비용이 많이 들고 더구나 역함수 or pseudo-역행렬을 계산하기 위해 부동소수점 연산과정이 많이 포함되는 것은 일반적으로 추천하지 않는다
- 따라서, 선형시스템을 풀기 위해 조금 더 간단한 접근법을 논의 해보고자 함
    1. Gaussian elimination 은 Determinants를 구하기 위해 중요함
    2. 각 벡터가 선형 독립인지 체크하는 과정이 필요함
    3. 행렬의 역행렬을 계산함
    4. 행렬의 rank를 계산함
    5. 행렬의 basis를 계산
- 대부분의 선형시스템이 바로 해를 구할 수 없기 때문에 다양한 접근법이 존재함
    - 리차드슨 메소드
    - 자코비 메소드
    - 가우스 사이델 메소드
- 가령 $x_*$가 $Ax=b$의 해 일때, 반복적인 방법을 통해 해를 도출함

$$x^{(k+1)} = Cx^{(k)} + d$$

- 적당한 $C$와 d를 활용해서 오차를 점차 줄일 수 있으며, 이후에 조금더 자세히 다룰 예정이다