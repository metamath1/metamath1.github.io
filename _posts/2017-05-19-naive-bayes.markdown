---
layout: post
title:  "나이브 베이즈Naive Bayse - 밑바닥부터 시작하는 데이터 과학Data Science from Scratch 보충 설명"
date:   2017-05-19 
---
이 문서는 [밑바닥부터 시작하는 데이터과학][grus]의 나이브베이즈 단원에 대한 보충 설명이다. 책의 설명은
베르누이 나이브베이즈와 다항분포 나이브베이즈에 대한 구별을 하지 않는데 코드는 베르누이 나이브베이즈를
구현하고 있어서 약간 혼란을 준다. 
특히 라플라스 스무딩Laplace smoothing부분은 책의 설명만으로는 완전히 이해하기 힘들다.
그래서 인터넷 자료나 블로그를 찾아보면 베르누이분포와 다항분포를 구별하지 않고 주로 
다항분포인 경우 스무딩 적용법에 대해서만 설명하고 있다. (한글 자료는 이 둘을 구분해서 설명한 것을 찾기 힘들다.)
그래서 책의 코드와 인터넷 자료의 스무딩 설명과 일치하지 않아 책을 보고 이해를 못한 경우 인터넷을 찾아 볼 때 매우 혼란을 격게 될 가능성이 높다.
이 글은 책의 코드로 실습을 하면서 베르누이 분포와 다항분포를 모두 구현하여 그 둘을 비교하고
스무딩의 차이도 자세히 설명하였다.
 
전체 글은 jupyter notebook으로 작성되어 있어서 블로그에 바로 싣지 못하고 아래 nbviewer로 공유한다.

[나이브 베이즈 - "밑바닥부터 시작하는 데이터 과학" 보충 설명][naive]

[grus]: http://www.aladin.co.kr/shop/wproduct.aspx?ItemId=84725482
[naive]: http://nbviewer.jupyter.org/github/metamath1/ml-simple-works/blob/master/naive/naive.ipynb 
