---
title: "Learning a Discriminative Filter Bank within a CNN for Fine-grained Recognition"
date: 2019-02-02 18:00:00 -0400
categories: paper ML
---

*http://openaccess.thecvf.com/content_cvpr_2018/papers/Wang_Learning_a_Discriminative_CVPR_2018_paper.pdf, CVPR2018*

classification에서 발생하는 recognition과 localization 간의 trade-off를 피해보자는 시도.

global features를 위한 기존 stream 외에 discriminative patches를 학습하는 P-steam을 추가한 후 merge.

<img src="http://postfiles6.naver.net/MjAxODA3MjdfNiAg/MDAxNTMyNjc2NDAwNjky.GeXRovr8KiYBcEl0D3Ohto7Co-Jj1CfF9IDeddjRrPAg.yrwHix-0dYhyoaS0wjVKvZIAHu9AZLG33IJcExrsulIg.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-07-27_%EC%98%A4%ED%9B%84_3.55.57.png?type=w773" width="500"/>

이것만 가지고는 P-stream이 잘 동작하지 않으므로 각각 k개의 discriminative patches를 가지는 M개의 detector를 도우미로 투입.
<img src="http://postfiles4.naver.net/MjAxODA3MjdfMjY5/MDAxNTMyNjc2NDE5MjA1.uff_cqlfdg0WyVG0lGkcmQUH-272dJ4Vu0dSibaJ6NIg.U6XzPK4yAwX_cOwxdUmTGTyhg7A_jczzhROFbX5nsJYg.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-07-27_%EC%98%A4%ED%9B%84_3.56.28.png?type=w773" width="500"/>

이래도 bad local minima에서 허우적거리는 경우가 많았다고. 그래서 1x1 Conv를 random initialize하지 않고, 각 클래스 별로 Imagenet pretrained 모델의 Conv4_3 feature를 이용해서 initialize.
그래서 잡은 discriminative region.
<img src="http://postfiles7.naver.net/MjAxODA3MjdfMjgx/MDAxNTMyNjc2NDM3NjA5.85dY2vGJVgAuqCHNmGEw5lBDAXWC6wDRc5noxn6S36kg.7vSEPdX9kcljGy1n1s6-LIBOHo6jEpz6bv7YkL6N1ckg.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-07-27_%EC%98%A4%ED%9B%84_4.19.32.png?type=w773" width="640"/>


