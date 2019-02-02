---
title: "Spatial Pyramid Pooling in Deep Convolutional Networks for Visual Recognition"
date: 2019-02-02 18:06:00 -0400
categories: paper ML
---

*https://arxiv.org/pdf/1406.4729v1.pdf, ECCV2014*

대부분의 Convolution Network들에서 학습 입력 이미지 크기를 고정으로 쓰고 있다. 이를 위해 수행하는 cropping, warping 때문에 결과 품질에서 손해를 보는 경우들이 있다. convolution layer의 연산은 입력의 크기에 구애받지 않는데, 입력 크기를 고정으로 하는 이유는 전적으로 fully connected layer 때문. 그래서 이 논문에서는 입력 크기를 고정하지 않고, FC 바로 직전에 Spatial Pyramid Pooling(SPP) layer를 추가하는 방식을 제안한다. 

<img src="http://postfiles7.naver.net/MjAxODA4MDlfMjU1/MDAxNTMzNzk1NDQ2OTc1.iDvPkSD3tM2wO-jZIaao4L6rzO8Wt0KAno7Tkcpkd50g.er-qdWAPzJETEDesXCEWZjn3iosxBSDPWpJSRZ1e3Rwg.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-08-09_%EC%98%A4%ED%9B%84_3.10.01.png?type=w773" width="500"/>

SPP의 동작 방식은 매우 간단. 바로 직전 convolution layer의 결과 feature map 크기가 가변일테니, 이 가변의 입력을 받아서, FC layer로 넘길 고정 크기의 tensor를 만들어내면 된다. 이 때의 포인트는, 여러 크기 종류의 결과를 만들어내도록 pooling을 다양한 크기로 여러번 수행한 후 concat 한다는 것. 위의 예제에서는 {1,4,16} 3개의 크기 종류를 출력하는 pooling을 수행하니까 최종 결과 크기는 21이 된다. 
학습을 할 때, 다양한 크기의 이미지를 이용하면 더 성능이 올라간다고 한다. 1 epoch마다 학습 이미지들의 크기를 바꿔주는 것이지. 논문에서 180~224 사이즈 사이를 오가며 학습했다고.

고정된 연구셋으로 연구/실험할 때와 다르게, 실제 필드에서는 말도 못하게 다양하면서 noisy한 데이터를 대상으로 모델을 적용해야 하는데, 이런 상황에서 cropping, warpping 등이 성능에 미치는 영향이 더 클 수 밖에 없다. 그래서 최대한 이미지 전처리에 덜 민감한 모델을 찾아내는 것이 중요해지고 있는 상황.
