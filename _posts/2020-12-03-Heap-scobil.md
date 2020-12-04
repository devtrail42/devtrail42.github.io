---
published: true
layout: single
title:  "[data structure][python] 힙을 이용해 효율성 챙기기"
header:
  overlay_image: /images/unsplash-image-1.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  actions:
    - label: "Learn more"
      url: "https://google.com"
      
categories: [study]
tags: [algorithm,python,programmers,heap]
comments: true
---

## heap 관련 문제를 살펴보고 왜 힙이 더 효율적인지 생각해보자 


> ### 목표
>   * 문제를 풀며 힙을 사용했을 때의 효율성을 생각해보기.
>   * 일반적인 비교정렬 알고리즘의 시간복잡도를 알아보기.


## 힙 정렬(Heap Sort) 

* 힙이란?
    완전 이진 트리의 일종으로 부모의 데이터가 자식의 데이터보다 항상 크거나(혹은 작거나) 같은 이진트리를 말한다.  
    + 힙에서의 삽입 연산
        새로운 노드를 힙의 마지막에 삽입하고, 부모들과 비교해가며 위치를 바꾸는 과정을 통해 힙의 자료구조를 유지한다. 
    + 힙에서의 삭제 연산
        루트 노드를 삭제하고, 루트 노드에 힙의 마지막 노드를 가져온다. 새롭게 바뀐 루트 노드는 자식들과 비교해가며 자식 중 가장 큰 자식과 바꾸는 과정을 반복해 힙의 자료구조를 유지한다. 

* 힙 정렬이란?
    최소 힙 트리(내림 차순 정렬) 또는 최대 힙 트리(오름 차순 정렬)를 구성하고, root에서 부터 아이템을 하나씩 꺼내는 방식의 정렬법을 의미한다. 

## 힙 정렬의 시간 복잡도 

힙은 앞서 설명했듯 완전 이진 트리의 구조를 갖는다. 트리의 전체 노드 수를 n이라 했을 때, 높이는 log n이 된다. 삽입 연산과 삭제 연산 모두 부모와 자식을 비교해가는 과정이기 때문에 최대 <code>O(log n)</code>의 시간 복잡도를 지닌다.  
힙 정렬의 경우 이렇게 heap을 구성한 후, 루트 노드에서 하나씩 삭제하는 연산을 진행하면 되므로 최대 <code>O(n log n )</code>의 시간 복잡도를 갖는다.  
정렬 알고리즘은 <code>O(n log n)</code>보다 더 빠르게 정렬할 수 없다는 것이 증명됐으므로, 정렬을 한 후 비교해야하는 문제일 수록 힙 자료구조가 힘을 발휘한 다는 것을 알 수 있다. 

## 문제 풀기 전 준비
본격적으로 문제를 풀기 전에 python에서는 힙을 어떻게 구성하는지 알아보자.

### python에서 heap을 적용하는 방법

~~~py
import heapq

L = [...]
heapq.heapify(L)  # 리스트 L로부터 min heap 구성
m = heapq.heappop(L)   # min heap L 에서 최소값 삭제
heapq.heappush(L, x)  # min heap L 에 원소 x 삽입

~~~

### 최대힙 만들기 

~~~py
import heapq

nums = [4, 1, 7, 3, 8, 5]
heap = []

for num in nums:
    heapq.heappush(heap, (-num, num))  # (우선 순위, 값)

while heap:
    print(heapq.heappop(heap)[1])  # index 1
~~~


## 힙을 사용하면 더 효율적으로 풀 수 있는 문제들 
다음은 힙을 이용하면 더 효율적으로 풀 수 있는 문제들이다. 리스트처럼 모든 원소를 이동시키지 않고, 항상 정렬된 상태를 유지시킬 수 있는 힙의 특성에 대해 고려하면서 문제를 풀어보도록 하자.  

### lv2_더 맵게

### 문제 설명

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

~~~
섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)
~~~

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

### 제한 사항

scoville의 길이는 1 이상 1,000,000 이하입니다.
K는 0 이상 1,000,000,000 이하입니다.
scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

### 입출력 예

|scoville|K|return|
|---|---|---|
|[1, 2, 3, 9, 10, 12]|	7	|2|

### 문제 풀이 
~~~py
import heapq

def solution(scovile, K):
    answer = 0
    heapq.heapify(scoville)
    while True:
        min1 = heapq.heappop(scoville)
        if min1 >= K:
            break
        elif len(scoville) == 0:
            answer = -1
            break
        min2 = heapq.heappop(scoville)
        new_scoville = min1 + 2 * min2
        heapq.heappush(scovile, new_scoville)
        answer += 1
    return answer
~~~


### lv2_배상 비용 최소화 사본

### 문제 설명

OO 조선소에서는 태풍으로 인한 작업지연으로 수주한 선박들을 기한 내에 완성하지 못할 것이 예상됩니다. 기한 내에 완성하지 못하면 손해 배상을 해야 하므로 남은 일의 작업량을 숫자로 매기고 배상비용을 최소화하는 방법을 찾으려고 합니다.
배상 비용은 각 선박의 완성까지 남은 일의 작업량을 제곱하여 모두 더한 값이 됩니다.

조선소에서는 1시간 동안 남은 일 중 하나를 골라 작업량 1만큼 처리할 수 있습니다. 조선소에서 작업할 수 있는 N 시간과 각 일에 대한 작업량이 담긴 배열(works)이 있을 때 배상 비용을 최소화한 결과를 반환하는 함수를 만들어 주세요. 예를 들어, N=4일 때, 선박별로 남은 일의 작업량이 works = [4, 3, 3]이라면 배상 비용을 최소화하기 위해 일을 한 결과는 [2, 2, 2]가 되고 배상 비용은 22 + 22 + 22 = 12가 되어 12를 반환해 줍니다.

### 제한사항

일할 수 있는 시간 N : 1,000,000 이하의 자연수
배열 works의 크기 : 1,000 이하의 자연수
각 일에 대한 작업량 : 1,000 이하의 자연수

### 입출력 예

|N|	works|	result|
|---|---|---|
|4|	[4,3,3]|	12|
|2|	[3,3,3]|	17|

### 문제 풀이 
~~~py
import heapq

def solution(no, works):
    result = 0
    max_heap = []
    for work in works:
        heapq.heappush(max_heap, (-work, work))
    # heap's item = heapq.heappop(max_heap)[1]
    while True:
        if no == 0:
            break
        target = heapq.heappop(max_heap)[1]
        if target > 0:
            target -= 1
        heapq.heappush(max_heap, (-target, target))
        no -= 1
    while True:
        if len(max_heap) == 0:
            break
        target = heapq.heappop(max_heap)[1]
        result = result + target**2
    return result
~~~


## 비교 정렬 알고리즘의 시간 복잡도는 아무리 좋아도 O(n log n)이다.

일반적인 상황에서 정렬 알고리즘의 시간 복잡도는 아무리 좋아도 <code>O(n log n)</code>이다. 

### 증명하기 

크기가 <code>n</code>인 데이터가 주어졌을 때, 데이터가 나열될 수 있는 모든 경우의 수는 순열로 계산했을 때 <code>n!</code>이 된다. 이 중 올바르게 정렬된 경우는 하나이며, <code>n</code>개의 데이터 중 두 개를 비교할 때마다 그와 같지 않은 나머지 경우의 수 절반을 배제할 수 있다. 다음의 예시로 이해해보자.  

가령 세 개의 데이터를 내림차순으로 정렬하고자 할 때,  
<code>(x1, x2, x3)</code>  
가 주어져 있다면 나올 수 있는 경우의 수는  
<code>3! = 6</code>  
으로, 다음과 같이 여섯가지 경우의 수가 있다.  

<code>(x1, x2, x3)</code>  
<code>(x1, x3, x2)</code>  
<code>(x2, x1, x3)</code>  
<code>(x2, x3, x1)</code>  
<code>(x3, x1, x2)</code>  
<code>(x3, x2, x1)</code>  
 
만약 x1 > x2 라 가정하면 위의 경우의 수 중 절반이 제거 되며 다음의 경우의 수만 남는다.  

<code>(x2, x1, x3)</code>    
<code>(x2, x3, x1)</code>  
<code>(x3, x2, x1)</code>  

이런식으로 비교 정렬 알고리즘은 비교를 통해 경우의 수를 절반씩 줄일 수 있다. 최악의 경우 절반으로 나눌 수 있는 최댓값인 <code>⌈log2 n!⌉ ≈ logn!</code>번을 계산해야한다.

여기서 스탈링 근사를 이용하면 n! ≈ e^(n log n − n) √(2πn) 이므로 다음과 같은 수식들을 유도해낼 수 있다. 
> log n! = log e^(n log n − n) √(2πn)
>   = n log n − n + log√(2πn)
>   = n log n − n + 1/2 log n + 1/2 log 2π
>   = O(n log n)
