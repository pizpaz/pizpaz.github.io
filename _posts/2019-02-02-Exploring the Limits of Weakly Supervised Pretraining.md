---
title: "Exploring the Limits of Weakly Supervised Pretraining"
date: 2019-02-02 18:08:00 -0400
categories: paper ML
---

*https://research.fb.com/wp-content/uploads/2018/05/exploring_the_limits_of_weakly_supervised_pretraining.pdf, Facebook*

- 인스타그램의 3.5 billion 이미지와 hashtag를 label로 아주 간단히 정제해서 ResNeXt-101로 학습. label 갯수는 1.5k ~ 17k. 42대 장비의 336개 GPU에서 학습(Accurate, Large Minibatch SGD:
Training ImageNet in 1 Hour)
- 이 pretrained 결과를 ImageNet 분류 등의 다른 task에 적용해봤더니, 결과가 매우 좋네?
- 데이터셋 annotation을 위해 별달리 한 것도 없어. 사용자들로 인해 데이터는 계속 늘어나. 게다가 label noise가 커져도 성능에 크게 영향을 안주네. 죽이지? pretraining/transfer learning에서 성능 향상의 여지는 아직 많아. ImageNet pretrained 기반으로 언제까지 쓸꺼니?(+통찰 몇가지) 정도의 느낌.


