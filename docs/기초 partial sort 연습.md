---
description:
aliases: 
tags: 
date created: Friday, February 10th 2023, 7:18:52 pm
date modified: Monday, February 27th 2023, 6:20:45 pm
created: 2023-02-10T19:18:52
updated: 2023-07-11T15:21:07
title: 기초 partial sort 연습
---
문제
	[no22. 기초 partial sort 연습](https://swexpertacademy.com/main/talk/codeBattle/problemDetail.do?contestProbId=AXsEXxxqe7sDFARX&categoryId=AYWab_JKjkwDFAQK&categoryType=BATTLE&battleMainPageIndex=1)

해설
	[[Pro 사전문제 - 기초 Partial Sort 연습]]  
	설명에 따르면 삽입정렬, 힙정렬이 가장 유효한 해답이 된다고 한다. 
	삽입정렬은 이미 정렬이 완료된 부분과 정렬이 덜된 부분을 나누어 생각하기 때문에 그때그때 새로운 데이터가 들어올 때 정렬하는 데 강점을 보인다. 
	내가 푼 방식인 힙정렬은 힙 속성을 만족하는 트리를 만드는데 로그의 시간이 걸리고 최소/최대값을 구하는데 단지 1번의 참조만 하면 되기 때문에 아주 유용하다.