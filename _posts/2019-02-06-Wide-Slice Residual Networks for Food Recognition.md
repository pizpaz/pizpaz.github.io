---
title: "Wide-Slice Residual Networks for Food Recognition"
date: 2019-02-06 14:37:28 -0400
categories: paper ML
---

*https://arxiv.org/abs/1612.06543*

이 논문에선 많은 종류의 음식이 수평적인 층을 가지고 있는 것에서 착안하여, 음식 이미지의 가로 전체를 하나의 conv filter로 연산하는 접근 방식을 취한다.

<img src="https://user-images.githubusercontent.com/2917022/52322687-6517fd80-2a1d-11e9-9285-81ae602f9126.png" width="500" />

간단한 접근 방식인만큼 구현도 간단하다. 224x5 conv를 한 후 1x5/3의 max pool을 하는 wide slice branch를 기존 residual branch와 concat하면 된다. max pooling은 vertically elongated window로 적용하는데, 음식의 vertical location이 항상 달라지는 것을 극복하기 위한 장치.

<img src="https://user-images.githubusercontent.com/2917022/52322690-68ab8480-2a1d-11e9-80c1-7f7782587af2.png" width="500" />

UEC-Food100, Food-101로 실험해봤는데, UEC-Food100은 어느정도 논문 수치가 재현되지만 Food-101은 턱도 없는 수치가 나온다. 논문에서 아예 언급하고 있지 않은 global average pooling 사용여부 등을 변경해가며 실험해봤지만 여전히 논문 수지가 재현이 안된다. 왜일까?


#### 논문의 Food-101 성능

<img src="https://user-images.githubusercontent.com/2917022/52322704-7234ec80-2a1d-11e9-8996-f59e7f6f2a75.png" width="500" />
