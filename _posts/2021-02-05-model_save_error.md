---
published: true
layout: single
title:  "Tensorflow2.X model.save Error NotImplementedError: Layer ResidualUnit has arguments in `__init__` and therefore must override `get_config`. + Custom ResNet"
header:
  overlay_image: /images/unsplash-image-1.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  actions:
    - label: "Learn more"
      url: ""
         
categories: [Deep Learning]
tags: [Error, Deep Learning]
comments: true

toc: true
toc_label: "Contents"
toc_sticky: true
# toc_icon: "heart"  # corresponding Font Awesome icon name (without fa prefix)
---

Tensorflow 2.x 버전을 사용하면서 발견한 에러와 해결방법에 대해 정리했습니다. 

&nbsp;

&nbsp;

제 환경은 다음과 같습니다. 
- python 3.6
- tensorflow 2.3

&nbsp;

&nbsp;

# 본문

텐서플로우를 통해 모델을 조금 더 개선하는 작업을 하고 싶어 커스텀 layer 유닛을 만들게 됐습니다. 

keras의 layer 모듈을 상속 받아 unit을 만들었습니다.  
이후 checkpoint callback 함수를 사용해서 h5 모델을 저장하려고 시도했습니다. 

&nbsp;

저장을 하는 과정에서 `model.save Error NotImplementedError: Layer ResidualUnit has arguments in __init__ and therefore must override get_config` 에러가 나오게 됐습니다.  

다음은 에러를 유발하는 코드입니다. 

&nbsp;

```py
class ResidualUnit(keras.layers.Layer):
  def __init__(self, filters, strides=1, activation="relu",**kwargs):
    super().__init__(**kwargs)
    self.activation = keras.activations.get(activation)
    self.main_layers = [
                        keras.layers.Conv2D(filters, 3, strides=strides,
                                            padding="same",use_bias=False),
                        keras.layers.BatchNormalization(),
                        self.activation,
                        keras.layers.Conv2D(filters, 3, strides=1,
                                            padding="same",use_bias=False),
                        keras.layers.BatchNormalization()
    ]
    self.skip_layers = []
    if strides > 1:
      self.skip_layers = [
                          keras.layers.Conv2D(filters, 1, strides=strides,
                                              padding="same",use_bias=False),
                          keras.layers.BatchNormalization()
      ]
  
  def call(self, inputs):
    skip_x = inputs
    x = inputs
    for layer in self.main_layers:
      x = layer(x)
    for layer in self.skip_layers:
      skip_x = layer(skip_x)
    
    return self.activation(keras.layers.add([x, skip_x]))

```

&nbsp;

&nbsp;

# 해결 방법 

에러를 해결하는 방법은 간단합니다.  
커스텀으로 만들어 낸 객체들을 config로 update할 요소를 명시해주면 됩니다.  
저는 다음과 같이 에러를 해결할 수 있었습니다. 

&nbsp;

```py
class ResidualUnit(keras.layers.Layer):
  def __init__(self, filters, strides=1, activation="relu",**kwargs):
    super().__init__(**kwargs)
    self.activation = keras.activations.get(activation)
    self.main_layers = [
                        keras.layers.Conv2D(filters, 3, strides=strides,
                                            padding="same",use_bias=False),
                        keras.layers.BatchNormalization(),
                        self.activation,
                        keras.layers.Conv2D(filters, 3, strides=1,
                                            padding="same",use_bias=False),
                        keras.layers.BatchNormalization()
    ]
    self.skip_layers = []
    if strides > 1:
      self.skip_layers = [
                          keras.layers.Conv2D(filters, 1, strides=strides,
                                              padding="same",use_bias=False),
                          keras.layers.BatchNormalization()
      ]

  def get_config(self):
    config = super().get_config().copy()
    config.update({
        'activation' : self.activation,
        'main_layers' : self.main_layers,
        'skip_layers' : self.skip_layers,
    })
    return config

  def call(self, inputs):
    skip_x = inputs
    x = inputs
    for layer in self.main_layers:
      x = layer(x)
    for layer in self.skip_layers:
      skip_x = layer(skip_x)
    
    return self.activation(keras.layers.add([x, skip_x]))


```

&nbsp;

&nbsp;

# Bonus : ResNet 기반 모델 만들어보기

에러를 해결한 코드는 다음과 같이 사용할 수 있습니다.

270 * 480 size의 흑백 사진을 5개의 class로 분류하는 문제입니다.  

마지막 dense layer에는 사용하고자 하는 loss function에 맞춰서 activation을 설정해주시면 됩니다.  

&nbsp;


```py
cnn_model = keras.models.Sequential()
cnn_model.add(keras.layers.Conv2D(64, 7, strides=2, input_shape=[270, 480, 1],
                              padding="same", use_bias=False))
cnn_model.add(keras.layers.BatchNormalization())
cnn_model.add(keras.layers.Activation("relu"))
cnn_model.add(keras.layers.MaxPool2D(pool_size=3, strides=2, padding="same"))
prev_filters = 64
for filters in [64] * 2 + [128] * 2 :
  strides = 1 if filters == prev_filters else 2
  cnn_model.add(ResidualUnit(filters,strides))
  prev_filters = filters
cnn_model.add(keras.layers.GlobalAvgPool2D())
cnn_model.add(keras.layers.Flatten())
cnn_model.add(keras.layers.Dense(128, activation='relu'))
cnn_model.add(keras.layers.Dropout(0.5))
cnn_model.add(keras.layers.Dense(5))
```

