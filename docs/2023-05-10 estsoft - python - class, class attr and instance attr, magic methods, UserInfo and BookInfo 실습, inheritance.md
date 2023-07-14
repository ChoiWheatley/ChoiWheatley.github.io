---
description:
aliases: 
tags: 
created: 2023-05-10T09:17:12
updated: 2023-07-12T10:46:25
title: 2023-05-10 estsoft - python - class, class attr and instance attr, magic methods, UserInfo and BookInfo 실습, inheritance
---
{% raw %}

# Class

[구글콜랩 링크](https://colab.research.google.com/drive/1gxoD01mjta80MkTOlrei1BHSUI0_k9-R#scrollTo=O7GLsOcjCaXC&line=19&uniqifier=1)

- 딱딱한 정의
    - 데이터(상태)와 기능을 가지는 인스턴스를 생성하기 위한 블루프린트.

- 직관적인 정의
    - 현실세계의 물체나 개념에 대한 정의를 간직하고 있는 덩어리
    - 1 + 1 = 2라는 공식을 컴퓨터는 모른다. 따라서 `class int`라는 이름 하에 정의한다.

- 인스턴스 & 클래스
    - 인스턴스: 실제 데이터(상태)를 가지고 있고 메모리 공간을 상주하고 있는 저시기
    - 클래스: 인스턴스를 낳는 거시기

- 메서드 & 멤버 = attribute
    - 메서드: 클래스 내의 함수
        - `__init__`: 인스턴스가 만들어질 때 최초로 한 번 실행되는 메서드
    - 멤버: 클래스 내의 변수

- `self` 라는 키워드가 모든 메서드의 첫 파라미터에 들어간다.

## Class attribute and Instance attribute

CPP의 static attribute???

클래스 변수는 클래스 바로 하위에 있는 변수이다. 클래스 변수는 모든 인스턴스가 공유할 수 있다. 따라서 `{{class-name}}.{{class-attr}}` 식으로 접근이 가능하다. 물론 `self.{{class-attr}}`도 가능함. 

문제는.... class attr를 instance attr와 혼동이 생길 우려가 있다는 것이다. 다음 예를 들어보자.
우리는 별 생각 없이 `Car.maxSpeed`를 변경한 걸로 알겠지만, 사실 엄밀히 따지고 보면 `change_speed` 메서드 안에서 새로운 instance attr인 `speed`를 만들어 저장한 것이 된다. 이것이 문제가 되는 이유는, `Car.maxSpeed`는 300으로 그대로인데, 프로그래밍 할 시점엔 실수하기 매우 쉽다는 것이다. 따라서, class attr를 참조할 땐 반드시 `self`가 아니라 클래스명을 사용하도록 권장한다.

```python
class Car:
	maxSpeed = 300
	maxPeople = 5

	def add_kinds(self, name):
		self.kinds.append(name)

	def change_speed(self, speed):
		self.speed = speed # 💥💥💥💥💥💥💥💥💥💥💥💥

k5 = Car()
k3 = Car()

k5.add_kinds('k5')
k3.add_kinds('k3')
k5.change_speed(500)
k3.change_speed(250)
```

class attr를 instance attr와 혼동하지 않고 사용하는 아래의 코드이다. `User` 인스턴스가 추가될 때마다 `User.count`가 증가하여 각자 고유한 `id` instance attr를 가지는 것을 볼 수 있다.

```python
class User:
	count = 0

	def __init__(self):
		self.id = User.count
		User.count += 1
```

몹과 유저를 만들어 서로 공격하게 만들어보자. 

```python
class User(object):
    
    def __init__(self, 이름, 공격력, 체력, 마력, 크기_넓이, 크기_높이, x, y):
        self.name = 이름
        self.power = 공격력
        self.hp = 체력
        self.mp = 마력
        self.width = 크기_넓이
        self.height = 크기_높이
        self.x = x
        self.y = y
        self.alive = True
    
    def take_damage(self, other):
        if self.hp < 0:
            print(f'DEBUG ---> {self.name} has already died.')
            pass
        self.hp -= other.power
        print(f'DEBUG ---> {other.name} attacks {self.name}. {self.name}.hp is now {self.hp}')
        if self.hp < 0:
            self.alive = False
            print(f'DEBUG ---> {self.name} died by {other.name}')

    def attack(self, other):
        other.take_damage(self)

    

class Mob(object):
    def __init__(self, 이름, 공격력, 체력, 마력, 크기_넓이, 크기_높이, 아이템확률, x, y):
        self.name = 이름
        self.power = 공격력
        self.hp = 체력
        self.mp = 마력
        self.width = 크기_넓이
        self.height = 크기_높이
        self.dropRate = 아이템확률
        self.x = x
        self.y = y

    
    def __repr__(self):
        return f"""{{
    name: {self.name}        
    power: {self.power}
    hp: {self.hp}
    mp: {self.mp}
    width: {self.width}
    height: {self.height}
    dropRate: {self.dropRate}
    x: {self.x}
    y: {self.y}
}}
"""


    def attack(self, user):
        user.take_damage(self)

    def take_damage(self, other):
        if self.hp < 0:
            print(f'DEBUG ---> {self.name} has already died.')
            pass
        self.hp -= other.power
        print(f'DEBUG ---> {other.name} attacks {self.name}. {self.name}.hp is now {self.hp}')
        if self.hp < 0:
            self.alive = False
            print(f'DEBUG ---> {self.name} died by {other.name}')


class Slime(Mob):
	"""
	아직은 상속에 대하여 배우지 않았지만, 써보는 건 공짜잖아?
	"""

    def __add__(self, other):
        if isinstance(other, Slime):
            return Slime(f'왕{self.name + other.name}', self.power + other.power, 
                         self.hp + other.hp, 
                         self.mp, 
                         self.width + other.width,
                         self.height + other.height,
                         self.dropRate,
                         self.x,
                         self.y)
        

슬라임 = Slime('슬라임', 1, 10, 10, 2, 2, 100, 0, 0)
오크 = Mob('오크', 10, 10, 10, 2, 2, 80, 0, 0)
고블린 = Mob('고블린', 100, 10, 10, 2, 2, 60, 0, 0)
드래곤 = Mob('드래곤', 1000, 10, 10, 2, 2, 40, 0, 0)
해골 = Mob('해골', 10000, 10, 10, 2, 2, 1, 0, 0)

licat = User('licat', 9, 30, 15, 10, 10, 0, 0)
mura = User('mura', 10, 35, 0, 10, 10, 0, 0)

슬라임.attack(licat)
해골.attack(mura)

licat.attack(슬라임)


슬라임2 = Slime('슬라임', 1, 10, 10, 2,2,100, 0,0)
왕슬라임 = 슬라임 + 슬라임2

print(왕슬라임)
```

아래 예제는 클래스를 활용하여 간편하게 html 문서를 만드는 저시기를 설명한다.
```python
# 쉽고 중요한 예제!

class BlogFactory(object):
    dataset = []

    def __init__(self, 제목, 내용, 조회수, 글쓴이, 생성날짜):
        self.title = 제목
        self.content = 내용
        self.count = 조회수
        self.writer = 글쓴이
        self.create_date = 생성날짜
        BlogFactory.dataset.append(self)

게시글1 = BlogFactory(
    '오늘 제주의 날씨',
    '오늘 제주의 날씨는 참 좋네요! 블라블라',
    '0',
    '이호준',
    '2023/05/10',
    )

게시글2 = BlogFactory(
        '오늘 부산의 날씨',
        '오늘 부산의 날씨는 참 좋네요! 블라블라',
        '1000000',
        '김재현',
        '2023/05/10',
    )

게시글3 = BlogFactory(
        '오늘 강원의 날씨',
        '오늘 강원의 날씨는 참 좋네요! 블라블라',
        '10000',
        '범남궁',
        '2023/05/10',
    )
 

for i in BlogFactory.dataset:
    print(f'<h2>{i.title}</h2>')
    print(f'\t<p>{i.content}</p>')
    print(f'\t<p>{i.writer}</p>')
    print(f'\t<p>{i.count}</p>')
    print(f'\t<p>{i.create_date}</p>')

```

## Magic methods

[[magic methods (python)]]

## UserInfo, BookInfo 실습

[[repr, dir, vars, pprint.pformat + UserInfo and BookInfo 실습 (python)]]

## Inheritance

python의 상속체계는 여타 언어들의 상속과 비슷해 보인다.

```python
class Car(object):
    maxSpeed = 300
    maxPeople = 5

    def __init__(self):
        self.option = ['aircon', 'fly', 'wheel', 'nuc']

    def move(self, x):
        print(x, '의 스피드로 달리고 있습니다.')
    def stop(self):
        print('멈췄습니다.')

class HybridCar(Car):
    battery = 1000
    batteryKM = 300

class ElectricCar(HybridCar):
    battery = 2000
    batteryKM = 600

# print(set(dir(Car)) - set(dir(object)))

assert id(Car.maxSpeed) == id(HybridCar.maxSpeed)  # ok
assert id(Car.move) == id(HybridCar.move)  # ok
```

- [x] class attr는 같은 id를 같는다는 건 확인. 그렇다면 instance attr는?
	- 직접 실험해 보았다. 결론부터 말하자면 서로 다른 id를 갖는다. 이유로 말할 것 같으면.......... `Car.__init__`을 상속받는 건 똑같은데, 메서드 내부 블럭은 각자 실행하기 때문인 것으로 보인다.
```python
bongbong = HybridCar()
cybertruck = ElectricCar()
assert id(bongbong.option) != id(cybertruck.option)
```

{% endraw %}