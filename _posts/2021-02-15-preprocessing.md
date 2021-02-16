---
published: true
layout: single
title:  "[Programmers][NLP] 텍스트 전처리"
header:
  overlay_image: /images/unsplash-image-1.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  actions:
    - label: "Learn more"
      url: ""
      
categories: [NLP]
tags: [programmers, NLP]
comments: true

toc: true
toc_label: "Contents"
toc_sticky: true
# toc_icon: "heart"  # corresponding Font Awesome icon name (without fa prefix)
---


NLP: 텍스트 전처리

&nbsp;

&nbsp;

# NLP 참고 수업자료

Speech and Language Processing (https://web.stanford.edu/~jurafsky/slp3/)

# 텍스트 전처리

- 문장부호의 의미 고려
    문장을 구분하는 기호 ,(쉼표)의 의미 등을 고려합니다.  
- 구어체 고려
    깨어진 단어들 혹은 말하다 중단된 단어, 의태어 의성어, 의미없는 filled pauses 등  
- 표제어 및 단어형태 고려
    cat과 cats의 경우 형태가 다르지만 동일한 뜻을 공유합니다. 
- 토큰들과 말뭉치 
    대용량의 문서들의 집합을 말뭉치라 부르며 어떻게 생성됐는지에 따라 특성이 달라집니다.  
    언어, 방언, 장르, 인구통계적 속성 등에 의한 영향을 받습니다.
    다양한 말뭉치에 적용될 수 있는 NLP알고리즘이 바람직하다 볼수 있습니다. 
- 텍스트 정규화
    모든 자연어 처리는 텍스트 정규화를 필요로 합니다. 
    - 토큰화(tokenizing)
    - 단어정규화(normalizing word formats)
    - 문장분절화(segmenting sentences)

# 토큰화 

## Unix 명령으로 토큰화하기 

별다른 고려를 하지 않으면 아주 간단하게 토큰화를 할 수 있습니다.

- 텍스트 파일 안에 있는 단어 토큰화하기 
```
tr -sc 'A-Za-z' '\n' < text.txt
```
토큰들이 열거되어 출력됩니다.


- 빈도수로 정렬
```
tr -sc 'A-Za-z' '\n' < text.txt | sort | uniq -c | sort -n -r
```
단어뿐만 아니라 나타난 횟수도 같이 출력되어 보여집니다. 등장한 갯수 순으로 내림차순 정렬됩니다. 

- 소문자로 변환 후 정렬
```
tr 'A-Z''a-z' < text.txt | tr -sc 'a-z' '\n' | sort | uniq -c | sort -n -r
```

### 문제점 

문장부호들을 항상 무시할 수는 없습니다.  
- 문장부호가 포함된 단어들 (PH.D. ,m.p.h. 등)
- 화폐단위, 날짜, URLs, hashtags, 이메일 주소 등 
- 문장부호가 단어의 의미를 명확하게 하는 경우는 제외시키지 않는 것이 좋습니다. 

접어(clitics)등이 존재합니다.  
we're -> we are

여러 개의 단어가 붙어야 의미가 있는 경우도 있습니다.  
New York, Peace!

## 한국어
- 한국어의 경우 토큰화가 복잡합니다.
- 띄어쓰기가 잘 지켜지지 않고 띄어쓰기가 잘 되어 있더라도 한 어절은 하나 이상의 의미를 내포할 수 있습니다.
- 형태소의 종류가 다양합니다. 
- 단어보다 작은 단위로 토큰화를 해야합니다. 

# Subword Tokenization
- 학습데이터에서 보지못했던 새로운 단어가 나타날 경우 
    - 학습데이터: low, new, newer
    - 테스트데이터: lower
    - -er, -est등과 같은 형태소를 분리하면 학습데이터를 이용할 수 있습니다. 
- Subword tokenization algorithm  
    - Byte-Pair Encoding(BPE)
    - WordPiece
    - Unigram language modeling
- 공통적인 두 가지 구성요소 
    - Token learner: 말뭉치에서 단어(토큰 집합)를 만들어냄
    - Token segmenter: 새로운 문장을 토큰화함

## Byte Pair Encoding


### Token Learner
단어들을 단일 문자들의 집합으로 초기화 시킵니다.  

- 다음 과정을 반복합니다. 
    - 말뭉치에서 연속적으로 가장 많이 발생하는 두 개의 기호들(vocab 내의 원소들)을 찾습니다.  
    - 두 기호들을 병합하고 새로운 기호로 vocab에 추가합니다. 
    - 말뭉치에서 그 두 기호들을 병합된 기호로 모두 교체합니다. 

위 과정을 k번의 병합이 일어날 때까지 반복합니다.  

![](/images/2021-02/nlp_pre/1.png)  

기호병합은 단어 안에서만 이뤄져야합니다. 이것을 위해 단어끝을 나타내는 특수기호 '_'를 단어 끝에 추가합니다. 그리고 각 단어를 문자단위로 쪼갭니다.  

- 예제 

예제로 아래와 같은 문자열이 있다고 해봅시다.

![](/images/2021-02/nlp_pre/2.png)  
 

문자열을 단어로 나타낸 뒤, 단어에서 사용되는 연속적인 두개의 기호를 count하고 가장 많이 등장한 기호쌍을 하나로 병합합니다. 

![](/images/2021-02/nlp_pre/3.png)  
 

하나로 병합된 기호들은 하나의 기호로 취급됩니다. 새롭게 만들어진 기호는 단어목록에 추가되게 됩니다. 

이를 k 만큼 반복하면 단어목록이 점점 늘어나게 됩니다. 

![](/images/2021-02/nlp_pre/4.png)  

### Token segmenter
- 새로운 단어가 등장한 경우 greedy한 방법을 적용합니다. 
- 새로운 문장에서 연속되는 토큰이 존재하는지 살펴보고 이전 병합 순서를 따라서 적용시킵니다.
- 예를 들어 새로운 단어 lower_는 low 와 er_로 토큰화가 이뤄집니다. 
- 드문 단어는 subword 토큰들로 분할됩니다.  

## Wordpiece

기호들의 쌍을 찾을 때 빈도수 대신에 likelihood를 최대화시키는 쌍을 찾습니다. likelihood 란 새로운 candidate가 나타날 확률을 뜻 합니다.  
Bert 모델에서 주로 사용되는 토큰화 방식입니다.  

![](/images/2021-02/nlp_pre/5.png)  

## Unigram 

- 확률모델(언어모델)을 사용합니다.  
- 학습데이터 내의 문장을 관측(observed) 확률변수로 정의합니다.  
- Tokenization을 잠재(latent) 확률변수로 정의합니다. (연속적인 변수)  
- 데이터의 주변 우도(marginal likelihood)를 최대화시키는 tokenization을 구합니다. 
    - EM(expectation maximization)을 사용합니다. 
    - Maximiztion step에서 Viterbi 알고리즘을 사용합니다. (비교: wordpiece는 greedy하게 likelihood를 향상시킵니다.)

# 단어 정규화 

tokeniztion 이후 단어들을 정규화된 형식으로 표현하기도 합니다.  
- U.S.A. or USA or US
- uh or uhhuh or uh-huh
- am, is be, are

검색엔진에서 문서들을 추출할 때 유용하게 사용되기도 합니다.  

문자들을 소문자하는 것도 정보검색 혹은 음성인식에서 유용하게 사용될 수 있습니다. 그러나 감성분석 등의 문서분류문제에서는 오히려 대소문자 구분이 유용할 수 있습니다. (국가 US, 대명사 us)

Lemmatization
- 어근을 사용해서 통일 시켜줍니다. 
    - car, cars, cars' => car

## 근본적인 이유

단어들 사이의 유사성을 이해해야하기 때문에 정규화를 사용합니다. 
단어 정규화 작업을 같은 의미를 가진 여러 형태의 단어들을 하나의 단어로 대응하는 것으로 이해할 수 있습니다.  
이러한 작업을 진행하면서 vocab의 차원 수가 너무 커지면 관련성을 찾는데 너무 많은 자원이 소모됩니다.  
따라서 저차원 밀집 벡터로 단어를 임베딩하는 방향으로 기술이 발전되기 시작했습니다. 단어 정규화 작업이 아니어도 벡터로 정규화를 진행할 수 있습니다. 
