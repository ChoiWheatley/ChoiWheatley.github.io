---
description:
aliases: 
tags: 
created: 2023-05-10T21:32:00
updated: 2023-07-11T15:21:08
title: match case in python
---
# match case

rust에서 아주 파워풀한 match 키워드의 냄새가 폴폴 난다. 

-  [Rust - Pattern Syntax](https://rust-book.cs.brown.edu/ch18-03-pattern-syntax.html#multiple-patterns) | [[Multiple Patterns and match guards]]
-  [PEP 636 - Match Case Tutorials in Python](https://peps.python.org/pep-0636/)
- [백준 27447 문제풀이 코드 참고](https://github.com/OrmiCodeRanger/ChoiSeunghyeon/blob/a005d108a359d242d421217bae8b5c912b204fc3/ex27447.py)

매치 구문에 튜플을 넣어 if-else로 도배되었을 복잡한 저시기에 광명을 찾아줄 수 있다.

1. `case` 절에 변수를 바인드(캡처) 할 수 있다.
2. 구체적인 값을 필터링 할 수 있다. 심지어 그 필터링 자체를 or 연산을 하여 유연하게 대응할 수 있다. 심지어!! 그 필터링 결과를 `as` 키워드를 사용하여 캡처할 수도 있다.
3. wildcard `_`를 사용하여 사용하지 않는 케이스를 건너뛸 수도 있다.
4. 패턴 뒤에 `if`를 붙여 캡처 변수에 조건을 걸 수 있다.

```python
# 튜플을 분해하여 각각의 원소에 바인드(캡처) 할 수 있다 => Capture Patterns
numbers = (2, 4, 8, 16, 32)
match numbers:
    case (first, second, third, fourth, fifth):
        print(f'some numbers: {first}, {second}, {third}, {fourth}, {fifth}')

# 구체적인 값을 필터링하여 처리할 수 있다 => Literal Patterns
# 거기에 더해 wildcard `_`를 사용하여 exhaustive pattern matching을 활용!
def http_error(status):
    match status:
        case 400:
            return "Bad request"
        case 404:
            return "Not found"
        case 418:
            return "I'm a teapot"
        case _:
            return "Something's wrong with the Internet"
print(http_error(400))

# Pattern에 `or` 연산을 사용하여 유연하게 대응하는 모습
def http_error2(status):
    match status:
        case 401 | 403 | 404:
            return "Not allowed"
        case _:
            return "Something's wrong with the Internet"

print(http_error2(403))

# Pattern에 `as` 키워드를 사용하여 저시기를 캡처하는 모습
def http_error3(status):
    match status:
        case (401 | 403 | 404) as code:
            return f"Not allowed: {code}"
        case _:
            return "Something's wrong with the Internet"
print(http_error3(403))

# Pattern에 아예 `if` 조건을 달 수 있다 예를 들어 `or` 연산을 너무 많이 붙여야 
# 할 경우, 아예 검사식을 따로 추가하는 것을 고려할 수 있다.
available_direction = ["left", "right"]
def command_parse(command):
    match command.split():
        case ["go", direction] if direction in available_direction:
            return f"let's go {direction}!"
        case ["go", _]:
            return f"Sorry, you can't go that way"
        case _:
            return f"Unknown command"
print(command_parse("go left"))
print(command_parse("go down"))
print(command_parse("hello, world"))
```