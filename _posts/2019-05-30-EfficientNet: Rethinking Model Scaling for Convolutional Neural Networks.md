---
title: "EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks"
date: 2019-05-30 17:58:28 -0400
categories: paper ML
---

*https://arxiv.org/abs/1905.11946*

 ICML 2019에 나온 구글 논문. 먼저 눈이 갔던 것은 논문에서 제안하고 있는 EfficientNet의 성능 대비 엄청나게 낮은 파라미터 수와 Flops.
 ![1](https://user-images.githubusercontent.com/2917022/58635537-1f16da80-8329-11e9-83ba-33fa2bc342d5.png)

<img width="980" alt="2" src="https://user-images.githubusercontent.com/2917022/58635580-32c24100-8329-11e9-9f9c-e82081d8596b.png">

기존 Resnet 등보다 파라미터 수는 월등히 낮으면서도 성능은 오히려 좋다.

역시 갓구글이라고 회사 동료와 함께 외쳤다. 하지만...

공개된 https://github.com/tensorflow/tpu/tree/master/models/official/efficientnet 코드를 이용해서 바로 속도 테스트를 해봤다. 
이 코드는 TPU용이라 Multi-GPU에서는 동작하지 않으므로 코드는 조금 수정했다. 그 결과,

* gpu: p40 1gpu
* model: efficientnet-b4
* batch_size: 32 (64 이상에선 OOM 발생. 이때부터 좀 불안.)
* global step per sec: 약 0.47 (batch_size 128 기준으론 0.1175)

resnet50(+trick)에서 batch_size=128로 global step/sec 이 1.0 정도 나왔으니 9배 정도 느리네??
Flops와 실제 학습 시간이 비례하지 않음은 꽤 겪어오긴 했다. 하지만 Flops가 이정도로 낮은데, 학습 속도가 더 느려지는건 무슨 일인가.

https://cloud.google.com/tpu/docs/tutorials 에 적혀있는 training 시간으로 유추해봤을 때, TPU에서도 학습 시간이 efficientnet-b4가 resnet50 대비 최소 3배, 최대 7배 정도 느린게 맞는 것 같다. 
cpu infenrence 시간 기준으로 보면 3배 정도 느린 것 같고.

아. 이게 뭐람. 

실제 환경에서의 EfficientNet 속도와 무관하게, Model Scaling 관점에선(논문 제목도 Rethinking Model Scaling...) 다음 결과가 보편적으로 유의미한듯.
<img width="1014" alt="3" src="https://user-images.githubusercontent.com/2917022/58635596-3bb31280-8329-11e9-81c1-15c1bd5a9615.png">

<img width="511" alt="4" src="https://user-images.githubusercontent.com/2917022/58635601-3e156c80-8329-11e9-802e-aac1fa736c45.png">

baseline은 efficientnet-b0.

Network의 scale 요소인 width, depth, resolution 중 하나만 scale-up했을 땐 80% 정도에서 빠르게 saturate되는데, scale 요소를 
복합적으로 up시키면 더 좋은 성능이 나온다는 것. 그 최적 지점을 기계가 잘 찾을 수 있도록 compound scaling method를 정의해서 사용.
