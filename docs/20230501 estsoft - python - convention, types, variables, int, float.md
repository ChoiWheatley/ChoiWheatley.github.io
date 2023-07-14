---
description:
aliases: 
tags: 
created: 2023-05-01T09:31:22
updated: 2023-07-11T15:21:10
title: 20230501 estsoft - python - convention, types, variables, int, float
---
- [오늘자 google colab 주소](https://colab.research.google.com/drive/1gxoD01mjta80MkTOlrei1BHSUI0_k9-R?usp=sharing)
	- [?] google colab 자체가 하나의 블로그 글처럼 쓸 수 있는데, 옵시디언에 쓸 내용과 콜랩에 쓸 내용을 어떻게 해야할까?
- [오늘자 멘토님 google colab 주소](https://colab.research.google.com/drive/1IRa8nYwM2HtkzlNJGBlavWK96kffytm_?usp=sharing)
- [[0014 Python 🐍]]
- 알고리즘, 디버깅 챕터를 제외한 모든 강의는 google colab에서 진행할 예정.
- [[google colab|google colab 자체 단축키 등등]]
- [google python coding convention](https://google.github.io/styleguide/pyguide.html)
- types
	- 변수를 처음 만나면 `type`, `dir` 빌트인 펑션을 사용하여 종류를 찍어보자.
	- `isinstance(variable, type)`을 사용하여 변수의 타입을 런타임에 체크할 수 있다.
	- 속성
		- 모든 타입은 `class`이며, 그 안에 들어있는 메서드, 매직 메서드, 멤버들을 **속성**이라고 부른다. 이때 사용자는 파이썬 표준 라이브러리 안에 있는 클래스들의 속성을 변경할 수도, 추가할 수도 있다. 권장하지는 않지만 때로 확장이 필요한 경우에 고민해 보면 좋을 것 같다. (래퍼클래스에 매직메서드 구현이면 군침이 도는걸)
		```python
		class int(int):
			def _add__(self, a):
				return 'hello, world!'
		int('10') + int('10') # 신기하게도 'hello, world'가 나온다!
		```
- formatting
	- `print(f'{"hello":<10}')` => 10칸을 사용하여 왼쪽 정렬
	- `print(f'{"hello":>10}')` => 10칸을 사용하여 오른쪽 정렬
	- `print(f'{"hello":^10}')` => 10칸을 사용하여 가운데 정렬
	- `print(f'{"hello":!<10}')` => 10칸을 사용하여 왼쪽 정렬, 이때 공백을 `!`으로 채우기

- int
	- built-in functions
		- [`int.bit_length()`](https://docs.python.org/3/library/stdtypes.html?highlight=bit_length#int.bit_length) => sign, leading zero를 제외한 해당 숫자를 표현하기 위해 필요한 정확한 비트의 개수를 리턴한다.
		- [`int.to_bytes(length=1, byteorder, signed)`](https://docs.python.org/3/library/stdtypes.html?highlight=bit_length#int.bit_length) => 정수형을 의미하는 바이트의 배열을 리턴한다. `(2).to_bytes(1, byteorder='little', signed=True)` 
	- 2의 보수법
		- 자체적으로 음수를 표현할 수단이 없기 때문에 2의 보수법을 사용한다. 한 마디로 정리하면 **비트 뒤집고 +1** 하면 음수다.
		- 1의 보수법으로 음수를 표현하면 안되냐? => +0과 -0이 생기는 문제가 발생. 일단 비트 하나를 낭비한다는 문제도 있고, -0 + 1 = 0이라는 괴랄한 예외도 생긴다. 
		- 따라서, 비트 오버플로를 활용한 2의 보수법이 현재 정수 표기법으로 채택된 것이다.
- float
	- int형과 셈을 하면 별도로 지정하지 않는 이상 float로 형변환 된다.
	- [부동소수점 특유의 오차가 발생](https://docs.python.org/ko/3/tutorial/floatingpoint.html) => 정확한 소수점 계산이 불가. 
		- [무한대수 문제](https://www.notion.so/5f34f21bf9a34015b170e7afd7da9593): decimal 0.5를 이진수로 나타내면 binary 0.1이지만, decimal 0.1을 이진수로 나타내면 binary 0.000110011001100110011... 가 끝도없이 반복된다. 
	- 

---
# 멘토의 조언

>여러분, 실력자 분들은 어떻게 정리하셔야 할지 잘 모르시겠죠? 그래서 준비해 보았습니다.

1. 우선 아래 Lv0코드를 보면서 모르는 문법이나, 활용하는 문법에서 모르는 것이 있는지 한 번 체크해보세요.(https://github.com/paullabkorea/programmersLv0) [[fast io in python]]
	1. 모르는 것이 있으시면 한 번 정리 부탁드립니다. 예를 들어 collections나 itertools나 functools, re 정규표현식 같은 것이요. 
	2. 우리 수업에 해당 수업이 들어 있습니다. 예를 들어 정규표현식이 부족하시다면 정규표현식 수업에는 꼭 수업을 들으시고, 정리한 것을 확인하시고 복습해보세요.
	3. 수업 시간에 아는 내용들이 나오는 시간에는 1.1을 수행하시면 됩니다.

2. 만약 모르는 문법이 없고, 거의 다 자신있다면 문법보다는 부족한 부분을 채워주세요. DB, ERD, 리눅스, AWS, HTTP 등이요. 실습도 해보시고, 여러 자료를 보면서 블로그로 정리해두시면 좋습니다. 역시나 부족한 부분에 수업하는 시간은 꼭 수업을 들으시고, 정리한 것을 확인하시고 복습해보세요.

3. 조금 난이도 있는 알고리즘 문제는 저희 후반부에서도 다루니 조금 기다리셔도 좋습니다. 다만 지금 공부를 하고 싶으시다면 저는 책으로는 "파이썬 알고리즘 인터뷰"를 권해드립니다. 제가 알고리즘 책 중 가장 좋아하는 책이에요. 🙂 상길님이 카카오 코테 출제위원이셨습니다. 유형도 잘 정리가 되어 있고요.



> Python 책으로는 O'reilly Learning python 책이 정말 잘 나왔어요. 다만 이 책이 초급자 대상으로 나오긴 했으나 초급자에게 권하긴 힘듭니다. 1000페이지씩 상, 하로 나뉘어 있어요. 6판 나오면서 분량도 더 늘었다 들었습니다. O'reilly 시리즈 중 전문가를 위한 파이썬 FLUENT PYTHON도 정말 좋았습니다. 위 책은 페이지부터 압박이라서 한국 서적 중에서는 저는 ‘파이썬 코딩 도장’이 정말 괜찮았어요. 영상강의도 무료로 제공해주고, 텍스트도 웹으로 공개되어 있습니다. ‘점프 투 파이썬’도 유명하긴 한데 저희 수업에서 다루는 내용은 이보다 한 단계 위입니다. 파이썬을 이미 조금 다루실 수 있으신 분은 ‘컴퓨터 사이언스 부트캠프 with 파이썬’이거 보시면 한 단계 업 되실겁니다!