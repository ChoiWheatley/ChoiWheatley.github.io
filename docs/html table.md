---
description:
aliases: 
tags: 
created: 2023-04-26T21:06:35
updated: 2023-07-15T21:33:04
title: html table
---
- [notion src](https://paullabworkspace.notion.site/table-298bb9380b9541cb852779393bbec940)
- table
	- tr : row
	- th : header : 테이블의 행,열, 제목
	- td: table data
	- colspan : 셀 병합 [[html table#^kgf9w8]] 
		- `<th colspan="3">Something</th>` => 현재 커서로부터 3개의 열을 병합한 열을 만든다.
	- rowspan : 셀 병합 (여러개의 행을 감싸는 부모행)
		- `<th rowspan="2">Somethin</th>` => 현재 커서로부터 2개의 행을 병합한 행을 만든다.
	- caption
	- thead : 머리글 (표 상단에 있는 줄)
	- tbody : 몸통
	- tfoot : 바닥글 (표 하단에 있는 줄)
	- colgroup : 열 그룹 (여러개의 열을 감싸는 요소) [[html table#^u8ukv6]]
		- 사용할만한 데가 CSS 스타일링 밖에 없는데

# table 마저 이어서

- [!] `<thead>` 와 `<th>`와의 차이점에 대해서 말하세욧
	- 가독성 차이에 불과.
	- 열그룹, 행그룹으로 구성할 때 사용.
- [!] `<colgroup>` 은 있는데 왜 `<rowgroup>` 은 없냐?
	- `<tr>`이 대신하고 있잖아.
- `colspan`, `rowspan` attr들은 `<th>`와 `<td>` 모두 적용 가능하다. ![[Pasted image 20230427100240.png|400]] ^zo77kz
---

![[image.png]] ^armjxj  
![[스크린샷 2023-04-26 21.13.18.png]] ^kgf9w8

![[스크린샷 2023-04-26 21.37.25.png]] ^u8ukv6
