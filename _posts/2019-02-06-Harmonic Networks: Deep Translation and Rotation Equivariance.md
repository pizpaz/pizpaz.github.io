---
title: "Harmonic Networks: Deep Translation and Rotation Equivariance"
date: 2019-02-06 12:58:28 -0400
categories: paper ML
---


*https://arxiv.org/abs/1612.04642*

> We present Harmonic Networks or H-Nets, a CNN exhibiting equivariance to patch-wise translation and 360-rotation. We achieve this by replacing regular CNN filters with circular harmonics, returning a maximal response and orientation for every receptive field patch.


다음과 같은걸 하겠다는 이야기.

<img src="http://blogfiles.naver.net/MjAxODEyMTNfMjQx/MDAxNTQ0Njg2OTY4MjU2.1iO1K1JeFsezUb7xTTEHD-P6I7lOsY5QNzwOTyyZsd8g.SamO9KLwfjkwKqkFrukLBhcYA6a95BAnDNJzMzn6qbgg.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-12-13_%EC%98%A4%ED%9B%84_3.13.08.png" widht="500" />

이미지 공간에서 회전 변환이 가지는 특성이, feature 공간에서의 회전 변환에서도 그대로 유지된다면 좋겠지. 그럼 회전 변환에 대해 dense layer에서 projection할 때 더 용이해질테니까.

그럼 convolution filter를 어떻게 구성하느냐.

<img src="http://blogfiles.naver.net/MjAxODEyMTNfMjgy/MDAxNTQ0Njg2OTkyODA3.3KFlUtU_GRc1vqWRBPqx3BhtO7DMRGSvFHMUqQBT5Ccg.JBmjB05gSegEI2f25_CbS9hiEyLaw1x4Vl_aZaXAvqMg.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-12-13_%EC%98%A4%ED%9B%84_3.14.16.png" width="500" />

R 함수는 실수->실수의 함수이고, 베타는 phase offset 인데, 결국 이 둘을 학습하는 구조다. R을 여기선 가우시안 필터라고 해보자. 임의의 레이어에서의 필터 구성은 다음과 같이 된다.

<img src="http://blogfiles.naver.net/MjAxODEyMTNfMjcw/MDAxNTQ0Njg3MDAzODA0.TFSk6-H7ywFBl0igwg-BtHE-8RMzvIWXmEIgq8_dg2Yg.atj_L7yRlQ4bftE2OMZGWaPUV8sKg9Gl3YWq4krPz1Qg.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-12-13_%EC%98%A4%ED%9B%84_3.13.22.png" width="500" />

m=0 은 그냥 가우시안 필터가 되고, m=1,2 등의 rotation order를 가지는 필터는 해당 rotation에 강하게 반응하는 필터가 된다. 각 필터에

<img src="http://blogfiles.naver.net/MjAxODEyMTNfMTEw/MDAxNTQ0Njg3MDQzMTQ3.ACKKzNcgXc9Mt4tOMz0nBSbeKrvOO6TBN-Kw5T1oeYgg.mMO3Q_gCKvwxy1ojYDOwm_S0K40FvTjvCw89wmv5mw4g.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-12-13_%EC%98%A4%ED%9B%84_3.29.51.png" width="80" />

를 적용하면 필터 회전이 되는 것이고.


논문에선 명시적으로 표현은 안했지만, 수식 표현과 복소수 공간이라는 단서에서, 푸리에 변환과 연관되어 있음을 알 수 있다. 

<img src="http://blogfiles.naver.net/MjAxODEyMTNfMjM0/MDAxNTQ0Njg3MDc2MTQ4.p45wokxV2MhyAiaNiWnvuBr3qV3bMqNY-0GpK4UWSL4g.YmeMcSuua6_0McDccvqRV__6u3d8aBcnSNqlB2OOkWcg.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-12-13_%EC%98%A4%ED%9B%84_3.14.34.png" width="500" />

그래서 위와 같은 필터를 어떻게 만드느냐. phase offset은 일단 제외하고 생각해보자.

<img src="http://blogfiles.naver.net/MjAxODEyMTNfMTgx/MDAxNTQ0Njg3MDg2NTk3.3g2-jdxCYtmTFjkAemsSusOZIVxpIHwRS7rcZQuE6u4g.lfyHXETq0fqvF2vJTrCuRkYo68y1z0s3c_4qzhBHqkAg.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-12-13_%EC%98%A4%ED%9B%84_4.13.49.png" width="500" />

3x3 필터를 만든다고 해보자. 가상의 필터공간에서 원점과 거리가 ri인 원이 하나 있고, 이 원을 N개의 각으로 쪼갠다. 그리고 각 각도마다 3x3 크기만큼의 가상의 필터가 N개 있고, 각 필터는 원점을 기준으로 하는 좌표값을 갖는다고 하자.

이 N개의 필터를 아우르는 함수를 f(x)라 하고, 이 f(x)를 푸리에 변환하면 F(u)라는 주파수 공간의 함수가 나오게 되겠지.(바뀌 말하면 원함수 f(x)를 주기성을 가지는 함수 N개의 합으로 표현되게 변환했다는 이야기.) 
u는 [0, N-1]의 범위를 가지고, 각 값마다 원래 공간에서의 특정 주파수적 특성을 나타나게 된다. 근데 원함수 f(x)는 회전의 특성이 반영되게 만든 함수기 때문에, F(u)는 자연스레 u값마다의 회전각 주기성분에 강하게 반응하게 되고, 결과적으로 특정 각도에 반응하는 필터가 만들어진다.

이런 것을 원점과의 거리 ri를 다르게 해서 K개 만들어낸 후 더하면 최종적으로 위의 Fig2. 와 같은 필터가 만들어진다. Fig2.에서 표시된 rotation order m은 결국 이렇게 구한 F(u)의 u값에 해당되는 셈이다. 따라서 m의 종류는 N개만큼 있을 수 있다. 논문에선 각 레이어 별로 N개 중 1개 또는 2개만 사용했다고 한다.

이렇게 m개의 필터를 만들어낸 것을 다음과 같이 convolution 연산해서 다음 layer로 넘기면 끝. 

<img src="http://blogfiles.naver.net/MjAxODEyMTNfMjAg/MDAxNTQ0Njg3MDk4MjY1.q90hFVW7ZtAWWX71Yo7mBqHB44QRTe29qBrArUrO8BIg.kTmlsmcmMnS5gsvnRpHgnE6qKCjSW7MLJyxu6l6Mru0g.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-12-13_%EC%98%A4%ED%9B%84_4.17.18.png" width="500" />

각 레이어마다 rotation order를 3개씩 사용할 때, 레이어 간 연결은 다음과 같다.

<img src="http://blogfiles.naver.net/MjAxODEyMTNfNTYg/MDAxNTQ0Njg3MTEyMzA0.KidwUAyevffcyudP3WDxSdTseWxL3O2wUfC40dTM7Vwg.bsIzq2O8jcO6AKBWIEBhrhjsJmoytzPGGpXtGn8Gbvcg.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-12-13_%EC%98%A4%ED%9B%84_4.12.33.png" width="500" />

0,1,2는 m값인데, forward해나가다보면 중복되는 결과가 생길 수 있다. 예를 들어 

a) 첫번째 레이어에서 -90도 회전하고 다음 레이어에서 90도 회전.
b) 첫번째 레이어에서 가만히 있고 두번째 레이어에도 가만히 있는다.
 
이면 a),b)는 결과적으로 세번째 레이어로 넘어가는 feature map이 동일하게 된다. 따라서 두번째 레이어에서 2개 중 1개만 연산해서 계산량을 줄인다는 내용.

그래서 뭐가 얼마나 좋아졌냐.

다음과 같은 구조로 쌓아서(H-Net),

<img src="http://blogfiles.naver.net/MjAxODEyMTNfMTI5/MDAxNTQ0Njg3MTIyMjE0.rCILWRAi4hYJdhN0dIUEe1Xq38oRZvoAzYZbW9EjGYEg.i8slO0DAQTSeBEpdNPSNHUvh5rlRWa-JUiFDJguIRNsg.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-12-13_%EC%98%A4%ED%9B%84_4.25.13.png" width="500" />

MNIST-rot

<img src="http://blogfiles.naver.net/MjAxODEyMTNfNCAg/MDAxNTQ0Njg3MTQwNTk2.gGpXSkGEHxq23anB8FfYrJUOx3uX5w3f_ACzThvT88wg.B6xs_dqKwc0IB33cobC6MqTvSKAdClHAOCbuIUAXvrMg.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-12-13_%EC%98%A4%ED%9B%84_4.25.27.png" width="500" />

BSD500으로 boundary detection

<img src="http://blogfiles.naver.net/MjAxODEyMTNfMTA1/MDAxNTQ0Njg3MTU1NjY3.Wi1iBXYzoWMl-XuDx0QLImsxP8RBclD6lzsGttsnyTQg.-qXUTNz-RfGgy1k4M7sI-s5GWaj8kFaGq4oJtm8p7ekg.PNG.pizpaz3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2018-12-13_%EC%98%A4%ED%9B%84_4.26.57.png" width="500" />
