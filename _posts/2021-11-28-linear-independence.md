---
title: 2.5 Linear independence
date: 2021-11-28 21:50:00 +0900
categories: [Math]
tags: [linear equation, mml-book]
use_math: true
---

- 이번장에서 알고 가야할 정의
    - Linear Combination
    - Linear Independent vector
    - Linear dependent vector

**Definition 2.11 (Linear Combination) 벡터 공간 $V$와 유한한 벡터 $x_1,...,x_k \in V$ 이면, 모든 $v \in V$이 다음을 만족하면 벡터 $x_1,...,x_k \in V$와 $\lambda_1,...,\lambda_k \in R$의 선형결합(Linear combination)이라 정의한다**

- 0-벡터는 항상 선형결합이다.

$$v=\lambda_1x_1 + \cdot\cdot\cdot+\lambda_kx_k=\sum^k_{i=1}\lambda_ix_i \in V$$

**Definition  2.12 (Linear (In)dependence)벡터 공간 $V$와 유한한 벡터 $x_1,...,x_k \in V$, $k \in N$ 이 존재하면,**

1. $\lambda_i \ne 0$이 아닌 해가 존재하면 선형 종속(linear dependence)이라 정의함
2. $\lambda_i = 0$ 만 해를 만족한다면 선형 독립(linear independence)이라 정의함

- 모든 컬럼 벡터가 pivot 이면 선형 독립의 필요충분(if and if only) 조건임 → 모든 컬럼이 pivot 인 것을 보이면 선형 독립(linear independence)을 보일 수 있음
- {$x_1,...,x_m$}이 선형 독립(linear independence)이면, 컬럼 벡터 {$\lambda_1,...,\lambda_m$} 역시 선형 독립임
- {$x_1,...,x_m$}이 선형 독립(linear independence)이더라도, 컬럼 벡터 {$\lambda_1,...,\lambda_n$} 은 선형 종속 일 수 있음
    
    ($n < m$)
    

- 이것이 왜 중요한가? → 선형 시스템의 해를 구하는 것은 결국 주어진 벡터들의 선형결합을 찾는 것과 같음
- 선형 독립의 부분집합은 선형독립이다.