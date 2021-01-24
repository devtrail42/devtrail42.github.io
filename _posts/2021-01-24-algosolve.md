---
published: true
layout: single
title:  "[programmers][python] lv3_야근 지수"
header:
  overlay_image: /images/unsplash-image-1.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  actions:
    - label: "Learn more"
      url: "https://www.daleseo.com/python-heapq/"
      
categories: [study]
tags: [algorithm,programmers]
comments: true

toc: true
toc_label: "Contents"
toc_sticky: true
---

가장 급한일을 알아낼때 사용하는 자료구조는 무엇일까요? 


&nbsp;

&nbsp;

# 문제

## 문제 설명

회사원 Demi는 가끔은 야근을 하는데요, 야근을 하면 야근 피로도가 쌓입니다.  
야근 피로도는 야근을 시작한 시점에서 남은 일의 작업량을 제곱하여 더한 값입니다.  
Demi는 N시간 동안 야근 피로도를 최소화하도록 일할 겁니다.  

Demi가 1시간 동안 작업량 1만큼을 처리할 수 있다고 할 때,  
퇴근까지 남은 N 시간과 각 일에 대한 작업량 works에 대해 야근 피로도를 최소화한 값을 리턴하는 함수 solution을 완성해주세요.

&nbsp;

## 제한 사항

works는 길이 1 이상, 20,000 이하인 배열입니다.
works의 원소는 50000 이하인 자연수입니다.
n은 1,000,000 이하인 자연수입니다.

&nbsp;

## 입출력 예

| works | n | result |
|---|---|---|
| [4, 3, 3] | 4 | 12 |
| [2, 1, 2] | 1 | 6 |
| [1, 1] | 3 | 0 |

&nbsp;

## 입출력 예 설명

입출력 예 #1
n=4 일 때, 남은 일의 작업량이 [4, 3, 3] 이라면 야근 지수를 최소화하기 위해 4시간동안 일을 한 결과는 [2, 2, 2]입니다.  
이 때 야근 지수는 22 + 22 + 22 = 12 입니다.

입출력 예 #2
n=1일 때, 남은 일의 작업량이 [2,1,2]라면 야근 지수를 최소화하기 위해 1시간동안 일을 한 결과는 [1,1,2]입니다.  
야근지수는 12 + 12 + 22 = 6입니다.

&nbsp;

&nbsp;


# 문제풀이 

이번 문제의 핵심은 작업량을 최소화하는 것 입니다.  

작업량을 최소화하려면 무엇보다 가장 많이남은 업무를 먼저 줄이는 것이 관건입니다.  
가장 많은 업무를 파악하기 위해서는 heap 자료구조를 사용해서  
max haep을 만드는 것이 효율적입니다.  

풀이는 다음과 같습니다.  

```py
import heapq

def solution(n, works):
    heap = []
    for work in works:
        heapq.heappush(heap, (-work, work))  # (우선 순위, 값)
    for i in range(n):
        work = heapq.heappop(heap)[1]
        if work == 0:
            return 0
        else:
            work -= 1
            heapq.heappush(heap, (-work, work))
    ans_list = [x[1]**2 for x in heap] 
    answer = sum(ans_list) 
    return answer
```