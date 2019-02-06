---
title: "Menu-Match: Restaurant-Specific Food Logging from Images"
date: 2019-02-06 12:59:28 -0400
categories: paper ML
---

*https://ieeexplore.ieee.org/document/7045971, Microsoft, 2015*

<img src="http://blogfiles.naver.net/MjAxODEyMTdfMzUg/MDAxNTQ1MDE3NjU5NDA0.NWzN0PcnuGIOmYaJ18Cj8SToaNXRVppEL1qyvjyrwaAg.TYDJuVTkPs_9oMP7lv3Fr2iy8abpk7aSvgq24LpO-y8g.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-12-17_%EC%98%A4%ED%9B%84_12.29.11.png" width="500" />

* 식단 관리 기록하기 힘든데 사진 한장 찍어서 가능하다면? 이란 질문에서 출발.
* 영양 단위나 개별 음식 정보로 칼로리 계산을 하기 쉽지 않으니, 식당별 음식으로 DB를 구축해서 해보겠다는 이야기.
* 전통적인 비전 기법 기반으로 접근. 데이터셋에 칼로리도 포함되어 있는데 작은 실험셋 수준.

<img src="http://blogfiles.naver.net/MjAxODEyMTdfODEg/MDAxNTQ1MDE3NjYyMzE2.HRTzFX5MCu3LS9glG9oDJzDuwK_h_u46Qw2grPRwcisg.AmvUv6XVpEv3lFUjXBLsF2YDVdH8g_gCY00ETCSKuN4g.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-12-17_%EC%98%A4%ED%9B%84_12.29.26.png" width="500" />

<img src="http://blogfiles.naver.net/MjAxODEyMTdfMjEy/MDAxNTQ1MDE3NjY1MTY4.1S4mEjwnA6GwNPycQUhj5q6zcQRz5B72exzZx2fTkrAg.bRQPnXCPa3EJuQ-2SRWgRQtVgJt_70tk9GXjC1jEo2Ag.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-12-17_%EC%98%A4%ED%9B%84_12.29.33.png" width="500" />

joint representation에서 pyramid pooling보다 rotationally invariant pooling을 썼을 때 유의미하게 성능 증가.

