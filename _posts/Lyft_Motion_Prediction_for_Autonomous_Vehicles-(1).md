---
layout: post
title:  "First Kaggle Challenge_Motion Prediction"
date:   2020-09-17
excerpt: "Lyft Motion Prediction for Autonomous Vehicles-(1)"
project: true
tag:
- Motion Prediction
- Autonomous Vehicles
- Kaggle
comments: true
---


# __Overview__

> ## Description

- 대회 주체자인 Lyft는 Level5(self-driving)을 위해 도전하고 있고, Level5 시스템을 구축하고 있다.
- 자율주행 자동차는 주변의 자동차, 자전거, 보행자의 움직임을 예측할 수 있어야 한다.
- 도전자들은 data science skill과 machine learning skill로 motion prediction model을 구축해야한다.

>  ## Code Requirements

- 이 챌린지는 Code competition 이다
- 제출은 Notebooks를 통해 제출해야만 한다
- CPU, GPU, TPU 를 무료로 사용할 수 있는데 시간제한이 있음
- 공개된 외부 데이터 사용가능, 사전 학습된 모델도 사용가능
- 제출할 파일 이름은 꼭 submission.csv이어야 함

        참고 문서: https://www.kaggle.com/docs/tpu

>  ## Evaluation

- 이 대회의 목표는 다른 교통에 참여하는 참가자(보행자, 자동차, 자전거등)의 궤도를 예측하는 것
- 샘플 당 단일 예측을 생성하는 단일 모달 모델 또는 여러 가설 (최대 3 개)을 생성하는 다중 모달 모델 (신뢰 벡터로 자세히 설명 됨)을 사용할 수 있다.
    + 모달 모델: 참가자의 움직임 궤도를 예측하는 모델
    + 단일, 다중 모델__*(잘 모르겠음)*__

- 다중 모달 예측이 주어지면 실제 정답값의 음의 로그 우도를 계산합니다.(Loss function은 음의 로그 우도를 사용한다)

        음의 로그 우도 참고문서: (https://ratsgo.github.io/deep%20learning/2017/09/24/loss/)


>>  ### 학습방법 추정
- 딥러닝 모델은 최대 우도추정(Maximum Likelihood Estimation)기법을 사용해서 학습
    + 주어진 데이터(X)만으로 미지의 최적 모델 파라미터(W)를 찾아야 한다.
    + 입력값 X와 파라메터 W가 주어졌을 때 정답 Y가 나타날 확률, 즉 우도 P(Y|X;W)를 최대화하는 W가 바로 우리가 찾고 싶은 결과라고 볼 수 있다.

    - Loss function으로 음의 로그우도(negative log-likelihood)를 사용한다

    참고: https://ratsgo.github.io/deep%20learning/2017/09/24/loss/

>  ## Submission File

- 교통에 참가하는 에이전트(자전거, 자동차, 보행자)는 track_id와 timestamp로 구별된다
- 각 에이전트의 궤적은 50개의 (x,y) 예측값이 있다.
- 하나의 에이전트에 대해 최대 3개의 궤적을 예측할 수 있다. 
- 각각의 궤적마다 신뢰도가 있고, 평가 중에 하나 이상의 궤적을 무시하도록 0으로 설정할 수 있다.

## 결론

: 3가지 행동(궤적) 중 하나를 예측해야 하며 가장 높은 확률을 가진 행동(궤적)이 정답 행동과 같아야한다.


>  ## Data

- 대회에서 제공하는 L5Kit module을 사용해 데이터 로드를 할 것을 권장한다
- L5Kit module은 kaggle_l5kit이라는 유틸리티 스크립트를 제공해 커널에 올리기만하면 간단하게 사용할 수 있다.
- 또한 추가해서 Notebook에 샘플이 업로드 된다 참고하길바란다.

>>  ### Data frame 구성

- scenes: 운전을 한 에피소드
- frames: 자율주행 차에서 찍은 순간 사진
- agents: 자율주행 자동차 센서에 잡힌 에이전트들 17개의 확률을 가진 에이전트들 중 4개만 데이터셋에 표현됨
- agents_mask: ?
- traffic_light_faces: 신호등 정보

- 다른 데이터도 도움 될 수 있으니 참고바란다

> ## What are we predicting?

- 99개의 테스트 frame을 보게 될 것이고 99개 frame 다음 50개 frame에서 에이전트들의 위치를 예측하면된다.

> ## File

- aerial_map: "py_satellite"모드로 래스터 화를 수행 할 때 사용되는 항공지도
        
        래스터화: https://m.blog.naver.com/skkim12345/221351312903

- semantic_map: "py_semantic"모드로 래스터 화를 수행 할 때 사용되는 고화질 시맨틱 맵
- sample.zarr: 샘플 데이터
- train.zarr: 학습 데이터
- test.zarr: 테스트 데이터
- validate.zarr: 평가데이터 (학습데이터와 크기가 비슷함)
- mask.npz: mask의 의미를 모르겠음'
- *sample_submission.csv: 제출방법 샘플 두개가 있음, 하나는 다중 모드 포멧, 다른 하나는 단일 모드 포멧
- train_full.csv: 기존 train.zarr보다 10배 크기의 데이터가 포함됨, 기존에 제공되는 데이터들을 포함하고 있음

