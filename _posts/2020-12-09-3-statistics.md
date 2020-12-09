---
published: true
layout: single
title:  "Basic! 인공지능 수학 part 6: 기초 통계학"
header:
  overlay_image: /images/unsplash-image-1.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  actions:
    - label: "Learn more"
      url: "https://en.wikipedia.org/wiki/Gaussian_elimination"
      
categories: [AI math_basic]
tags: [programmers, AI, math, statistics]
comments: true
---

Basic! 인공지능 수학 여섯 번째 시간으로 통계의 기초에 대해 알아보도록 합시다. 

## 개념 정의 

![](/images/2020-12/statistic/1.png)

![](/images/2020-12/statistic/2.png)

### 도수 (Frequency)

데이터 수집의 가장 기초적인 방법입니다. 

![](/images/2020-12/statistic/3.png)

![](/images/2020-12/statistic/4.png)

### 상대도수

![](/images/2020-12/statistic/5.png)

## 파이썬 이용해보기 

파이썬의 대표적인 통계학 모듈들을 설치해 실습을 해봅시다. 

### 평균 

파이썬의 기본 패키지인 statistics를 사용하면 평균을 구할 수 있습니다. 

![](/images/2020-12/statistic/6.png)

모집단 전체 자료를 사용할 경우 모평균이라 부르고,  
표본만 사용해 평균을 낼 경우 표본평균이라 부릅니다.

### 중앙값 (Median)

![](/images/2020-12/statistic/7.png)

![](/images/2020-12/statistic/8.png)

![](/images/2020-12/statistic/9.png)

### 분산 (Variance)

데이터들이 평균에서 얼마나 흩어져 있는지를 나타냅니다. 

![](/images/2020-12/statistic/10.png)

다음은 표본분산을 구하는 코드입니다. 

![](/images/2020-12/statistic/11.png)

### 표준편차 (Standard Deviation)

![](/images/2020-12/statistic/12.png)

다음은 표준편차를 구하는 코드입니다. 

![](/images/2020-12/statistic/13.png)

numpy를 이용해서 표본표준편차도 구할 수 있습니다.  
ddof를 이용해서 표본의 자유도를 조절할 수 있습니다.  

![](/images/2020-12/statistic/14.png)

### 범위

![](/images/2020-12/statistic/15.png)

### 사분위수 (Quartile)

범위만 가지고 표현하면 양 극단에 이상점이 있을 경우  
자료의 대략적인 모습이 왜곡될 수 있습니다.  
따라서 사분위수를 사용해 범위를 표현할 수 있습니다.  

![](/images/2020-12/statistic/16.png)

### z-score

![](/images/2020-12/statistic/17.png)

주어진 데이터가 모집단인 경우

![](/images/2020-12/statistic/18.png)

주어진 데이터가 표본인 경우 

![](/images/2020-12/statistic/19.png)
