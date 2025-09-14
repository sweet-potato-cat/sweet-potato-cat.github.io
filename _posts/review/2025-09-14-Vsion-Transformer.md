---
layout: post
title: "Vision-Transformer"
subtitle: "an image is worth 16x16 words transformers for image recognition at scale"
category: review
tags: ai
image:
  path: /assets/img/paper-review/vit_architecture.jpg
---

기존에 Vision 분야에서 쓰이던 pre-training, transfer learning을 NLP 테스크에서 응용했는데 이제는 Transformer 아키텍처가 Vision 분야에 쓰이는 것이 흥미로워서 이 논문을 선택해 리뷰하게 되었습니다.

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 개요
* 이 논문에서 사용한 이론이 pre-trainig과 transfer learning인데 원래는 Vision 분야에서 쓰이던 것이고 이를 LLM에서 응용했었는데, 이제는 NLP 분야의 Transfoermer를 Vision 분야에서 사용한다고 한다.
### 초록
논문 이전 비전 분야에서는 Transformer를 직접적으로 사용하지 않고 제한적으로만 사용하고 있었고, Attention을 CNN과 결합, 대체하는 방식이었다.<br>
이에, CNN에 의존하지 않고, 이미지를 패치로 분할해 순수 Transformer에 직접 입력하는 방식을 제안했으며, 이미지 패치를 NLP의 단어 토큰 처럼 취급해서 Transformer에 입력한 것이 이 논문의 가장 큰 특징이라고 생각했다.<br>
논문의 가설을 통해 얻은 가장 주요한 결과는 **Huge Data**로 pre-training 한 후에, transfer learning을 하면 VIT가 SOTA CNN과 비슷하거나 더 나은 성능을 보인다는 것이다.<br>
* 즉, 방대한 데이터와 전이학습만 있으면 CNN의 의존없이 Attention으로 높은 성능을 낼 수 있음!
## 선행연구
Transformer를 CNN에 도입했다 보니, Attention is All You Need 논문에서 제기된 이론인 Transformer Architecture와 Self-Attention mechanism 이론이 본 논문에서 사용되었다.
* Transformer Architecture (Vaswani et al., 2017)
* Self-Attention mechanism
### Transformer Architecture
Transformer Architecture는 기존 Seq2Seq 모델의 단점을 보안하고자 고안되었다.<br>
1. Can't process at parallel
2. Long Distance Dependency Problem

시퀀스의 특성상 순차적으로 입력을 해야 했기에 parallel process conducting이 불가하고, 이 때문에 대규모의 데이터 처리에서는 매우 긴 시간이 필요하다.<br>
또 Reference window의 크기가 한정되어 있기 때문에 시퀀스에서 멀리 떨어진 문장 또는 항목들과의 관계성은 학습이 되지 않았다.<br>
Attention Architecture는 Computing Resource만 infinite 하다면 이론적으로는 reference window 또한 infinite 하다.<br>
또한, 기존 병렬화 문제가 해결되고 모든 Query 는 각각의 Key와 비교되기 때문에 Long Distance Dependency Problem 또한 해결될 수 있다<br>
### Self-Attention mechanism


## 모델
![Model Architecture](/assets/img/paper-review/vit_architecture.jpg)

### VIT

### FINE-TUNING AND HIGHER RESOLUTION

## 가설
CNN 고유의 inductive bias 없이도, 대규모 데이터로 사전학습한다면 CNN 의존이 없는 Transformer만으로도 이미지 인식에서 SOTA 성능을 달성할 수 있다
## 실험 과정 및 결과
## 나의 생각 및 궁금증

## 결론

Next Paper Review would be **ConvNeXt (2022, Liu et al.)!** i've got curiosity of CNN, and i will going to dig it.
