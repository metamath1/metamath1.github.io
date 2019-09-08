---
layout: post
title:  "합성곱 신경망에서 컨볼루션과 트랜스포즈드 컨볼루션의 관계 Relationship between Convolution and Transposed Convolution in CNN"
date:   2019-05-09
---

이 문서의 목적은 트랜스포즈드 컨볼루션transposed convolution 설명을 구체적으로 제시하는 것이다. 

구체적으로는 CNN에서 CONV층의 포워드 패스 컨볼루션의 백워드 연산이 상류층 그래디언트를 필터로 하는 컨볼루션이며 이 백워드 패스 컨볼루션이 결국 포워드 패스의 컨볼루션에 대한 트랜스포즈드 컨볼루션transposed convolution이라는 것을 알아보고자 하였다.

간단한 CNN CONV층을 가정하고 포워드 패스와 백워드 패스 간의 미분관계 그리고 그것과 트랜스포즈드 컨볼루션의 관계를 알아보았다.

다음과 같은 세가지 방법이 모두 동일한 연산이라는 것을 확인한다.

1. CONV층을 직접 미분하는 방법, 
2. cs231n에서 설명하는 필터를 적층시키는 방법, 
3. 필터를 적절히 행렬로 구성하고 이를 전치시키는 방법

CONV층의 미분을 직접 계산하여 결과가 올바른지 pytorch의 autograd.grad를 사용하여 검증하였다.

본 문서를 통해 아래 그림과 같은 트랜스포즈드 컨볼루션이 왜 그렇게 되는지 정확히 이해할 수 있게 될 것이다.

![Convolution arithmetic: https://github.com/vdumoulin/conv_arithmetic]({{ "/assets/no_padding_strides_transposed.gif" | absolute_url }})
 
전체 글은 jupyter notebook으로 작성되어 있어서 아래 링크를 통해 nbviewer로 공유한다.

[Relationship between Convolution and Transposed Convolution in CNN][transconv]

[transconv]: https://nbviewer.jupyter.org/github/metamath1/ml-simple-works/blob/master/CNN/transconv_fullconv.ipynb
