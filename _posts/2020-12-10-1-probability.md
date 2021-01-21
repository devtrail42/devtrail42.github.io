---
published: true
layout: single
title:  "Basic! 인공지능 수학 part 7: 확률"
header:
  overlay_image: /images/unsplash-image-1.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  actions:
    - label: "Learn more"
      url: "https://en.wikipedia.org/wiki/Bayesian_inference"
      
categories: [AI math_basic]
tags: [programmers, AI]
comments: true
---

Basic! 인공지능 수학 일곱 번째 시간으로 확률에 대해 알아보도록 합시다. 

## 확률

### 확률(probabiliy)

* 상대 도수에 의한 확률의 정의 :  
똑같은 실험을 무수히 많이 반복할 때 어떤 일이 일어나는 비율

* 고전적 정의 
    + 표본 공간 (sample space)
        모든 가능한 실험결과들의 집합
    + 사건 
        관심있는 실험결과들의 집합
    + 어떤 사건이 일어날 확률
        표본공간의 모든 원소가 일어날 확률이 같은 경우 :  
        사건의 원소의 수 / 표본공간의 원소의 수  

사건을 $A$라 하면 A라는 사건이 일어날 확률은 $P(A)$로 표시합니다.

확률은 0에서 1사이의 값을 가집니다. 

### 조합 (combination)

원소의 수를 구하기위해 주로 조합을 사용합니다.  
어떤 집합에서 순서에 상관없이 뽑은 원소의 집합을 의미합니다. 

$$ _nC_r = {n \choose r} = \frac{n!}{r!(n - r)!}$$

### 덧셈 법칙 (Addition Law)

$$ P(A \cup B) = P(A) + p(B) - P(A \cap B)$$

### 서로 배반 (Mutually Exclusive)

절대로 동시에 일어날 수 없는 사건 둘을 서로 배반이라 합니다. 

$$P(A \cap B) = 0$$  

$$P(A \cup B) = P(A) + p(B)$$

### 조건부 확률 (conditional probability)

어떤 사건 A가 일어났을 때, 다른 사건 B가 일어날 확률을 의미합니다.  

$$ P(B \mid A) = \frac{P(A \cap B)}{P(A)}$$

### 곱셈 법칙 

조건부 확률을 구할 수 있으면 교집합도 구할 수 있습니다.  

$$ P(A \cap B) = P(B \mid A)P(A)$$

### 서로 독립 

$P(B \mid A) = P(B)$인 경우 사건 A와 B는 서로 독립 입니다.  

$$ P(A \cap B) = P(B \mid A)P(A) = p(B)P(A) = P(A)P(B) $$

### 여사건 

특성 사건이 일어나지 않을 사건을 의미합니다. 

$A^c$

어떤 사건과 그 여사건은 서로 배반입니다. 따라서 둘 중 하나는 반드시 일어납니다. 

### 분할 법칙 

$$ P(B) = P[(A \cap B) \cup (A^c \cap B)] = P(A \cap B) + P(A^c \cap B) $$  

$$ = P(B \mid A)P(A) + P(B \mid A^c)P(A^c)$$

### 베이즈 정리 

베이즈 정리를 활용하면 A의 관련정보를 가지고 B의 관련정보를 알 수 있습니다.  
일종의 추론을 가능하게 해줍니다.

$$ P(A \mid B) = \frac{P(A \cap B)}{P(B)} $$  

$$ = \frac{P(B \mid A)P(A)}{P(B \mid A)P(A) + P(B \mid A^c)P(A^c)} $$






