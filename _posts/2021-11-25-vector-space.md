---
title: 2.4 Vector space
date: 2021-11-25 22:05:00 +0900
categories: [Math]
tags: [linear equation, mml-book]
use_math: true
---

- 이번장에서 알고 가야할 정의
    - Group (군)
    - Vector space
    - Vector subspace
- 한창 linear system에 대해서 소개하다가 왜 Vector space가 등장할까? 라고 생각을 해보면 이후에 Vector space에 대한 정의를 하기위해서 밑밥(?)을 다진다고 보면 될 것 같다.

## 2.4.1 Group

**Definition 2.7 (Group)** 어떤 집합 $\mathcal{G}$와 연산 $\oplus$가 다음과 같이 정의되면, $\oplus$ : $\mathcal{G}$X $\mathcal{G}$ → $\mathcal{G}$,

$G$ := ($\mathcal{G}$,$\oplus$) 은 다음을 따를 때 정의됨

1. 연산 $\oplus$ 에 대해 닫혀있음(Closure) $\forall$$x, y \in \mathcal{G} \ : \ x \oplus y \in \mathcal{G}$
2. 결합법칙이 성립함(Associativity) → $\forall x, y, z \in \mathcal{G} \ : \ (x\oplus y )\oplus z = x \oplus(y \oplus z)$
3. 항등원이 존재함(neutral element) → $\exists e \in \mathcal{G} \ \forall x \in \mathcal{G} \ : \ x \oplus y = x \ \ and \ e \oplus x = x$
4. 역원이 존재함(Inverse element) → $\forall x \in \mathcal{G} \ \exists y \in \mathcal{G} \ : \ x \oplus y = e \ and \ y \oplus x = e$
    - 이때, $x$의 역원을 $x^{-1}$로 표기
5. (Abelian Group) 교환법칙이 성립하면 아벨리안 그룹임 → $\forall x, y \in \mathcal{G} \ : \ x \oplus y = y \oplus x$
    - 1~4번을 만족하고 5번 또한 만족하는 경우 $G = ($$\mathcal{G}$,$\oplus )$ is Abelian group

**Example 2.10 (Group)** 

- 그룹의 성질과 관련한 몇가지 집어 볼 사실들
    - $(Z,+)$는 그룹임
    - $(N_{0},+)$는 그룹이 아니다 ⇒ 역원이 포함되지 않기 때문에
    - $(Z,\cdot)$에 대해서 그룹이 아니다 ⇒ 항등원인 1은 포함하지만, $\pm1$을 제외하면 역원이 없음!
    - $(R,\cdot)$에 대해서 그룹이 아니다 ⇒ 0이 역원을 가지고 있지 않기 때문에!
    - $(R ,\cdot)$에서 0을 제외하면 아벨리안 그룹이다.
    - $(x_1,...,x_n) + (y_1,...,y_n) = (x_1 + y_1,...,x_n+y_n)$이 성립하면 
    $(R^n ,+), \ (Z^n ,+), \ n \in N$ 에서 아벨리안 그룹이다.
        - 이때,  $(-x_1,...,-x_n)$이 역원이고, $e = (0,...,0)$ 이 항등원임
    - $(R^{n \times n}, \cdot)$ 에 대해서는 다음을 고려해 볼 수 있음
        - Closure & Associativity 는 행렬곱의 정의에 따라 성립함
        - 항등원은 $I_n$이 존재함
        - $A \in R^{n \times n}$에 대해 $A$가 regular라면 역원이 성립하고, 이때 General linear group 이라 부름
        

**Definition 2.8 (General linear group)** 역행렬이 존재하는 $A \in R^{n \times n}$ 의 집합은 General linear group 이라하며, $GL(n, R)$로 표기함 그러나 교환법칙이 성립하지 않기에 아벨리안 그룹은 아님

**Definition 2.9 (Vector space) 벡터 스페이스 $V = (\mathcal{V}, +, \cdot)$ 는 집합 $\mathcal{V}$와 2개의 연산으로 구성됨**

$$+ : \mathcal{V} \times \mathcal{V} Rightarrow \mathcal{V} \\ \cdot : R \times \mathcal{V} Rightarrow \mathcal{V}$$

1. $**(\mathcal{V}, +)$** 은 아벨리안 그룹임
2. 분배법칙(Distributivity) 이 성립
    - $\forall \lambda \in R, x,y\in \mathcal{V} \ : \ \lambda \cdot(x + y) = \lambda \cdot x + \lambda \cdot y$
    - $\forall \lambda, \psi \in R, x \in \mathcal{V} \ : \ (\lambda + \psi)\cdot x = \lambda \cdot x + \psi \cdot x$
3. 결합법칙(Associdativity)이 성립함 : $\forall \lambda,\psi \in R, x \in \mathcal{V} \ : \ \lambda \cdot (\psi \cdot \ x) = (\lambda \psi)\cdot x$
4. 상수의 행렬곱에 대해 항등원이 존재함 : $\forall x \in \mathcal{V}, \ : \ 1\cdot x = x$

- 여기에서 행렬의 elementwise 곱연산은 고려하지 않으며, 행렬곱은 외적(outer product) & 내적(inner/scalar/dot product)으로 구분함

**Definition 2.10 (Vector Subspace)**  **$V = (\mathcal{V}, +, \cdot)$ 이 벡터 스페이스이면서 $\mathcal{U} \subseteq \mathcal{V}, \mathcal{U} \ne \emptyset$ 을 만족하면,  $U = (\mathcal{U}, +, \cdot)$ 는 $\mathcal{V}$의 벡터 서브스페이스(or linear subspace) 라 정의함**

- $U$ 또한 벡터 스페이스의 vector subspace 이므로, 벡터스페이스의 정의를 만족함

오늘은 간단하게 벡터 공간에 대해 정의를 집어보았다. 이후에 이어질 개념을 이해하기 위한 밑밥정도이니 간단하게 음미하고 지나가면 될 것 같다.