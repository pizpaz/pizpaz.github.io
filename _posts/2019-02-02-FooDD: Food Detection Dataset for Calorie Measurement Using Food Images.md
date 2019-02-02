---
title: "FooDD: Food Detection Dataset for Calorie Measurement Using Food Images"
date: 2019-02-02 18:12:00 -0400
categories: paper ML
---

*http://www.site.uottawa.ca/%7Eshervin/pubs/FoodRecognitionDataset-MadiMa.pdf*

동일 음식, 구도로 다른 카메라, 광도 등으로 찍은 복수의 데이터셋으로 구성. 한가지 이채로운 점은 이미지에 엄지손가락을 등장시켰다는 것. 이는 음식의 크기를 상대적으로 유추할 수 있게 해준다.
이걸 어디에 응용해서 쓸 수 있느냐? 대략 생각해보면, 음식 칼로리 계산을 한다고 했을 때 다음 과정이 필요하다.
1) 음식 분류 또는 음식을 구성하는 재료별 분류.
2) 음식 또는 재료의 크기(양) 측정.

1)도 1)일지만 2)도 제대로 하려면 만만찮은 task이다. 크기,양을 추정하는데 이미 알려진 크기를 가지는 기준 오브젝트와 비교하면 좀 수월해질 수 있을 것 같다. 이를테면, 손가락이나 동전과 같은.
<img src="http://postfiles7.naver.net/MjAxODEwMjRfMjkx/MDAxNTQwMzg2NDc5NDk0.PPHmBF2AGFAIfsMc4fkeZnN5ubdhm-S8KJpGkDGqkOMg.gMmF_-yFdZZoqcsTaAF49hh464rZzes_5keN6VTYtSUg.PNG.pizpaz3/04f9f5c8-c7cd-11e8-8ed1-6de9931b5f8f.png?type=w773" width="500"/>
