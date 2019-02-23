---
title: "DropBlock: A regularization method for convolutional networks"
date: 2019-02-23 14:37:28 -0400
categories: paper ML
---

*https://arxiv.org/abs/1810.12890, Google Brain*

# 개념

![1](https://user-images.githubusercontent.com/2917022/53282173-6b291080-3777-11e9-9194-a6f2477a4e8e.png)

Regularization method인 DropBlock에 대한 논문이다. 고의로 unit을 drop한다는 개념에서는 dropout과 비슷하나, feature map에서 인접한 부분을 같이 drop한다는 점이 dropout과 다르다. dropout 처럼 랜덤하게 unit을 drop하면, semantic 정보 단위로 제거가 되지 않기 때문에 regularization 측면에서 큰 효과가 없다는 것. 그래서 이들은 인접한 영역을 하나의 block으로 제거하는 방식을 취했다고 한다.

# 구현

![screenshot from 2019-02-23 14-20-21](https://user-images.githubusercontent.com/2917022/53282174-6e240100-3777-11e9-9e43-241bc718abf9.png)

구현적인 측면을 봐보자. 위의 그림에서 x는 block의 중심점이다 이 중심점을 기준으로 block_size*block_size 가 block의 크기이고 이 단위로 drop이 된다. 이 때 drop하지 않고 activation 시킬 확률을 keep_prob 파라미터로 조정한다. 논문에선 0.75~0.9 정도를 사용했다고 한다.

# keep probability scheduling

keep_prob을 decay하면 성능이 더 올랐다고 한다. 이를테면 1.0으로 시작해서 학습이 끝날 때 쯤엔 0.7까지 떨어뜨리는 것이다. 스케쥴링 방식에는 별 것 없고, 그냥 선형으로 떨어뜨렸다고 되어 있다. 

# ImageNet 성능

![screenshot from 2019-02-23 14-20-45](https://user-images.githubusercontent.com/2917022/53282175-6fedc480-3777-11e9-97a5-e6e64be8f9b8.png)

바닐라 resnet50 대비 keep_prob=0.9로 했을 때 0.3% 정도 향상되었다. 위에서 언급한대로 keep_prob에 스케쥴링 적용 시, 1.5%까지 향상되었다.

# 비고
* 논문의 수치는 비교적 잘 재현된다.
* source의 학습에서도 dropblock을 사용하고, 이를 transfer learning할 때도 dropblock을 했을 때 가장 성능이 좋았다.
* 음식 이미지 데이터셋으로 transfer learning 시, keep_prob을 0.5까지 떨어뜨려도 성능이 떨어지지 않았다.
* 0.3까지 떨어뜨리자 성능은 다소 하락했으나 ECE 지표는 계속 향상되었다. 즉, regularization의 효과가 확실히 있었다.

