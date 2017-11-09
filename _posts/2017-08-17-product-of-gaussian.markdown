---
layout: post
title:  "베이즈정리와 정규분포의 곱Bayesian theorem and normal distribution multiplication"
date:   2017-08-17
---

베이지안 선형회귀bayesian linear regression를 공부하다 보면 계수벡터 w에 대한 사후 확률분포posterior를 구하게 된다.
이때 데이터의 분포를 정규분포로 가정한 가능도와 w에 대한 공액 사전분포conjugate prior의 곱을 
완전제곱completing the square 형태로 바꾸는 테크닉을 사용하여 사후 확률분포의 평균과 분산을 간단히 구하게 된다.
정규분포를 사전분포prior로 선택하면 사후분포도 정규분포가 되는 성질을 이용한 것인데
이 문서에서는 거기서 끝내지 않고 베이즈룰을 완전히 손으로 계산하여
일변수 정규분포에 대해서 p(D|\mu,\sigma^2)p(\mu)/p(D)가 정규분포가
되는지 확인해본다.

이를 위해 정규분포의 곱을 정리하고 간단한 프로그램으로 확인 해본다.
그리고 그 결과를 이용하여 가능도와 공액 사전분포를 곱하여 정리한다.
그후 p(D,\mu)를 \mu에 대해 주변화시키는 과정(적분)을 직접하여
베이즈정리에 의해 구해지는 사후 확률분포가 정확히 정규분포가 되는지 확인한다.

전체 글은 jupyter notebook으로 작성되어 있어서 아래 링크를 통해 nbviewer로 공유한다.

[베이즈정리와 정규분포의 곱][productgaussian]

[productgaussian]: http://nbviewer.jupyter.org/github/metamath1/ml-simple-works/blob/master/fitting/product-of-gaussian.ipynb 
