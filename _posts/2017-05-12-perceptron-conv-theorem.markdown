---
layout: post
title:  "퍼셉트론 테스트와 수렴정리Perceptron test and its convergence theorem"
date:   2017-05-12 
tags: [perceptron, machine-learning, convergence, python]
use_math: true
---

이 글에서는 간략하게 퍼셉트론의 동작 원리를 살펴보고 이를 python 코드로 실습한다.

아래 적혀 있는 퍼셉트론의 수렴정리에 대해서 잘 정리된 한글 자료는 거의 찾아보기 힘들기 때문에 
퍼셉트론 수렴정리에 대해서도 상세하게 정리하였다.

<b style="color:#FF0000">Theorem</b>. Assume that there exists some parameter vector $\mathbf{w}^{\*}$ such that $\lVert\mathbf{w}^{\*}\rVert=1$ , and some $\gamma>0$ such that for all $k=1 ... n$, $y\_{k}(\mathbf{w}^{\*} \cdot \mathbf{x}\_{k}) \ge \gamma$. Assume in addition that for all $k=1 ... n$, $\lVert \mathbf{x}\_{k} \rVert \le R$, Then the perceptron algorithm makes at most $\frac{R^{2}}{\gamma^{2}}$ errors. (The definition of an error is as follows: an error occurs whenever we have $y'\_{k} \ne y\_{k}$ in the algorithm.)

전체 글은 jupyter notebook으로 작성되어 있어서 블로그에 바로 싣지 못하고 아래 nbviewer로 공유한다.

[퍼셉트론 테스트와 수렴정리][perceptron]

[perceptron]: http://nbviewer.jupyter.org/github/metamath1/ml-simple-works/blob/master/perceptron/perceptron.ipynb
