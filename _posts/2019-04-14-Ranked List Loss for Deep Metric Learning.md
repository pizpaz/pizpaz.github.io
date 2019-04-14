---
title: "Ranked List Loss for Deep Metric Learning"
date: 2019-04-14 17:58:28 -0400
categories: paper ML
---
_https://arxiv.org/abs/1903.03238, CVPR2019_

<img width="1150" alt="1" src="https://user-images.githubusercontent.com/2917022/56089303-c6e85e00-5ecb-11e9-98ae-e37c8c8fc517.png">

query를 기준으로 알파만큼 떨어진 바깥으로 negative들을 밀고(경계 안쪽에 있을수록 더 큰 가중치 부여), negative 군집과 m 만큼의 margin을 두고 positive들이 군집을 이루도록 loss를 구성.

<img width="552" alt="2" src="https://user-images.githubusercontent.com/2917022/56089304-c8198b00-5ecb-11e9-9dbc-702b93f5edcb.png">
<img width="535" alt="3" src="https://user-images.githubusercontent.com/2917022/56089305-c9e34e80-5ecb-11e9-8f76-161c09aa326f.png">
<img width="536" alt="4" src="https://user-images.githubusercontent.com/2917022/56089307-cb147b80-5ecb-11e9-9c4e-44376ccccf87.png">
<img width="536" alt="5" src="https://user-images.githubusercontent.com/2917022/56089316-1b8bd900-5ecc-11e9-8e37-0dacebf1c3eb.png">


positive 군집을 위한 연산이 추가로 필요하게 된만큼 성능은 올라간 모양새. 그리고 margin으로 인한 hard-negative-sampling되는 효과도 있겠다. (RLL-(L,H,M)은 Googlenet의 각 FC embedding 결과를 concat 한 것.)
<img width="1055" alt="6" src="https://user-images.githubusercontent.com/2917022/56089317-1d559c80-5ecc-11e9-9bd2-5999207a0cc6.png">
