---
published: true
layout: single
title:  "[Web]웹 크롤링으로 뉴스 데이터 추출하기"
header:
  overlay_image: /images/unsplash-image-1.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  actions:
    - label: "Learn more"
      url: ""
      
categories: [web]
tags: [Web Crawling, Data, EDA]
comments: true
---

웹 크롤링으로 데이터 수집하기 

오늘은 '나무위키 뉴스'의 텍스트 데이터를 웹 크롤링으로 수집한 다음, 데이터 내에서 등장하는 키워드의 빈도를 분석해보겠습니다. 

이를 통해 현재 이슈가 되고 있는 키워드가 무엇인지 분석할 수 있을 것 입니다. 


# 페이지 살펴보기 

먼저 크롤링을 하기 위한 페이지를 열어보겠습니다. 저는 나무위키의 뉴스 페이지를 먼저 살펴볼 예정이므로 https://namu.news/section/news 에 접속해 페이지의 구조를 살펴보도록 하겠습니다. 

개발자 도구를 열어 페이지를 확인해보면 해당 페이지의 정보를 볼 수 있습니다. 

<그림 1>

뉴스의 텍스트 데이터에 접근하기 위해 뉴스들의 url을 먼저 추출합니다. 
개발자 도구 상단의 마우스 포인터 아이콘을 클릭한 후, url 정보를 확인하고 싶은 페이지의 링크을 클릭합니다. 클릭을 하면 다음과 같이 url 주소를 확인해 볼 수 있습니다. 

<그림 2>

# 웹 크롤링 라이브러리

파이썬에서는 BeautifulSoup와 requests 라이브러리를 사용해보도록 하겠습니다. 

requests는 특정 url로부터 HTML 문서를 가져오는 작업을 처리하고, BeautifulSoup는 문서에서 데이터를 뽑는 작업을 수행합니다. 이 모듈을 사용하기 위해서는 lxml, beautifulsoup4 requests 모듈을 설치해야 합니다. pip install을 이용해 설치해주도록 합시다. 

# 페이지 url 수집하기 

``` py
import requests
from bs4 import BeautifulSoup
import re

# 크롤링할 사이트 주소 정의 
source_url = "https://namu.news/section/news?page="
table_rows = []

# 페이지를 넘겨가면서 a 태그 크롤링 수행, list 만들어주기 
for page in range(5):
    req = requests.get(source_url+str(page+1))
    html = req.content
    soup = BeautifulSoup(html, 'lxml')
    contents_table = soup.find(name="div", attrs={"class":"hkXeBB"})
    table_rows.extend(contents_table.find_all(name="a"))

# a 태그의 href 속성을 리스트로 추출 
page_url_base = "https://namu.news"
page_urls = []
for row in table_rows:
    if len(row) > 0:
        page_url = page_url_base + row.get('href')
        page_urls.append(page_url)

# 중복 url 제거
page_urls = list(set(page_urls))

# 페이지 확인 
for page in page_urls:
    print(page)

```

위 코드는 개발자 도구로 살펴본 HTML 구조를 이용해 url을 뽑아내는 코드입니다.  
기사의 url이 div (class = hkXeBB) -> a 안에 존재하기 때문에 위 코드와 같이 계층구조를 좁히면서 매핑을 시켜줍니다.  
이러한 과정을 페이지를 이동해가며 반복하면 a 태그의 리스트가 만들어집니다.  
리스트의 아이템에서 get 함수를 이용해 href 속성값만을 추출해 낸 뒤 이를 완성된 url로 만들어 줍니다. 

# 텍스트 정보 수집하기 



<참고문헌>

