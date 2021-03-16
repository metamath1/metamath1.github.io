---
layout: post
title:  "패턴인식과 머신러닝 부록 D 변분법 Pattern Recognition and Machine Learning - Appendix D Calculus of Variations"
date:   2021-03-15
tags: [machine-learning, calculus, variations, PRML]
use_math: true
---

패턴인식과 머신러닝(Pattern Recognition and Machine Learning, PRML) 식(1.88)에 변분법을 적용하는 내용이 나오는데 변분법에 대해 간략하게 부록 D로 정리할 뿐 별다른 설명이 없어 부록을 읽어도 무슨 소린지 잘 모르게 되어있다. 
이 블로그가 늘 그렇듯 얉은 지식이나마 최대한 쥐어짜서 합리적인 수준에서 해당 내용을 이해할 수 있도록 변분법에 대한 기초 내용을 정리하였다. 

PRML에서 부록 D의 식(D.8)까지 내용이 정리되어 있기 때문에 이 글을 읽고나면 식(1.87)에서 안쪽 적분이 $f(\mathbf{x}, y, \dot y)$에 해당하는 항임을 알 수 있고,

$
\mathbb{E}[L] = \int \underbrace{\int \{y(\mathbf{x}) - t\}^2 \, p(\mathbf{x}, t) \, \text{d}t}_{f(\mathbf{x}, y, \dot y)} \,  \text{d}\mathbf{x} \tag{1.87}
$


이 때 범함수 $\mathbb{E}[L]$의 $y(\mathbf{x})$에 대한 미분은 오일러-라그랑지 방정식 좌변항에 의해

$
\begin{aligned}
\frac{ \delta \mathbb{E}[L]}{\delta y(\mathbf{x})} = \frac{\partial \, f}{\partial \, y} -  \frac{\text{d}}{\text{d}\mathbf{x}} \left( \frac{\partial \, f}{\partial \, \dot y} \right) 
= 2 \int \{y(\mathbf{x}) - t\} \, p(\mathbf{x}, t) \, \text{d}t
\end{aligned}
$

가 됨도 알 수 있게 된다. 이 결과를 0으로 놓은 식이 바로 식(1.88)이 되는데


$
\dfrac{ \delta \mathbb{E}[L]}{\delta y(\mathbf{x})} = 2 \int \{y(\mathbf{x}) - t\} \, p(\mathbf{x}, t) \, \text{d}t = 0 \tag{1.88}
$

 이 식의 유도 뿐 아니라 어떤 이유로 오일러-라그랑지 방정식이라는 것이 등장하는지 전체적인 의미를 이해할 수 있게 글을 작성하였다.

전체 글은 jupyter notebook으로 작성되어 있어서 아래 링크를 통해 nbviewer로 공유한다. 아래 링크를 클릭하면 전체 글을 읽을 수 있다. 

[Calculus of Variations][variations]

[variations]: https://colab.research.google.com/github/metamath1/ml-simple-works/blob/master/PRML/calculus_of_variations.ipynb


