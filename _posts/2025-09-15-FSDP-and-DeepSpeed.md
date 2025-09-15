---
title: 'FSDP + DeepSpeed'
date: 2025-09-15
permalink: /posts/tool/fsdp_and_deepspeed/
tags:
  - accelerate
  - large model
  - data parallelism
---

DP, DDP, FSDP에 대한 요약 및 DeepSpeed 라이브러리 소개

들어가기에 앞서: Data Parallelism이란?
-----

모델이 너무 커서 여러 GPU에 데이터를 병렬화 하여 로드하는 방식으로 아래와 같은 단계를 거쳐 이루어진다:
  1. Scatter: 입력된 데이터 배치를 GPU 개수만큼 등분하여 각 GPU에 할당한다.
  2. Replicate: 0번 GPU에 저장된 모델 파라미터를 각 GPU에 복사한다.
  3. Gather: 각 분산된 GPU에서 계산된 모델의 출력 값을 0번 GPU에 모은다.
  4. Scatter: 전체 loss를 계산한 후 각 GPU에 분산한다.
  5. Reduce: 각 GPU에서 계산된 gradient를 0번 GPU에서 합산하여 모델을 업데이트 한다.

당연한 얘기지만, 배치에 대해 병렬적으로 gradient가 계산될 수 있는 손실함수에 대해서 작동한다.
다만, GPU memory가 부족해서 gradient update를 여러 iteration에 나눠하는 방식과 차이점은 DP 방식은 contrastive learning과 같이 두 분산된 배치 간 pair를 구성하는 경우에도 사용가능하고 SyncBN을 통해 BatchNorm 통계를 보다 정확하게 계산할 수 있다.

위 방법(DP)은 python이 싱글 쓰레드에서 작동하여 효율이 감소한다. 이에 Distributed Data Parallelism (DDP)가 등장했는데 데이터부터 분할하여 각 프로세스에서 다른 GPU로 분산 연산을 수행하는 방법이다.
이 때, gradient만 sync해줌으로써 각 프로세스의 모델을 동일하게 유지할 수 있다.

FSDP (Fully Sharded Data Parallel)
-----

하지만 여전히 DDP는 모델이 모든 GPU에 똑같이 복제되어 있어야 한다는 한계를 가진다.
모델의 파라미터, gradient, momentum 등을 모든 GPU에 동일하게 가지고 있는 것은 large model 처리에 치명적인 단점이다.
(Adam optimizer 사용시 최소 모델 파라미터의 3배를 처리할 수 있는 GPU memory를 가지고 있어야 한다)
이를 해결하고자 모델 파라미터도 분산하여 처리하는 방식이 등장했고 이것이 FSDP이다.

FSDP에서는 Reduce-Scatter라는 개념이 등장하는데, DP에서 Reduce와 Scatter를 순차적으로 적용한 개념이다.
분산된 모델의 출력을 하나로 합치고 각 GPU에 분산시켜 주는 방식이다.
Gradient 계산 후에 모델의 각 부분에 해당되는 gradient만 대응되는 GPU에 분산시키는 방식으로 자세한 내용은 아래 프로세스를 참고하자.

Feed forward
  0. 각 레이어은 여러 GPU에 나누어 저장(파라미터가 분산되어)되어 있다.
  1. Gather: 각 GPU에 저장된 파라미터를 하나로 합쳐 연산을 수행한다.
  2. 각 GPU에서 소유했던 파라미터를 제외하고 모두 삭제한다.

Backward
  1. Gradient는 분산된 mini-batch에 대해 각 GPU에서 계산된다.
  2. Reduce-Scatter: gradient가 통합되어 파라미터 별로 GPU에 분산된다.

실제로 위 과정은 FSDP로 모델을 wrapping할 때, 레이어 별로 FSDP unit(노드 별)이 생겨 각 unit별로 돌아가게 된다.
0번 유닛에서 각 유닛의 파라미터와 gradient를 복제하여 처리한다고 하면, 0번 유닛 파라미터의 필요 메모리(optimizer 파라미터 포함)에 다른 유닛 중 최대 파라미터의 필요 메모리만큼이 필요하다.
i번 유닛에서는 0번 유닛 파라미터와 i번 유닛 파라미터의 합산 메모리가 필요하니 GPU 크기에 맞게 레이어를 분할할 수도 있다 (heterogeneous sharding).

DeepSpeed
-----
Microsoft 사에서 발발한 라이브러리로, FSDP에 필요한 메모리를 줄일 수 있는 최적화 도구이다: ZeRO(zero redundancy optimizer).
* https://www.microsoft.com/en-us/research/blog/ZeRO-deepspeed-new-system-optimizations-enable-training-models-with-over-100-billion-parameters/

위에서 답습한 내용이지만, DeepSpeed의 ZeRO 옵션을 통해 optmizer의 상태(모멘텀, 분산)을 샤딩할지, gradient만 샤당할지, 파라미터까지 샤딩할지를 선택할 수 있다.
메모리 효율은 아래 그림과 같다:
https://www.microsoft.com/en-us/research/wp-content/uploads/2020/02/DeepSpeed-Image-1.png<img width="951" height="392" alt="image" src="https://github.com/user-attachments/assets/442ed111-7325-4a2e-96bf-15ded410c35e" />
