---
description:
aliases: 
tags:
created: 2023-05-15T15:06:09
updated: 2023-07-12T10:50:15
title: re - regex python package
---


{% raw %}

- [[regex]]
- [weniv notion link](https://paullabworkspace.notion.site/1c57fc683c33468d95e7a490b6f66c95#d1317f3fd16e4d318cccec4906a3ab49)
* [regex google colab](https://colab.research.google.com/drive/1MpAPDAHo1CcWa1gB85BkH1CEch3DHXmy?usp=sharing)
* [ormi-master - regex.ipynb](https://github.com/ChoiWheatley/ormi-master/blob/main/python/regex.ipynb) ==> 여기로 가면 실습결과도 바로 볼 수 있어서...

```python
# 1
p = re.compile(r'([0-9]|10)([SDT])([\*\#]?)')
p.findall('1S2D*3T')

# 2
re.findall(r'([0-9]|10)([SDT])([\*\#]?)', '1S2D*3T')
```

# 자주 쓰는 re 메서드들

* `compile` : 패턴 컴파일
* [`match`](https://docs.python.org/3/library/re.html#re.match)
	* 문자열의 앞 부분이 매치되는가를 체크, 추출
* [`sub`](https://docs.python.org/3/library/re.html#re.sub)
	* 매치된 부분을 치환
* search() : 선두에 한해서 매치하는지를 체크, 추출
* [`findall`](https://docs.python.org/3/library/re.html#re.findall) : 매치된 부분 모두 리스트 반환
* [`finditer`](https://docs.python.org/3/library/re.html#re.finditer) : 정규식과 매치되는 모든 문자열(substring)을 반복 가능한 객체로 리턴한다.
* [`spilt`](https://docs.python.org/3/library/re.html#re.Pattern.split) : 정규표현 패턴으로 문자열을 분할

* 반환 객체의 값
    * group() : 매치된 문자열
    * groups() : 매치된 문자열 전체
    * start() : 매치된 문자열의 시작 위치
    * end() : 매치된 문자열의 끝 위치
    * span() : 매치된 문자열의 시작과 끝

* 컴파일 옵션(플래그)
    * 사용 예
	
    ```python
    re.compile('[a-z]+', re.I)
    ```
	
    * `re.DOTALL` or `re.S ` : 줄바꿈 문자까지 모두 매칭
    * `re.IGNORECASE` or `re.I` : 대소문자 구분하지 않음
    * `re.MULTILINE` or `re.M` : ^, & 등의 매칭 패턴을 라인마다 적용
    * `re.VERBOSE` or `re.X` : 아래와 같이 # 으로 주석문을 사용할 수 있음
	
    ```python
    a = re.compile(r"""\d +  # the integral part
                   \.    # the decimal point
                   \d *  # some fractional digits""", re.X)
    b = re.compile(r"\d+\.\d*")
    ```

* tip
    * 같은 패턴입니다.
	
    ```python
    re.compile('\\\\section')
    re.compile(r'\\section')
    ```
	
    * {}를 표현하고 싶을 때에는 중괄호 2개, 또는 때에 따라 3개가 필요합니다.
	
    ```ipython
    re.compile(f'{{section}}')
    ```

# re.group

https://docs.python.org/3/library/re.html#re.Match.group

regex에서 그룹을 사용하면 인덱싱을 돌릴 수 있다. 

> If the regular expression uses the `(?P<name>...)` syntax, the groupN arguments may also be strings identifying groups by their group name.

# re.sub

[re.sub - doc](https://docs.python.org/3/library/re.html#re.sub) 
미국식, 영국식 날짜로 표기하시오

```
입력: '2023/05/16'
출력1: '05/16/2023' 미국식 날짜 표기 월 -> 일 -> 년 순임...
출력2: '16/05/0223' 영국식 날짜 표기 일 -> 월 -> 년 순임...
```

## regex 그룹에 변수명을 붙일 수 있다...'

`(?P<VARNAME>PATTERN)` 으로 그룹을 선언하면 `sub`을 사용할 때 해당 그룹을 맞춰갈 수 있다.

```python
import re

# regex 그룹에 변수명을 붙일 수 있다... 바로 '?P<var-name>'
pattern = r'(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/(?P<day>[0-9]{2})'
r = re.compile(pattern)
strings = '''2023/05/16
1997/09/26
1968/10/06
1973/05/20
1999/06/19'''
print(r.findall(strings))

print('\n미국식 날짜표기\n')
for s in strings.split('\n'):
    print(r.sub(r'\g<month>/\g<day>/\g<year>', s)) 

print('\n영국식 날짜표기\n')
for s in strings.split('\n'):
    print(r.sub(r'\g<day>/\g<month>/\g<year>', s))


# 연습문제

- [숨어있는 숫자의 덧셈](https://school.programmers.co.kr/learn/courses/30/lessons/120851)

loop + `str.isdigit` 활용하여 풀기
```python
def solution(my_string):
    ret = 0
    for c in my_string:
        if c.isdigit():
            ret += int(c)
    return ret
```

list comprehension 활용하여 풀기
```python
def solution(my_string):
 	return sum(int(c) for c in my_string if c.isdigit())
```

# re.findall

[369 게임](https://school.programmers.co.kr/learn/courses/30/lessons/120891)

주어진 숫자 안에 있는 3, 6, 9의 숫자를 세시오.

```python
import re
solution = lambda order: len(re.findall('[369]', str(order)))

solution(29423)
```

## 연습문제: 잘라서 배열로 저장하기

https://school.programmers.co.kr/learn/courses/30/lessons/120913

사실 슬라이싱 문법 사용하여 간단하게 풀어도 되지만 `findall`을 사용하여 풀어보자
```python
# slicing 사용
solution = lambda my_str, n: [my_str[l:l + n] for l in range(0, len(my_str), n)]
solution('abc1Addfggg4556b', 6)
```

```python
import re
# re.findall 사용
# python에서 {를 넣기 위해선 {{를 사용하여야 합니다
solution = lambda my_str, n: re.findall(f'.{{1,{n}}}', my_str)
solution('abc1Addfggg4556b', 6)
```

## 연습문제: 영어가 싫어요

https://school.programmers.co.kr/learn/courses/30/lessons/120894

```python
import re

pair = {
    'one':   '1',
    'two':   '2',
    'three': '3',
    'four':  '4',
    'five':  '5',
    'six':   '6',
    'seven': '7',
    'eight': '8',
    'nine':  '9',
    'zero':  '0',
}

def solution(numbers):
    return int(
        ''.join(
            pair[i] for i in re.findall(
                r'(zero|one|two|three|four|five|six|seven|eight|nine)', numbers))
    )

solution('onetwothreefourfivesixseveneightnine')
```

## 연습문제: 암호문

https://pyalgo.co.kr/?page=2#

```python
import re

def solution(string):
    sum_ = sum(int(x) for x in re.findall(r'[rev](10|[1-9])', string))
    sum_ = str(sum_)
    return f'{sum_[0]}월 {sum_[1]}일'

solution('a10b9r10ce33uab8wc918v2cv11v10')
```

# re.split

정규표현 패턴으로 문자열을 분할한다. `str.split`과 같음

```python
import re

s = '010 8752!4037'

re.split(r'[ !]', s)
```

# 연습문제: Markdown 문법을 Html 문법으로

Before:

```
# Hello world
```

After:

```
<h1>Hello world<h1>
```


```python
import re

text = '''
# This is a heading

## This is a subheading

* This is an unordered list item
* This is another unordered list item

**This is bold text**

_This is italic text_

`This is code`
'''


def markdown_to_html(text):
    """
    마크다운 텍스트를 HTML 문서로 변환합니다.

    Args:
        text: 변환할 마크다운 텍스트.

    Returns:
        변환된 HTML 문서.
    """

    # 마크다운을 HTML로 변환하기 위한 정규식 패턴 목록입니다.
    substitution_pairs = [
        (r'^# (.*)', r'<h1>\1<h1>'),
        (r'^## (.*)', r'<h2>\1<h2>'),
        (r'^### (.*)', r'<h3>\1<h3>'),
        (r'^#### (.*)', r'<h4>\1<h4>'),
        (r'^\* (.*)', r'<ul><li>\1</li></ul>'),
        (r'^\- (.*)', r'<ol><li>\1</li></ol>'),
        # (r'**(.*)**', r'<strong>\1</strong>'),  # 문제가 있는듯
        (r'_(.*)_', r'<em>\1</em>'),
        (r'`(.*)`', r'<code>\1</code>'),
    ]

    # 정규식 패턴을 사용하여 마크다운을 HTML로 변환합니다.
    for pat, repl in substitution_pairs:
        text = re.sub(pat, repl, text)   

    return text


# HTML을 출력합니다.
print(markdown_to_html(text))
```

## 다른 사람들의 개쩌는 MD -> HTML 코드

```python
# 바름님 코드
import re

def md_to_html(text):
    # 토글
    text = re.sub(r'(>+) (.*)', r'<blockquote>\1 \2</blockquote>', text)
    # 헤딩
    text = re.sub(r'### (.*)', r'<h3>\1</h3>', text)
    text = re.sub(r'## (.*)', r'<h2>\1</h2>', text)
    text = re.sub(r'# (.*)', r'<h1>\1</h1>', text)
    # 코드블럭
    if re.match('\t?```', text):
        text = re.sub(r'^\t?```.*', r'<pre><code>', text)
        text = re.sub(r'\t?```$', r'</code></pre>', text)
    text = re.sub(r'\t?`(.*)`', r'<pre><code>\1</code></pre>', text)
    # 볼드
    text = re.sub(r'[\*_]{2}(.*)[\*_]{2}', r'<strong>\1</strong>', text)
    # 이탤릭
    text = re.sub(r'[\*_]{1}(.*)[\*_]{1}', r'<em>\1</em>', text)
    # 취소선
    text = re.sub(r'~~(.*)~~', r'<del>\1</del>', text)
    # 이미지
    text = re.sub(r'!\[(.*)]\((.*)\)', r'<div><img src = "\2" alt:="\1" width = "100"></div>', text)
    # 링크
    text = re.sub(r'\[(.*)]\((.*)\)', r'<div><a href = "\2">\1</a></div>', text)
    # 순서 없는 리스트
    if re.match(r'[ \t]*[\*-]+[ \t]+', text):
        text = re.sub(r'^[ \t]*[\*-]+[ \t]+(.+)', r'<ul>\n<li>\1</li>', text)
        text = re.sub(r'[ \t]*[\*-]+[ \t]+(.+)', r'<li>\1</li>', text)
        text += '</ul>'
    # 순서 있는 리스트
    if re.match(r'[ \t]*\d+\.', text):
        text = re.sub(r'^[ \t]*\d+\.[ \t]+(.+)', r'<ol>\n<li>\1</li>', text)
        text = re.sub(r'[ \t]*\d+\.[ \t]+(.+)', r'<li>\1</li>', text)
        text += '</ol>'
    return text
    
    
name = 'markdown'

f1 = open(f'{name}.md', 'r' ,encoding='UTF8')
f2 =  open(f'{name}.html', 'w' ,encoding='UTF8')
text = f1.readlines()
temp = ''
flag = 0
for i in text:
    # 여러줄짜리 코드블럭 처리
    if re.match('\t?```', i):
        temp += i
        if flag == 1:
            f2.write(md_to_html(temp))
            temp, flag = '', 0   
        flag = 1
        continue
    if flag == 1:
        temp += i
        continue
    # 순서있는 리스트 처리
    if re.match(r'[ \t]*\d+\.', i):
        temp += i
        flag = 2
        continue
    if flag == 2:
        f2.write(md_to_html(temp))
        temp = ''
    # 순서없는 리스트 처리
    if re.match(r'[ \t]*[\*-]+[ \t]+', i):
        temp += i
        flag = 3
        continue
    if flag == 3:
        f2.write(md_to_html(temp))
        temp = ''
    #특이사항없을시
    f2.write(md_to_html(i))

f1.close()
f2.close()
```

## 파이썬 모듈 `markdown`을 활용

[[markdown - python package]]


{% endraw %}