`소개 <ddp_series_intro.html>`__ \|\| **분산 데이터 병렬 처리 (DDP) 란 무엇인가?** \|\|
`단일 노드 다중-GPU 학습 <ddp_series_multigpu.html>`__ \|\|
`결함 내성 <ddp_series_fault_tolerance.html>`__ \|\|
`다중 노드 학습 <../intermediate/ddp_series_multinode.html>`__ \|\|
`minGPT 학습 <../intermediate/ddp_series_minGPT.html>`__

분산 데이터 병렬 처리 (DDP) 란 무엇인가?
=======================================

저자: `Suraj Subramanian <https://github.com/subramen>`__
번역: `박지은 <https://github.com/rumjie>`__

.. grid:: 2

   .. grid-item-card:: :octicon:`mortar-board;1em;` 이 장에서 배우는 것
      :class-card: card-prerequisites

      *  DDP 의 내부 작동 원리
      *  ``DistributedSampler`` 이란 무엇인가?
      *  GPU 간 변화도가 동기화되는 방법


   .. grid-item-card:: :octicon:`list-unordered;1em;` 필요 사항
      :class-card: card-prerequisites

      * 파이토치의 `(분산이 아닌) 기본적인 학습 방법  <https://tutorials.pytorch.kr/beginner/basics/quickstart_tutorial.html>`__ 에 익숙할 것

아래의 영상이나 `유투브 영상 youtube <https://www.youtube.com/watch/Cvdhwx-OBBo>`__ 을 따라 진행하세요.

.. raw:: html

   <div style="margin-top:10px; margin-bottom:10px;">
     <iframe width="560" height="315" src="https://www.youtube.com/embed/Cvdhwx-OBBo" frameborder="0" allow="accelerometer; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
   </div>

이 튜토리얼은 파이토치에서 분산 데이터 병렬 학습을 가능하게 하는 `분산 데이터 병렬 <https://pytorch.org/docs/stable/generated/torch.nn.parallel.DistributedDataParallel.html>`__ (DDP)
에 대해 소개합니다. 데이터 병렬 처리란 더 높은 성능을 달성하기 위해
여러 개의 디바이스에서 여러 데이터 배치들을 동시에 처리하는 방법입니다.
파이토치에서, `분산 샘플러 <https://pytorch.org/docs/stable/data.html#torch.utils.data.distributed.DistributedSampler>`__ 는
각 디바이스가 서로 다른 입력 배치를 받는 것을 보장합니다.
모델은 모든 디바이스에 복제되며, 각 사본은 변화도를 계산하는 동시에 `Ring-All-Reduce
알고리즘 <https://tech.preferred.jp/en/blog/technologies-behind-distributed-deep-learning-allreduce/>`__ 을 사용해 다른 사본과 동기화됩니다.

`예시 튜토리얼 <https://tutorials.pytorch.kr/intermediate/dist_tuto.html#>`__ 에서 DDP 메커니즘에 대해 파이썬 관점에서 심도 있는 설명을 볼 수 있습니다.

``데이터 병렬 DataParallel`` (DP) 보다 DDP가 나은 이유
----------------------------------------------------

`DP <https://pytorch.org/docs/stable/generated/torch.nn.DataParallel.html>`__ 는 데이터 병렬 처리의 이전 접근 방식입니다.
DP 는 간단하지만, (한 줄만 추가하면 됨) 성능은 훨씬 떨어집니다. DDP는 아래와 같은 방식으로 아키텍처를 개선합니다.

.. list-table::
   :header-rows: 1

   * - ``DataParallel``
     - ``DistributedDataParallel``
   * - 작업 부하가 큼, 전파될 때마다 모델이 복제 및 삭제됨
     - 모델이 한 번만 복제됨
   * - 단일 노드 병렬 처리만 가능
     - 여러 머신으로 확장 가능
   * - 느림, 단일 프로세스에서 멀티 스레딩을 사용하기 때문에 Global Interpreter Lock (GIL) 충돌이 발생
     - 빠름, 멀티 프로세싱을 사용하기 때문에 GIL 충돌 없음


읽을거리
---------------

-  `Multi-GPU training with DDP <ddp_series_multigpu.html>`__ (이 시리즈의 다음 튜토리얼)
-  `DDP
   API <https://pytorch.org/docs/stable/generated/torch.nn.parallel.DistributedDataParallel.html>`__
-  `DDP Internal
   Design <https://pytorch.org/docs/master/notes/ddp.html#internal-design>`__
-  `DDP Mechanics Tutorial <https://tutorials.pytorch.kr/intermediate/dist_tuto.html#>`__
