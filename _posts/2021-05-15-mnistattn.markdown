---
layout: post
title:  "어텐션 쉽게 이해하기Attention is easy to understand."
date:   2021-05-15
tags: [machine-learning, mnist, attention, MLP]
use_math: true
---

![]({{ "/assets/mnistattn/result.png" | absolute_url }})

이 글에서는 최근 딥러닝의 강력한 트랜드인 어텐션attention에 대해서 알아본다. 다른 블로그와 다르게 어텐션 본질에 집중하기 위해 언어모델이나 RNN같은 개념을 전혀 이야기하지 않고 MLP(Multi Layer Perceptron)만 사용한다. 최근 이슈가 되는 ViT(Vision Transformer)을 시작으로 유행이 되고 있>는 이미지를 패치로 잘라서 MLP에 입력하는 방법을 사용하고 있다.

이런 이유로 이 글을 읽기위한 선행 지식은 numpy와 pytorch, MLP뿐으로 가장 직관적으로 빠르게 어텐션의 의미를 파악할 수 있도록 작성하였다.

전체 글은 Google Colab으로 작성되어 있어서 아래 링크로 Colab을 통해 공유한다. 아래 링크를 클릭하면 전체 글을 읽을 수 있다.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/metamath1/ml-simple-works/blob/master/mnistattn/mnist_attn.ipynb) 어텐션 이해하러 가기



