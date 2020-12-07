---
published: true
layout: single
title:  "Basic! 인공지능 수학 part 1: 선형대수"
header:
  overlay_image: /images/unsplash-image-1.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  actions:
    - label: "Learn more"
      url: "https://en.wikipedia.org/wiki/Gaussian_elimination"
      
categories: [AI math_basic]
tags: [programmers, AI, math, linear]
comments: true
---

Basic! 인공지능 수학의 첫번째 시간으로 선형시스템에 대해 알아봅시다.  

> 본 글은 프로그래머스의 인공지능 데브코스를 
> 바탕으로 쓰여진 글입니다.

# 선형시스템
가우스 소거법 -> 이걸 기반으로 여러가지 선형 시스템의 변수들을 처리한다. 
식이 수천개가 넘어간다면? 
어떤 종류의 선형 시스템이던 굉장히 정형적으로 표현하고 풀어낼 수 있게 만드는 것이 목표!
아무리 복잡한 식이라도 이런 꼴로 표현해서 Ax = b , 초중학교때 방식으로 풀어보고싶다.
A^(-1) Ax = A^(-1)b
x = A^(-1) b

선형 시스템의 구성요소 

1. 선형 방정식

linear : 선의 형태 ?-> 평면이나 왜곡되지 않은 공간(올곧은 공간)을 의미한다. 
ex) 3x + y + z = 4 -> 평면 (올곧게 그려진다) -> 선형 방정식이다. 

unknown(variable) : 우리가 알아내려는 미지수들을 의미한다. ex) x, y, z ...

3x + y + z = 4
x - 2y - z = 1
x + y + z = 2

위와 같은 시스템은 3개의 linear 시스템과 3개의 unknown으로 구성되어 있으므로
3(linear) X 3(unknown) linear system 이라고 한다. 

비선형 방정식 -> sinx , y^3, xy 등 곡선의 형태는 승수가 1이 아닌 변수들로 구성되어 있다. 

2. 선형시스템의 표현: Ax = b (연립일차방정식의 대수적 표현)

Ax = b로 표현하기
    1. 선형시스템의 미지수들을 모아 column vector(열벡터) x로 표현한다. 
    2. 선형시스템의 linear equation(선형방정식)에 대해 다음을 수행한다. 
        - coefficients(계수)를 모아 A의 row vector(행벡터)로 표현한다. 
        - constant(상수)를 모아 b에 표현한다. 
    
3  1  2 | x     4
1 -2 -1 | y  =  1
1  1  1 | z     2
 

3. 선형시스템의 해 
선형시스템의 해의 종류는 3가지가 있다. 
    * unique solution : 3x = 6
    * no solution : 0x = 6
    * infinitely many solution : 0x = 0

* ~a = 0이면 특이한 경우가 나온다?~ 
    * ax = b 의 해가 바로 나오지 않는 이유
        * a의 역수(inverse)가 존재하지 않기 때문-> 이 경우에 a가 특이(singular)하다 이야기 한다. 

* 해가 존재하면 선형시스템이 consistent 하다고 한다. 
* 해가 없으면 선형시스템이 inconsistent 하다고 한다.  


## 가우스 소거법

가우스 소거법은 m X n 선형시스템의 해를 구하는 가장 대표적인 방법이다. 
가우스 소거법은 다음의 두 단계로 수행된다. 

1. Forward elimination(전방소거법): 주어진 선형시스템을 아래로 갈수록 더 단순한 형태의 선향방정식을 가지도록 변형한다.
2. back-substition(후방대입법): 아래서부터 위로 미지수를 실제값으로 대체한다. 

전방소거법은 아래로 갈수록 더 단순해지는 식을 만드는걸 목표로한다. 

### 소거법에 쓰이는 elementary row operation (EROs, 기본행연산)

### Forward Elimination의 가치 
Gauss elimination에서 forward elimination의 가치는 다음과 같다.
    * 주어진 선형시스템을 가장 풀기 쉬운 꼴로 변형해 준다.
    * 주어진 선형시스템의 rank(랭크)를 알려준다. -> rank : 의미있는 수식의 갯수 
    * 선형시스템이 해가 있는지 없는지 알려준다. (consistent vs inconsistent)

## numpy에서 제공해주는 가우스 소거법 

프로그래밍 실습


