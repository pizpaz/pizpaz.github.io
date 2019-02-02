---
title: "ChineseFoodNet: A Large-scale Image Dataset for Chinese Food Recognition"
date: 2019-02-02 18:10:00 -0400
categories: paper ML
---

*https://arxiv.org/abs/1705.02743*

ChineseFoodNet이란 중국 음식 분류 학습 데이터를 구축한 후, TastyNet이란 네트워크를 제안한다. 다음 2가지 정도만 주목.

1. ChineseFoodNet 데이터 구축 시, ImageNet의 pretrained 모델로 이미지 similarity를 계산하여 너무 가까운 이미지들은 중복으로 간주하여 제거.
2. TastyNet은 정말 별 것 없고, VGG19, Resnet, DenseNet 계열을 조합해서 쓰는 모델이다. 동일 모델 계열을 섞어쓰는 것보다 서로 다른 계열을 섞어쓰는 것이 더 성능이 좋았고, 특히 deeper와 wider 네트워크를 섞을 때 가장 좋은 성능을 보인다.

<img src="http://postfiles16.naver.net/MjAxODEwMDFfMTcy/MDAxNTM4MzYxMjAwMzg5.bmOhAyE7rltOQZwAXOWCEkeTVYVdfGv-pTmgVOxRW1Ug.ocBw-0XwzlgaUR-R1CNkenQI2KSW9JJaAiv4xAb9wKIg.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-10-01_%EC%98%A4%EC%A0%84_11.26.44.png?type=w773" width="500"/>


