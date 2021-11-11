---
layout: post
title:  "진짜로(?) 주석 달린 트랜스포머 Really annotated transformers"
date:   2021-11-11
tags: [machine-learning, NLP, deep-learning, deep, learning, transformer, BERT]
use_math: true
---

![]({{ "/assets/transformer/trans.png" | absolute_url }})

이 글의 원문은 논문과 코드를 함께 적어논 아주 좋은 글 "[The Annotated Transformer][anno]"이다.
하지만 제목과는 다르게 코드에 주석이 거의 적혀있지 않고 코드의 구성도 논문에서 제시하는 
순서대로 큰 코드를 먼저 제시하고 작은 코드로 진행하는 형식(top-down 방식)이어서 이해하기 매우 난해하다. 

이 글은 "The Annotated Transformer"의 코드를 bottom-up 방식으로 재구성하여 읽는 이가 문서를 처음부터 
순서대로 읽기만해도 트랜스포머에 대해 편안하게 이해할 수 있도록 하는 것을 목적으로 하고 있다.
그리고 원문에서 많이 생략된 세부적인 부분(예를 들면 인코더, 디코더에서 일어나는 마스킹)을 예제 코드와 함께
상세하게 풀어적었다.



전체 글은 Google Colab으로 작성되어 있어서 아래 링크로 Colab을 통해 공유한다. 아래 링크를 클릭하면 전체 글을 읽을 수 있다.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/metamath1/ml-simple-works/blob/master/transformer/annotated_transformer.ipynb) 트랜스포머 제대로 이해하러 가기


[anno]: https://nlp.seas.harvard.edu/2018/04/03/attention.html


