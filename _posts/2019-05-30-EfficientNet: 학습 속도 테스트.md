---
title: "EfficientNet: 학습 속도 테스트"
date: 2019-06-04 17:58:28 -0400
categories: paper ML Tensorflow
---

https://pizpaz.github.io/paper/ml/EfficientNet-Rethinking-Model-Scaling-for-Convolutional-Neural-Networks 에서
소개했던 Efficientnet의 학습 속도를 테스트해봤다.

대회 참가했던 iFood2019용으로도 쓰고, 성능 측정도 할 겸 Google Cloud Platform에서 TPUv2-8로도 학습을 했다. 
참고로 TPUv2-8을 하루 사용하면 10만원 정도 과금된다.

## Efficientnet: GPU와 TPU에서의 학습 속도
* iFood2019 데이터셋 사용.
* batch size는 256 기준.

| model | FLOPS | (p40 x 8) global step/s | TPUv2-8 global step/s |
| -- | -- | -- | -- |
| vanilla resnet-50 | 4.1B | 6.0 | - |
| efficientnet-b0 | 0.39B | 2.64 | - |
| efficientnet-b3 | 1.8B | 0.78 | 0.94 |
| efficientnet-b4 | 4.2B | 0.4 | 0.58 |


### 결론
* p40 gpu 8개를 쓰는 것보다 tpuv2-8 하나를 쓰는 것이 조금 더 빠르다.
* vanilla resnet50보다 efficientnet-b3가 7배 넘게 느리다.  FLOPS는 절반 넘게 작은데... 항상 망령처럼 따라다니는 FLOPS와 현실과의 괴리.
* #### 서비스에는 못쓰겠다. 이건 못쓰는 놈.
