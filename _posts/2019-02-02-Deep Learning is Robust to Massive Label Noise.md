---
title: "Deep Learning is Robust to Massive Label Noise"
categories: paper ML
---

*https://openreview.net/pdf?id=B1p461b0W*

Approaches to learn from noisy data can generally be categorized into two groups:

* In the first group, approaches aim to learn directly from noisy labels and focus on noise-robust algorithms.
* The second group comprises mostly label-cleansing methods that aim to remove or correct mislabeled data.
* we do not aim to clean the training dataset or propose new noise-robust training algorithms. Instead, we study the behavior of standard neural network training procedures in settings with massive label noise.
* 학습데이터에서 노이즈와 correct 데이터 간의 비율보다는, correct 데이터의 절대적인 양이 학습 품질에 더 영향을 미친다. 즉, A decrease in the number of correct examples is more destructive to learning than an increase in the number of noisy labels.

EXPERIMENT 1: TRAINING WITH UNIFORM LABEL NOISE 실험 결과. 밑의 figure에서 볼 수 있듯이, noisy label 갯수가 늘어나더라도 accuracy에 엄청난 영향을 미치지 않는다. Resnet 등처럼 layer가 deep할수록 noisy label의 영향을 덜 받는 것도 볼 수 있다.

<img src="https://nicolaswon.files.wordpress.com/2018/05/e18489e185b3e1848fe185b3e18485e185b5e186abe18489e185a3e186ba-2018-05-31-e1848be185a9e18492e185ae-12-03-27.png?w=1024" width="500"/>

EXPERIMENT 2: TRAINING WITH STRUCTURED LABEL NOISE 실험 결과는  Figure5에서 왼쪽. Random order로 false label을 구성했을 때가 가장 결과에 악영향을 미친다. This result might help explain why we often observe quite good results from real world noisy datasets, where label noise is more likely to be biased towards related and confusing classes.

<img src="https://nicolaswon.files.wordpress.com/2018/05/e18489e185b3e1848fe185b3e18485e185b5e186abe18489e185a3e186ba-2018-05-31-e1848be185a9e1848ce185a5e186ab-11-54-47.png?w=1024" width="500"/>

마지막으로, noisy label  비율과 batch size, learning rate의 관계를 살펴봐보자.

<img src="https://nicolaswon.files.wordpress.com/2018/05/e18489e185b3e1848fe185b3e18485e185b5e186abe18489e185a3e186ba-2018-05-31-e1848be185a9e18492e185ae-1-11-37.png" width="500"/>
<img src="https://nicolaswon.files.wordpress.com/2018/05/e18489e185b3e1848fe185b3e18485e185b5e186abe18489e185a3e186ba-2018-05-31-e1848be185a9e18492e185ae-1-11-16.png?w=1024" width="500"/>
Our observations indicate that increasing noise in the training set reduces the effective batch size and  we see that one can additionally scale the learning rate to compensate for any remaining change in effective batch size induced by noisy labels.

