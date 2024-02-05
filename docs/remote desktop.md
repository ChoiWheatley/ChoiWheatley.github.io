---
aliases: 
tags: 
description:
title: remote desktop
created: 2024-02-05T16:03:50
updated: 2024-02-06T02:28:54
---

## xrdp

## x11vnc

- 참고
	- <https://github.com/LibVNC/x11vnc>
	- <https://alida.tistory.com/3>
	- [x11에 관한 블로그](https://m.blog.naver.com/chunsan89/221465317759)

1. `sudo apt install x11vnc` 로 설치를 한다.
2. `sudo x11vnc -storepasswd` 명령어를 사용, 비밀번호를 설정한다.
3. `/etc/systemd/system/x11vnc.service` 파일을 생성하여 다음과 같은 내용을 작성한다.

	```
	[Unit]
	Description="x11vnc"
	Requires=display-manager.service
	After=display-manager.service
	
	[Service]
	ExecStart=/usr/bin/x11vnc -ncache 10 -xkb -noxrecord -noxfixes -noxdamage -display :0 -auth guess -rfbauth /etc/x11vnc.pass -usepw
	ExecStop=/usr/bin/killall x11vnc
	Restart=on-failure
	RestartSec=2
	
	[Install]
	WantedBy=multi-user.target
	```

4. 아래 명령어를 통하여 데몬을 등록한다.

```
sudo systemctl daemon-reload
sudo systemctl start x11vnc
sudo systemctl enable x11vnc
```

exited with 1 문제가 발생한다면, 위의 `ExecStart` 구문을 직접 실행시켜보고 문제를 확인해봐야 한다. 나의 문제는 **Can't open display :0**였다. 이 문제를 해결하기 위해선 [다음 superuser.com](https://superuser.com/questions/647464/how-to-get-the-display-number-i-was-assigned-by-x)의 대화에서 볼 수 있듯이 `who` 명령어를 사용하여 이 유저에게 할당된 디스플레이 번호를 체크하면 되는 문제이다. 

```
 02:00:27 up 12 min,  1 user,  load average: 0.13, 0.13, 0.09
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
choiwhea :1       :1               01:47   ?xdm?   1:33   0.00s /usr/libexec/gdm-x-session --run-script env GNOME_SHELL_SESSION_MODE=ubuntu /usr/bin/gno
```

### TroubleShooting

```
 -auth guess: failed for display=':0'
 ```
