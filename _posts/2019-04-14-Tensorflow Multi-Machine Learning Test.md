---
title: "Tensorflow Multi-machine learning test"
date: 2019-04-14 17:58:28 -0400
categories: tensorflow ML
---

회사에서 학습셋으로 몇천만장 단위의 데이터를 뽑을 수 있게 되었는데 100만장 정도의 학습셋을 학습한 경험 밖에 없다. 이 상태에선 파라미터 튜닝은 기다리다 지쳐 못한다.

## 언제까지 100만장에서 놀아나고 있을 것인가. 가자! 분산 머신 학습!
##### 안되면 말고.
### 참고
* [Distributed TensorFlow - TensorFlow Dev Summit 2018](https://www.youtube.com/watch?v=-h0cWBiQ8s8)
* [Distributed Training in TensorFlow](https://www.tensorflow.org/guide/distribute_strategy)
* https://www.tensorflow.org/api_docs/python/tf/estimator/train_and_evaluate
* [Distribution Strategy](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/distribute)
* https://www.tensorflow.org/api_docs/python/tf/estimator/train_and_evaluate
* https://github.com/tensorflow/examples/blob/master/community/en/docs/deploy/distributed.md
* https://github.com/tensorflow/community/blob/master/rfcs/20181016-replicator.md
* [Goodbye Horovod, Hello CollectiveAllReduce](https://www.logicalclocks.com/goodbye-horovod-hello-tensorflow-collectiveallreduce)
* [Accurate, Large Minibatch SGD: Training ImageNet in 1 Hour](https://research.fb.com/wp-content/uploads/2017/06/imagenet1kin1h5.pdf)

## 학습 속도 실험
1. 1 worker에서 in-graph로 동작하는 Mirrored strategy와 between-graph로 동작하는 CollectiveAllReduce 학습 속도 비교
2. CollectiveAllReduce에서 worker 수를 3개까지 늘려서 비교.

* machine: p40
* tensorflow version: 1.11, 1.12
* model: vanilla resnet50b
* dataset: 25만여장의 회사 내부 학습셋.
* batch_size: 128 per gpu



# A. 1 worker Mirrored vs. CollectiveAllReduce

strategy | worker수 | woker 당 gpu 수 | 총 gpu 수 | global step per sec | 비고
| - | - | - | - | - | -
A-1. Mirrored (nccl 사용) | 1 | 8 | 8 | 1.26 | . 
A-2. CollectiveAllReduce | 1 | 8 | 8 | 1.06 | Mirrored 대비 16% 하락

### 역시 nccl(nvlink 이용)이 빠르다.

# B. CollectiveAllReduce 학습 속도 비교

strategy | worker수 | woker 당 gpu 수 | 총 gpu 수 | global step per sec | A-1 대비 속도 | A-2 대비 속도 |
| - | - | - | - | - | - | - 
CollectiveAllReduce | 1 | 8 | 8 | 1.06 | 16% 하락
CollectiveAllReduce | 2 | 8 | 16 | 1.98 |  157% 증가 | 185% 증가
CollectiveAllReduce | 3 | 8 | 24 | 2.925 | 232% 증가 | 275% 증가

* CollectiveAllReduce 내에서는 worker수(gpu수) 대비 거의 선형으로 증가한다. [tensorflow benchmarks](https://www.tensorflow.org/guide/performance/benchmarks) 의 결과가 틀리지 않네! (파라미터 서버 사용 유무의 차이가 있긴 하지만.)
* 1worker에서 nccl을 사용했을 때와 비교해서는 좀 손해가 있지만, 232% 부터는 worker 수를 더 늘릴수록 거의 선형으로 성능이 올라갈 것 같다.
* 분산 학습해볼만 하겠다는 것! 이제 worker 3대 규모로 학습 성능이 어떻게 나오는지 실험해보면 되겠다.
