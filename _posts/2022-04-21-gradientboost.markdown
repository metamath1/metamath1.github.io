---
layout: post
title:  "친절한 그래디언트 부스팅A Gentle Introduction to Gradient Boosting"
date:   2022-04-21
tags: [machine-learning, gradient, boosting, steepest, descent, gradientboosting, ensemble, boosting, TreeBoost, sklearn, scikit-learn]
use_math: true
---

![]({{ "/assets/gradientboost/linesearch.gif" | absolute_url }})

그래디언트 부스팅은 최근 머신러닝 알고리즘 중에서 가장 각광받는 알고리즘이지만 그 원리를 제대로 
설명하는 글을 접하기 쉽지 않은 것이 사실이다.
대부분 회귀 문제에 대한 예로 시작해서 선행한 약한 학습기가 만들어내는 에러를 보완한다는 
개념으로만 설명하는데 아쉽게 이런 설명으로는 그래디언트가 어디서 등장하는지 이해하기 쉽지 않다.

또 회귀문제에서 이야기한 논리를 분류문제에 적용하기도 쉽지 않은데 분류문제에 대한 적용을 설명한
문서는 거의 찾을 수 없어서 이 글을 작성하였다.
 
이 글에서는 간단한 회귀문제 예제로 시작하여 그래디언드 부스팅의 원리를 알아 본다.
그 과정에서 어떤 생략도 없이 가능한 친절하게 설명하고자 하였다.
그후 설명한 원리를 분류문제에 적용하기 위한 단계적 확장을 설명한다. 
100줄 미만의 코드로 설명한 원리를 직접 구현하여 그 구현 결과가 sklearn의 그래디언트 부스팅 구현과 정확히 일치하는지 확인한다.
마지막으로 TreeBoost에 대한 원리를 이해하고 구현하면서 글을 마무리 한다.

이렇게 원리 이해와 이해를 바탕으로 직접 구현한 결과를 sklearn과 비교하는 과정을 통해
그래디언트 부스팅에 대한 이해를 조금 넓힐 수 있는 계기가 되었으면 하는 바람이다.

이 글은 국가수리과학연구소 초청세미나 발표 목적으로 작성되었으며 발표 슬라이드는 여기[slide]에서 볼 수 있다.

![]({{ "/assets/gradientboost/001.jpeg" | absolute_url }})
![]({{ "/assets/gradientboost/005.jpeg" | absolute_url }})

전체 글은 Google Colab으로 작성되어 있어서 아래 링크로 Colab을 통해 공유한다. 아래 링크를 클릭하면 전체 글을 읽을 수 있다.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/metamath1/ml-simple-works/blob/master/gradientboost/gradient_boosting.ipynb) 그래디언트 부스팅 제대로 이해하러 가기


[slide]: https://docs.google.com/presentation/d/1I5hX2G3U_JzaJH2F05jD7O8uGn5tGYm4NbKLyPHapTI/edit?usp=sharing


