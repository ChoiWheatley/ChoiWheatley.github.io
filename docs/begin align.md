---
description:
aliases: 
tags: 
created: 2023-05-07T18:53:37
updated: 2023-07-11T15:21:09
title: begin align
---
- https://tex.stackexchange.com/questions/546655/how-does-align-work#546699
$$
\begin{align*} &\text{base case:}~~D_{(i, i)}=&0 \\ &\text{base case:}~~D_{(i, i+1)} = &k[i]+k[i+1]\\ &\text{general case:}~~D_{(i,j)}= &\sum_{m=i}^{j}k[m]+\arg \min_{i~\le~l ~\lt~j}{ D_{(i,~l)} + D_{(l+1,~ j)} } \end{align*}
$$
