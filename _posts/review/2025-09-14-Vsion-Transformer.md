---
layout: post
title: "Vision-Transformer"
subtitle: "an image is worth 16x16 words transformers for image recognition at scale"
category: review
tags: ai
image:
  path: /assets/img/paper-review/vit-architecture.jpg
---

기존에 Vision 분야에서 쓰이던 pre-training, transfer learning을 NLP 테스크에서 응용했는데 이제는 Transformer 아키텍처가 Vision 분야에 쓰이는 것이 흥미로워서 이 논문을 선택해 리뷰하게 되었습니다.

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}


** 논문 분석 전에 든 생각 **<br>
왜 Vision Field를 CNN를 발전시키지 않고 Attention Mechanism에다가 도입하려고 하는 걸까? 라는 질문이 떠올라서 조금 찾아보았습니다. 우선 Transformer 방식의 장점을 찾아보면 데이터와 모델의 성능이 비례한다는 점이 있었습니다. CNN은 small datasets에서도 성능이 좋다는 장점이 있지만, 21세기에 매우매우 많은 데이터들이 존재하는데, 고정된 구조 때문에 이 데이터들을 다 활용하지 못하고 데이터와 모델의 성능의 정비례 관계가 잘 성립하지 않는다고 합니다.

### 🍠 ABSTRACT
논문 이전 비전 분야에서는 Transformer를 직접적으로 사용하지 않고 제한적으로만 사용하고 있었고, Attention을 CNN과 결합, 대체하는 방식이었다.<br>
이에, CNN에 의존하지 않고, 이미지를 패치로 분할해 순수 Transformer에 직접 입력하는 방식을 제안했으며, 이미지 패치를 NLP의 단어 토큰 처럼 취급해서 Transformer에 입력한 것이 이 논문의 가장 큰 특징이라고 생각했다.<br>
논문의 가설을 통해 얻은 가장 주요한 결과는 Large Data로 pre-training 한 후에, transfer learning을 하면 VIT가 SOTA CNN과 비슷하거나 더 나은 성능을 보인다는 것이다.<br>
* 즉, 방대한 데이터와 전이학습만 있으면 CNN의 의존없이 Attention으로 높은 성능을 낼 수 있다!

## 🍠 RELATED WORK
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

* 하나의 입력 안에서 존재하는 단어들 간의 관계를 고려하는 것이 Self-Attention mechanism의 핵심이다

Attention 메커니즘은 Query, Key를 내적해서 코사인 유사도를 구한 후 스케일링한 가중치를 Value에 곱해주는 방식을 의미하는데, Self-Attention이란 말 그대로 하나의 입력에서 이 메커니즘을 수행하는 것을 말한다.

## 🍠 METHOD
![Model Architecture](/assets/img/paper-review/vit-architecture.jpg)

### VIT

VIT 모델은 기존 Transformer 모델을 가능한 한 변형없이 사용하기 때문에 입력값이 1D sequence of token dembedding이 되기 위해서 2D 이미지들을 $$x \in \mathbb{R}^{H \times W \times C}$$ (H는 높이, W는 너비 C는 채널 개수다 RGB를 사용하면 C는 3) -> $$x^p \in \mathbb{R}^{N \times (P^2 \cdot C)}$$ flattened 2D patche들로 바꿔준다. N은 여기서 $$x^p \in \mathbb{R}^{N \times (P^2 \cdot C)}$$ 식 그대로 원본 이미지 해상도를 패치 이미지 해상도로 나눠준 값으로 이해하면 될 것 같다. P는 논문에서 16으로 설정된 듯.

![Model Equation1](/assets/img/paper-review/vit-expression2.png)

그 후에 Transformersms constant latent vector size D를 사용해서 논문에서 이에 맞춰 patch들을 flat하게 하고 D dimensions에 맞춰 projection 한다, 이 프로젝션 결과를 논문에서는 patch embeddings라고 한다<br>

NLP에서는 BERT가 token을 사용하는데 이를 이미지에도 적용시켰다. 패치 임베딩의 맨 앞에 추가되어서 랜덤값으로 초기화 된 후 learnable embedding이기 때문에 학습되면서 이미지 전체를 요약하는 기능을 한다.<br>

Position embedding 또한 learnable embedding이다. 임베딩해서 1D 인풋으로 들어가다보니까 위치 정보가 왜곡되기 때문에 각 벡터에 위치 정보를 붙여준 것이라고 보면 된다. 논문에서는 2D 정보를 줘봤자 효과가 있지 않아서 그냥 1D 정보만 주었다고 함.

* Equation<br>

![Model Equation2](/assets/img/paper-review/vit-expression1.png)

이 사진은 ViT에서 일어나는 일들을 equations로 표현한 것이다. 입력은 이 전 사진에서 나온 9*(16^2*3)에 learnable embedding을 포함해서 10*768 크기의 벡터가 나온다. 그 후에 $$\text{LN}(x)_i = \gamma \cdot \frac{x_i - \mu}{\sqrt{\sigma^2 + \epsilon}} + \beta$$ 정규화를 거치고 Multi Head Self-Attention에 집어넣은 후 다시 정규화 시키고 MLP에 넣고 마지막으로 BERT에서 처럼 이미지의 대표 token $$y = \text{LN}(z^0_L)$$ 만 뽑으면 출력이다. <br>

** Inductive bias **<br>
Inductive bias는 학습 시에 만나지 않은 상황에 대해서도 정확한 예측을 할 수 있게 넣어두는 알고리즘이라고 한다. CNN에서는 이 Inductive bias가 매우 강하다. 애초에 CNN이 이미지가 지역적 정보를 많이 가지고 있다는 것을 이용하기 때문에 작은 데이터에도 잘 학습되지만, ViT는 MLP를 제외하고는 거의 다 global한 특성을 갖고 있어 inductive bias가 약하다

** Hybrid Architecture **<br>
ViT의 입력이 꼭 raw datasets일 필요는 없다고 주장한다. 대신 CNN Feature Map을 가져와서 입력으로 사용하는 방식을 제안한다. CNN backbone에서 feature map을 뽑고, 이 feature map을 작은 patch 단위 flatten해서 Transformer에 넣는 방식으로 작동한다.<br>
왜 이런 하이브리드 방식을 적용할까? 위에서 Vit는 Inductive bias가 약하다고 했는데 이 단점을 보완해줄 수 있기 때문이다. 하지만 Large data상에서는 그냥 순수 ViT가 더 성능이 좋다고 함.


### FINE-TUNING AND HIGHER RESOLUTION
모델을 pre-train할 때 large dataset으로 하고 fine-tuning 할 때 small dataset으로 훈련시킴(원래 하는 방식). fine-tuning 할 때는 pre-train 된 MLP를 뗀 다음에 작은 데이터 클래스 수인 K를 이용해서 D*K 짜리 layer를 하나 붙여서 사용했다. fine-tuning 할 때는 더 고해상도 이미지를 썼는데 패치 크기를 고정시키고 이처럼 하면 패치 개수가 늘어나서 더 많은 정보를 얻는 효과가 있는 것 같다.

## 🍠 EXPERIMENTS

ResNet, Vision Transformer, hybrid의 표현 학습 능력을 평가했다.<br>



### SETUP

* Datasets

* Model Varients

* Training & Fine-tuning

* Metrics

### COMPARISON TO STATE OF THE ART

### PRE-TRAINING DATA REQUIREMENTS

### SCALING STUDY

### INSPECTING VISION TRANSFORMER

### SELF-SUPERVISION

### SUMMARY


## 🐈 CONCULUSION

** 이미지를 직접 트랜스포머에 적용시켰다. ** <br>
기존의 연구들과는 달리 이미지를 패치들의 배열로 해석해서 트랜스포머 인코더에 적용한 것이 핵심이다. 또 논문에서 강조하듯이, large datasets로 pre-trained 된 모델에서 큰 성능을 발휘한다. 다른 이미지 분류 모델들과 성능은 비슷하거나 우월하면서 훈련 비용이 적게 든 다는 장점이 있다. <br>
논문에서는 몇 가지 해결해야할 문제들을 나열했다. <br>
1. ViT를 detection, segmentation 문제에 적용하기
2. self-supervised와 large-scale supervised 간의 성능 차이 줄이기
3. ViT 스케일링을 통해 더 나은 성능 도출해내기

## 🐈 나의 생각 및 궁금증

* 논문을 읽고 난 후에 든 생각들입니다.

1. 2D 이미지 정보를 통해서 그렇다할 성능 향상을 보이지 않아서 1D로 변환시켰는데, 유의미한 성능 향상이 나타나지 않은 이유는 무엇일까?
2. Large Datasets이 필요한 것을 알겠지만, 성능과 데이터의 반비례 관계를 지표화 시킬 수 있을까?
3. 왜 16x16 패치를 사용했을까, 48x48 크기의 이미지를 9등분 한 이유가 무엇일까?
4. Query, Key, Value를 현재 처리중인 데이터 토큰, 다른 주변 토큰, Q,K 사이의 유사도를 표현하는 V라고 본다면 이미지 처리에서 Query, Key, Value는 어떤 의미를 가질까? 

Vision 분야에 계속 흥미가 있어서 다음 논문 리뷰도 이미지 인식 관련해서 찾아보려고 합니다. 이번에 Transformer Model을 사용하는 VIT 논문을 리뷰 했는데 다음에는 이에 대항해서 Convolution 연산 위주인 **ConvNeXt (2022, Liu et al.)**를 리뷰하려고 합니다.<br>
감사합니다
Next Paper Review would be **ConvNeXt (2022, Liu et al.)!** i've got curiosity of CNN, and i will going to dig it.
