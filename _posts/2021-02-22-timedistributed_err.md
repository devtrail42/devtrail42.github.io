---
published: true
layout: single
title:  "케라스 TimeDistributed input 에러: 
ValueError: The channel dimension of the inputs should be defined. Found `None`."
header:
  overlay_image: /images/unsplash-image-1.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  actions:
    - label: "Learn more"
      url: "https://datascience.stackexchange.com/questions/55737/keras-reuse-trained-weights-on-cnn-with-different-number-of-channels"
         
categories: [Deep Learning]
tags: [Error, Deep Learning]
comments: true
---

&nbsp;

&nbsp;

TimeDistributed를 처음 써보고 접해본 에러의 해결방법 입니다.  

&nbsp;

# 에러내용
케라스에는 layer API에 TimeDistributed 라는 wrapper가 있습니다.  

레이어를 감싸서 연속적인 데이터를 같은 레이어에 각각 적용할 수 있습니다.  
이는 연속적으로 이어지는 이미지를 네트워크에 학습시키고자 할 때 많이 사용하는 layer입니다.  
이번에 이미지 분류모델에 처음 사용해보면서 다음과 같은 에러를 만날 수 있었습니다. 

```ValueError: The channel dimension of the inputs should be defined. Found `None`.```

&nbsp;

# 에러 해결 과정 

이는 보통 CNN의 이미지와 채널의 역할을 구분하지 못하는데에서 오는 실수로 발생됩니다.  

모델의 구조에 따라서 다르지만 제 경우에는 한장의 이미지를 잘라서 CNN 모델에 넣고 마지막에 LSTM을 적용하고자 하는 모델이었습니다. 

저는 초기에 이미지를 잘라 TimeDistributed에 넣는 대신에 CNN으로 학습을 시키고 나온 5개의 채널로 LSTM을 학습시키고자 하는 실수를 했습니다. 

채널은 모델의 가중치와 관련된 파라미터로서 이미지의 갯수와는 완전이 다른 속성을 지닙니다. 이미지의 갯수는 네트워크의 가중치 수에 영향을 주지 않습니다.  

TimeDistributed는 이미지 채널이 아니라 하나의 이미지인 frame을 받습니다. CNN의 동작구조를 어렴풋이 알고 적용시켰던 제 실수가 냈던 에러라고 볼 수 있습니다.  

따라서 이런 방식으로 하는 것이 아니라 처음부터 Image를 cropping하고 Timedistributed로 감싼 CNN에 적용한뒤 마찬가지로 TimeDistributed로 감싼 LSTM레이어에 적용시켜야 해결할 수 있습니다.  

저와는 다른 모델을 가지고 있지만 비슷한 문제를 가지신 분을 스택오버플로에서 찾을 수 있었습니다.  
자세한 내용은 상단 배너에 Learn more 버튼을 클릭하시면 알 수 있습니다.  


