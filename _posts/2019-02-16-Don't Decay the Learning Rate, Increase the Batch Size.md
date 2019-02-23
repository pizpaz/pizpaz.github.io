---
title: "Don't Decay the Learning Rate, Increase the Batch Size"
date: 2019-02-16 14:37:28 -0400
categories: paper ML
---

link:
*https://arxiv.org/pdf/1711.00489.pdf, ICLR2018, Google Brain.*


# The scale of random fluctuations, learning rate, batch size
* [A BAYESIAN PERSPECTIVE ON GENERALIZATION AND
STOCHASTIC GRADIENT DESCENT](https://arxiv.org/pdf/1710.06451.pdf)에서 유도된 바에 의하면,
The scale of random fluctuations in the SGD dynamics g는

![1](https://user-images.githubusercontent.com/2917022/52892890-46162a00-31da-11e9-8b28-93dfd93def65.png)

* 엡실론은 learning rate, N은 training set 크기, B는 Batch size.
* SGD dynamics에서 g는 simulated annealing 알고리즘에서의 온도와 비슷한 요소라, 처음엔 높게 잡고 점차 낮게 잡아가지요.
* 그래서 learning rate를 decay하며 학습해왔던 것이고.
* 그런데 위 수식에서 알 수 있듯이 N이 B보다 훨씬 큰 경우, learning rate를 줄이는 것과 batch size B를 높이는 것이 같은 스케일로 g에 영향을 미친다.

# 그러니 learning rate 줄이기 대신 batch 크기를 늘려주시오
* 그렇다면 step마다 learning rate를 줄이는 것보다 step마다 batch size를 늘리는게 낫지. 
* 왜냐? batch size가 커지면 update해야할 파라미터 수가 감소하고,
* data를 더 분산해서 처리할 수 있게 되니까
* 학습 속도가 빨라진다.

#  성능
* 우선, batch size B는 B ~ N/10 까지를 최대치로 실험했다고 함.

### CIFAR10
![2](https://user-images.githubusercontent.com/2917022/52892891-47475700-31da-11e9-9e1e-072646bceaa9.png)


* Hybrid는 첫 스테이지에서는 batch size를 올리다가(lr은 고정), 장비군의 처리 한도에 다다랐을 때부터 두번째 스테이지로 batch size는 고정하고 lr을 decay한 것.
* 동일한 cross-entropy loss 값으로 수렴하는데 Hybrid, increasing batch size가 업데이트 되는 파라미터 수가 단연 적다.
![3](https://user-images.githubusercontent.com/2917022/52892892-48788400-31da-11e9-9811-00d766ba0d44.png)
* accuracy는 셋다 비슷.

#### Advanced
* 빨간색은 learning rate, batch size 모두 증가시키는 것.
* 보라색은 learning rate, batch size, momentum coefficient 모두 증가 시키는 것. momentum coefficient까지 증가시키면 수렴은 좀 더 빨라지는데, accuracy도 좀 손해본다.
![4](https://user-images.githubusercontent.com/2917022/52892893-4a424780-31da-11e9-9a03-a74d5802f8d6.png)

### Imagenet
Imagenet도 비슷하다.
![5](https://user-images.githubusercontent.com/2917022/52892894-4c0c0b00-31da-11e9-8253-9a4626a9be3c.png)

# 비고
* (당연한 이야기이지만) 구글은 스케일이 우리랑 다른게, 몇만 단위로 batch size가 늘어나네...
* 우리 단일 장비에서는 batch size가 제한적이라, 지금 서비스에 적용한 lr decay 하는 비율만큼 batch size를 올리기는 힘들겠네. 그렇지만 적당히 lr decay 비율을 조정하고 거기에 맞춰서 실험해볼 수는 있을 듯.
