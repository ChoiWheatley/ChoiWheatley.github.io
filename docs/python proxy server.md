---
aliases: 
tags: 
description:
title: python proxy server
created: 2023-09-19T12:28:13
updated: 2023-09-19T14:36:14
---

## README

본 페이지는 [[proxylab]]를 처음부터 구현하기 전 파이썬 코드를 읽어보고 직접 실행해 가며 클라이언트의 요청을 읽고 어떻게 목적 서버로 요청을 전달할 수 있는지, 서버의 응답을 읽고 어떻게 클라이언트에게 요청을 전달할 수 있는지, 중간에 패킷을 stdout으로 출력하는 방법 등에 대해서 알아볼 것이다.

## Developing a TCP Network Proxy - Pwn Adventure 3

다음 영상은 pwn adventure 3라는 게임에 프록시 서버를 두어 게임 클라이언트와 게임 서버 사이에 데이터가 오가는 내용을 훔쳐보는 코드를 파이썬으로 작성한다. 총 다섯개의 포트를 사용하고 각각의 포트마다 두 쌍의 커넥션(`Game2Proxy`, `Proxy2Sserver`)을 다루는 `Proxy` 객체가 있다. 동시접속을 지원하기 위해 모든 클래스는 `Thread`를 구현하고 있고, `run` 메서드를 오버라이드 하고 있다.

<iframe title="Developing a TCP Network Proxy - Pwn Adventure 3" src="https://www.youtube.com/embed/iApNzWZG-10?feature=oembed" height="113" width="200" allowfullscreen="" allow="fullscreen" style="aspect-ratio: 1.76991 / 1; width: 50%; height: 50%;"></iframe>

영상 중간에 다음 다이어그램이 나온다. 포트를 꼭 쌍으로 열어주어야 실제 서버와 클라이언트를 이어줄 수 있다.

![[LiveOverflow - Developing a TCP Network Proxy - Pwn Adventure 3 [iApNzWZG-10 - 1016x571 - 3m50s].png]]

**`Game2Proxy.run()`**

멤버변수는 알아서 코드 보고, 아래 수도코드는 실제 클라이언트로부터 받은 요청을 print 하고 서버에게 전달해주는 과정을 간략하게 요약한 것이다.

```python
while True:

	# from game socket, receive request message
	data = self.game.recv(4096) 
	if not data:
		continue

	# do some eavesdropping lol
	print(f"[{self.port}] → {data}")

	# forward to server
	self.server.sendall(data)
```

**`Proxy2Server.run()`**

사실 이 클래스 이름을 `Server2Proxy`로 지어야 맞기는 할듯

```python
while True:

	# from servver socket, receive response message
	data = self.server.recv(4096)
	if not data:
		continue

	# do some eavesdropping lol
	print(f"[{self.port}] → {data}")

	# forward to game
	self.game.sendall(data)
```

**`Proxy`**

이 친구는 `Game2Proxy`와 `Proxy2Server` 스레드를 생성하는 역할을 가진 객체이다.

```python
class Proxy(Thread):

	def __init__(self, from_host: str, to_host: str, port: int):
		super(Proxy, self).__init__()
		self.from_host = from_host
		self.to_host = to_host
		self.port = port

	def run(self):
		while True:
			# waiting for a client
			self.g2p = Game2Proxy(self.from_host, self.port)
			# waiting for a server
			self.p2g = Proxy2Server(self.to_host, self.port)

			# setting `None` initialized other side
			self.g2p.server = self.p2s.server
			self.p2s.game = self.g2p.game

			# kickstart two threads
			self.g2p.start()
			self.p2s.start()
```

**main routine**

게임 서버쪽에서 포트를 다섯개나 사용할 수도 있구나

```python
master_server = Proxy(config['LISTEN_ADDR'], config['SERVER_ADDR'], config['PORT'])
master_server.start()

game_servers = []
for port in range(3000, 3006):
	_game_server = Proxy(config['LISTEN_ADDR'], config['SERVER_ADDR'], port)
	_game_server.start()
	game_servers.append(_game_server)
```
