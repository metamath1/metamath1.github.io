---
layout: post
title:  "베이즈정리와 정규분포의 곱"
date:   2017-08-17
---

베이지안 선형회귀를 공부하다 보면 계수벡터 w에 대한 사후확률분포를 구하게 된다.
이때 가능도와 w에 대한 공액 사전분포의 곱을 완전제곱 형태로 바꾸는 테크닉을 사용하여
사후확률분포의 평균과 분산을 간단히 구하게 된다.
정규분포를 사전분포로 선택하면 사후분포도 정규분포가 된다는 성질을 이용한 것인데
이 문서에서는 일변수 정규분포에 대해서 $$P(D|\mu,\sigma^2)P(\mu)/P(D)$$가 정규분포가
되는지 확인해본다.

이를 위해 정규분포의 곱을 정리하고 간단한 프로그램으로 확인해본다.
이 결과를 이용하여 가능도와 공액 사전분포를 곱하여 정리하고
P(D,\mu)를 \mu에 대해 주변화시키는 과정(적분)을 직접하여
베이즈정리에의해 구해지는 사후확률분포가 정확히 정규분포가 되는지 확인한다.

전체 글은 jupyter notebook으로 작성되어 있어서 아래 링크를 통해 nbviewer로 공유한다.

[베이지정리와 정규분포의 곱][productgaussian]

[productgaussian]: http://nbviewer.jupyter.org/github/metamath1/ml-simple-works/blob/master/fitting/product-of-gaussian.ipynb 
