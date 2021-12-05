---
title: Latent Semantic Analysis (LSA, 잠재의미 분석)
date: 2021-12-05 21:57:00 +0900
categories: [etc]
tags: [etc]
use_math: true
---
# 잠재 의미 분석(LSA, LSI)

- `BoW에 기반한 단어 - 문서 행렬` , `TF-IDF`는 기본적으로 단어의 빈도 수를 수치화하여 분석하는 방법론이었기 때문에 → **단어의 의미를 고려하지 못한다 라는 단점**이 있었음
- 이를 보완하기 위한 대안으로 `단어 - 문서 행렬`의 잠재된(latent) 의미를 이끌어 내는 방법으로 잠재 의미 분석(LSA)을 활용함 (LSI 라고도 함)
- `m x n (단어,문서) 행렬` → `m x t, t x n 의 (단어, 토픽), (토픽, 문서) 행렬`로 분해됨
- How - to : 토픽모델링을 위해 고안된 아이디어라기 보기 보다는, SVD(개인적으로는, 활용관점에서 선대의 꽃이라고 생각)를 토픽 모델링이라는 분야에 활용한 사례라고 할 수 있음

## 1. 특이값 분해(Singular Value Decomposition, SVD)

> 특이값 분해를 들어가기전에 EVD(Eigen Value Decomposition)을 잠깐 짚고 
넘어가겠음
> 
- 몇몇 `정방행렬(n x n)`은 대각화 분해 (Eigen Value Decomposition)가 가능함
- n x n 행렬 A가 대각화 분해가 가능하려면, 행렬 A가 일차독립인 고유벡터(Linearly independent)를 가져야 함 → 각 벡터는 공간의 기저(Basis) 역활을 함
- 행렬 A를 대각화 분해하면, Det(A), A의 거듭제곱, 역행렬, 대각합(TraceA) 등을 손쉽게 계산할 수 있는 장점이 있음
- 정방행렬 중 H를 다음처럼 대각원소를 중심으로 원소값들이 대칭되는 행렬이라고 가정해보자

$$H^T = H$$

- **실수 원소를 갖는 대칭행렬(Symmetric matrix)**은 1. 항상 고유값 대각화(EVD)가 가능하며 2. 직교행렬로 대각화가 가능함 → 이 성질로 인해 SVD, 주성분분석(PCA) 등이 가능해짐

$$H = PDP^{-1}$$

### 1) 일반 m x n 행렬

- 일반 행렬 m x n을 대각화 분해하기 위해선 어떤 과정이 필요할까?
- m x n 행렬 A가 있다고 가정해보자, 그러면 아래와 같이 되고

$$A^TA \neq AA^T$$

- 하나는 m x m 정방행렬, 또 하나는 n x n 정방행렬이 된다
- 가장 중요한점은 저 **두 행렬이 모두 대칭행렬(Symmetric matrix)**라는 점이다
- 이제 **두 정방행렬은 각각 EDV가 가능한 행렬**이 되었으며, 항상 **직교행렬로 대각화**가 될 것임

### 2) 특이값 분해

- 특이값 분해(SVD)는 A가 m x n 행렬일 때, 다음과 같이 3개의 행렬 곱으로 분해하는 것을 말함
$$A=U \sum V^T$$


- 이 3개의 행렬은 다음과 같은 조건을 만족함
- U = mxm 직교행렬
- V = nxn 직교행렬
- $\sum$ = mxn 직사각 대각행렬
<a href="https://ibb.co/4Pgp1KH"><img src="https://i.ibb.co/KFzWKbJ/Untitled-4.png" alt="Untitled-4" border="0"></a>

- 가운데 직사각 대각행렬은 각 Component
<a href="https://ibb.co/TWd6kwR"><img src="https://i.ibb.co/2ZHxyFP/Untitled-5.png" alt="Untitled-5" border="0"></a>

(참고 SVD의 기하학적 의미 : 위키피디아 [https://en.wikipedia.org/wiki/Singular_value_decomposition](https://en.wikipedia.org/wiki/Singular_value_decomposition))

### 3) 특이값 분해의 응용

1. Thin SVD
<a href="https://ibb.co/2W73Tkk"><img src="https://i.ibb.co/pbvjSLL/Untitled-6.png" alt="Untitled-6" border="0"></a>

- SVD 행렬에서 대각행렬의 아래부분과 U에서 아래부분에 해당하는 부분을 제거
- A를 완벽하게 원복할 수 있음
- sparce한 부분을 모두 제거하는 것을 Compact SVD라고도 함

2. truncated SVD
<a href="https://ibb.co/ZGvdDVJ"><img src="https://i.ibb.co/sbcjY2P/Untitled-7.png" alt="Untitled-7" border="0"></a>

- A의 SVD 대각행렬 부분에서 상위값 t개만 골라낸 형태임
- A를 원복할 수 없지만, 데이터 정보를 압축했음에도 A를 근사할 수 있게되어 계산비용이 절감
- 반대로 생각해보면, 중요하지 않는 부분은 제외한다고 볼 수 있음
- **LSA에서 주로 활용하는 방식**

## 2. Latent Semantic Indexing (LSI)

- LSI는 k 차원의 벡터로 단어와 문서를 표현함
    - Topic에 관련한 정보를 보존하는 차원 압축 방법임
    - [https://ratsgo.github.io/from frequency to semantics/2017/04/06/pcasvdlsa/](https://ratsgo.github.io/from%20frequency%20to%20semantics/2017/04/06/pcasvdlsa/)
    
    <a href="https://ibb.co/qg5DLCR"><img src="https://i.ibb.co/2MNF0Sj/Untitled-8.png" alt="Untitled-8" border="0"></a>

    
    - Topic Importance를 통해 토픽의 중요도 파악이 가능
        - 또한 두 Components term 1,2의 유사성이 보존됨

    <a href="https://ibb.co/QvbTT6v"><img src="https://i.ibb.co/HTFbbrT/Untitled-9.png" alt="Untitled-9" border="0"></a>

    
    - 두 components 텀에 doc 1,2 / doc 3, 4의 유사성이 보존됨
    
    <a href="https://ibb.co/ck5FJBJ"><img src="https://i.ibb.co/VJySV1V/Untitled-10.png" alt="Untitled-10" border="0"></a>

- SVD에 대한 수학적인 리뷰는 이후에도 하겠지만, 활용사례를 정리하면서 늘상 헷갈렸던 부분을 다시한번 정리해보았고, 이제는 구현이 매우 간단하지만 아직도 비즈니스에서 효과적으로 사용할 수 있는 사례 중 하나라고 생각하며 정리글을 마무리하겠음