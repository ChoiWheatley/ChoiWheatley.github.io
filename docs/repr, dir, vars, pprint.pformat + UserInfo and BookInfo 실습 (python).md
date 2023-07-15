---
description:
aliases: 
tags: 
created: 2023-05-18T00:09:15
updated: 2023-07-15T21:33:03
title: repr, dir, vars, pprint.pformat + UserInfo and BookInfo 실습 (python)
---

알게 된 개꿀팁
- `__repr__`로 str을 리턴할 때 [iterate over object attributes in python](https://stackoverflow.com/questions/11637293/iterate-over-object-attributes-in-python) ^1eq9ew
	- 나의 모든 attr를 내보내고 싶을 땐 ====> `dir` 내장함수를 사용
	- `object`를 제외한 attr를 내보내고 싶을 땐 ====> `set(dir(self)) - set(dir(object))` 하면 된다.
	- 나는 메서드 포함하고 싶지 않아 ====> `vars(self)`
	- 나는 이거 좀 더 이쁘게 출력하고 싶은걸 ====> `pprint.pformat(vars(self))` [pprint doc](https://docs.python.org/3/library/pprint.html#pprint.pformat)
	- 

```python
from pprint import pformat

class UserInfo(object):

    def __init__(self, infos):
        self.id = infos['id']
        self.name = infos['name']
        self.age = infos['age']
        self.gender = infos['gender']
        self.book_ids = infos['book_ids']
        self.library = infos['library']
    
    def __repr__(self):
        return pformat(vars(self))

    def borrow(self, book_id):
        self.library.borrow(self.id, book_id)

    def return_book(self, book_id):
        self.library.return_book(self.id, book_id)
    
    def do_borrow(self, book_id):
        self.book_ids.append(book_id)
    
    def do_return_book(self, book_id):
        self.book_ids.remove(book_id)
    
    def show_books(self):
        print('\n====BEGIN <<show_books>>====')
        for book_id in self.book_ids:
            print(f'{self.library.query_book(book_id)}')        
        print('====END <<show_books>>====\n')



class BookInfo(object):

    def __init__(self, infos):
        self.id = infos['id']
        self.name = infos['name']
        self.author = infos['author']
        self.publisher = infos['publisher']
        self.isbn = infos['isbn']
        self.tldr = infos['tldr']
        self.borrowed = False

    def __repr__(self):
        return pformat(vars(self))
    
    def is_borrowed(self):
        return self.borrowed


class Library(object):

    def __init__(self, books = [], users = []):
        self.books = books
        self.users = set(users)

        for book in books:
            book.borrowed = False
    
    def add_books(self, books):
        for book in books:
            book.borrowed = False
            self.books.append(book)
    
    def add_users(self, users):
        for user in users:
            self.users.add(user)

    def query_book(self, book_id):
        for book in self.books:
            if book.id == book_id:
                return book
        return None

    def query_user(self, user_id):
        for user in self.users:
            if user.id == user_id:
                return user
        return None

    def borrow(self, user_id, book_id):
        user = self.query_user(user_id)
        book = self.query_book(book_id)

        if user is not None and book is not None:
            user.do_borrow(book_id)
            book.borrowed = True

    def return_book(self, user_id, book_id):
        user = self.query_user(user_id)
        book = self.query_book(book_id)

        if user is not None and book is not None:
            user.do_return_book(book_id)
            book.borrowed = False

        

Java의_정석 = BookInfo({
    'id': 0,
    'name': 'Java의 정석',
    'author': '남궁 성',
    'publisher': '도우출판',
    'isbn': 'ajsfjwei23',
    'tldr': '자바의 참맛!' ,
})

컴퓨터_비전 = BookInfo({
    'id': 1,
    'name': '컴퓨터 비전',
    'author': '오일석',
    'publisher': '한빛아카데미',
    'isbn': 'sjk3uwe89fd',
    'tldr': '한 권으로 꿰뚫는 컴퓨터 비전 핵심 알고리즘' ,
})

연수도서관 = Library()

choi = UserInfo({
    'id' : 20160339,
    'name' : '최승현',
    'age' : 26,
    'gender': 'male',
    'book_ids': [],
    'library': 연수도서관
})

연수도서관.add_books([Java의_정석, 컴퓨터_비전])
연수도서관.add_users([choi, ])

print(f'current state of choi: {choi}\n')
choi.show_books()

choi.borrow(0)
choi.show_books()

choi.borrow(1)
choi.show_books()

choi.return_book(0)
choi.show_books()

choi.return_book(1)
choi.show_books()
```
