---
title: "Visual Search At Alibaba"
date: 2019-03-02 14:37:28 -0400
categories: paper ML
---

*https://102.alibaba.com/downloadFile.do?file=1534297833520/VisualSearch.pdf*

# Intro
알리바바의 타오바오앱의 Visual Search 품질이 심상치 않게 좋다는 이야기를 작년에 곧잘 회사 동료들과 나누곤 했다. 그 장맛의 비밀은 작년 하반기에 이 논문이 공개되면서 어느정도 밝혀졌다.

논문에선 크게 다음 3가지에 대해 설명하고 있다.
* 상품 카테고리 분류
* 유사 이미지를 찾는 모델
* re-ranking 방법 및 검색 시스템

# Joint Detection and Feature Learning
유사 이미지 검색 모델에 대해 살펴보자.
특이할만한 점은 localization과 유사 이미지 feature 학습을 end-to-end로 한다는 것이다. object localization이 품질에 미치는 영향은 다음 그림에 나타나 있다.

![screenshot from 2019-03-02 16-17-46](https://user-images.githubusercontent.com/2917022/53678808-92e41f80-3d07-11e9-978e-970ea8eff291.png)

유사 이미지 feature 학습 시엔 triplet loss를 기반으로 한다. 여기서 중요한 점은 triplet set 구축 시에 검색 결과 클릭 로그를 이용한다는 점이다. 

![screenshot from 2019-03-02 16-19-43](https://user-images.githubusercontent.com/2917022/53678818-a7281c80-3d07-11e9-9d0d-fb5f90b45507.png)

localization은 다음과 같이 진행된다.

![screenshot from 2019-03-02 16-20-15](https://user-images.githubusercontent.com/2917022/53678810-95467980-3d07-11e9-9280-9823dc6c68d6.png)

localization branch가 박스 좌표 4개를 찾고, 전체 이미지에서 박스에 있는 이미지를 마스킹해서 feature 학습 branch를 진행한다. 그리고 backpropagation 시에 triplet loss의 gradient가 localization branch까지 전달되어 학습되는 구조다.

전체 부분을 박스로 마스킹하는 다음 operation은 미분이 불가능하기 때문에, 불연속을 만드는 step 함수 h를 sigmoid로 교체해 미분 가능하게 만든다.

![screenshot from 2019-03-02 16-22-57](https://user-images.githubusercontent.com/2917022/53678809-94154c80-3d07-11e9-80cf-4c7720512f52.png)

# Disucussion
이 논문에서 중요한 점은 다음 3가지이다.

1. 마스킹을 하는 브랜치와 뒷 브랜치 사이의 discontinuous한 함수를 sigmoid로 대체해서 비슷한 효과를 내면서 backprop이 가능하게 했다.

2. triplet loss 구성도 인상적이다. 사용자 쿼리 이미지를 학습에 사용하고 있다는 점이 중요하다고 본다. 논문에선 판매자의 이미지(DB 이미지)는 잘 정돈된 환경에서 찍은 고퀄리티 이미지인 반면, 사용자 쿼리 이미지는 그 반대 환경에서 찍은 이미지들이 많은게 문제라고 운을 띄운 후, 이를 localization과 triplet loss 구성으로 해결했다는 뉘앙스임. triplet 구성 중 positive image q+과 negative image q-은 DB 이미지이고, q는 query image니까 둘의 특성이 하나의 공간 안에 잘 버무려져서 임베딩되겠지. 이게 서비스적인 측면에선 매우 중요한 포인트일 것 같다.
그리고, 무엇보다 무서운 것은 이 학습셋을 클릭로그를 이용해서 생성한다는 것이다. 사용자가 사진을 찍고, 클릭을 할수록 모델은 별다른 추가 정제작업 없이 더 견고해진다는 이야기가 되니까. 게다가 DAU가 2,3천만 단위라면 더 말할 나위없다. **쓸수록 강해진다**. 서비스와 모델 간 선순환의 좋은 예라고 본다. 
앞으로 이런 선순환적인 dog이득을 얻기 위해 서비스 설계 시에 클릭로그 등의 사용자 피드백을 학습 모델과 어떻게 연계할지 데이터 파이프라인 디자인까지 고려하는게 필요할 것이다. (+사용자 쿼리 이미지도 학습셋에 어떻게 잘 자동화해서 포함시킬 수 있을지) 아마도 글로벌 자이언트 회사들은 이런 프로세스가 이미 잘 구축되어 있을 것 같다.

3. 이건 시스템적 이슈인데, 논문을 보니 Fine-re-rank라고 부르는, re-ranking 기능도 꽤 잘 지원되는 검색 엔진을 디자인한 것 같다는 인상이다. 가끔 이야기했던 내용이지만, 추천 비스므레한 영역으로 연결되게 되면, 단순히 비주얼 유사도만이 아닌, 대상 제품 또는 음식점이 가진 다른 고유의 특성이나 text 기반의 다른 feature를 랭킹에 반영할 수 밖에 없을 것이다. 그럼 엔진에서 기능적으로 뒷받침이 되어야겠지. 
