---
layout: post
title:  "Simple network with 150 lines"
date:   2017-07-19
---
이 글에서는 아주 간단한 네트워크를 직접 구현해 보는 것을 목표로 한다. Nielsen문서나 다른 문서들을
보면서 이론을 이해했으면 그 문서에서 제공하는 코드를 보게 되는데 아무래도 각 층을 객체지향적으로 
구현하다보니 소스가 좀 복잡하다고 느껴진다.
특히 역전파같은 경우는 객체지향적으로 구현하기에 매우 적합한 구조라 생각하지만 모르는 입장에서는
막상 구현된 결과를 보고 전체적인 그림을 그리기 힘들다는 단점이 있다. 
그런 이유로 객체지향 구현보다 더 간단하지만(적어도 내가 생각하기에는) 비슷한 성능을 내는 코드를
처음부터 구현하면서 간단한 뉴럴네트워크에 대한 이해도를 높이려고 하였다.
따라서 코드는 절차지향적으로 작성되었으며 네트워크를 정의하는 부분만 Class를 사용하였다.
나머지는 모두 데이터가 입력되고 웨이트와 곱해지고 하는 네트워크의 절차적 흐름을 최대한 가독성 높게
구현하려고 하였다. 

전체 글은 jupyter notebook으로 작성되어 있어서 아래 링크를 통해 nbviewer로 공유한다.

[Simple network with 150 lines][simplenet]

[simplenet]: http://nbviewer.jupyter.org/github/metamath1/ml-simple-works/blob/master/simplenet/simplenet.ipynb
