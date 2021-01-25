---
published: true
layout: single
title:  "바꿨습니다.. 도메인.."
header:
  overlay_image: /images/unsplash-image-1.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  actions:
    - label: "Learn more"
      url: ""
      
categories: [web]
tags: [web]
comments: true
---

사이트라도 이름은 있어야죠.. IP로 접근하는건 2% 부족한 것 같습니다.

&nbsp;

&nbsp;

현재 AWS를 이용해 자기소개 페이지와 프로젝트 페이지를 접근할 수 있는 간단한 서버를 운용하고 있습니다.  

AWS에서는 동적 IP 할당 기능을 제공하고 있어서 동일 IP로 계속 접근이 가능합니다.  

그런데 실제로 서비스되는 사이트들은 다 이름이 있습니다.  
서버의 IP 숫자로만 표현되는 제 사이트의 주소 링크를 그냥 보고 있자니 조금 안타까웠습니다.  
그래서 이번 기회에 AWS 인스턴스와 도메인을 한번 연결해보려고 합니다.  
(사실 동적 IP할당이 돈이 조금씩 나갑니다..)

&nbsp;

# 도메인 제공 서비스 이용

무료 도메인 제공 서비스를 검색 해보면 다음과 같이 DNS를 제공하는 서비스를 찾을 수 있습니다.

https://freedns.afraid.org

일단 사이트에 들어가면 계정을 만들어야 하는데 생성버튼을 누르면 메일인증이 하나 올 것입니다.  

메일로 온 url로 접속해서 서브도메인을 만들도록 합시다.  

![](/images/2021-01/freedns/1.png) 

저는 Subdomain을 만들었기에 이미 가운데에 관련 도메인이 만들어져 있습니다.  

왼쪽 메뉴에서 Subdomains를 누르면 다음과 같은 생성 창이 뜹니다. 

![](/images/2021-01/freedns/2.png) 

Type은 설명을 읽어서 알맞는 것으로 설정해 주시길 바랍니다.  저는 일반적으로 쓰인다는 A타입을 선택하겠습니다.  

Domain은 이미 무료로 제공하고 있는 도메인들을 모아놓은 것입니다.  
이 domain이 지정한 subdomin 뒤에 붙어서 주소가 완성됩니다.  
저는 치킨을 아~주 좋아하기 땨문에 치킨킬러로 도메인을 설정하겠습니다.  

다른 정보들을 모두 입력하면 Save를 눌러 주시길 바랍니다.  

저장이 성공적으로 되고 시간이 좀 지나면 첫번째 사진에 나와있는 것처럼 도메인 주소가 나와있을 것입니다.  
해당 주소로 접속해보면 사이트가 저희를 반겨줄 것입니다. 
