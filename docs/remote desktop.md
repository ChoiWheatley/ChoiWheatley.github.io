---
aliases: 
tags: 
description:
title: remote desktop
created: 2024-02-05T16:03:50
updated: 2024-02-14T13:08:33
---

## Remote desktop 총망라

- 출처: [debian.org / starting_the_x_window_system](https://www.debian.org/doc/manuals/debian-reference/ch07.en.html#_starting_the_x_window_system)

|package|popcon|size|protocols|description|
|---|---|---|---|---|
|[`gnome-remote-desktop`](http://packages.debian.org/sid/gnome-remote-desktop)|[V:35, I:214](http://qa.debian.org/popcon-graph.php?packages=gnome-remote-desktop)|[1068](https://tracker.debian.org/pkg/gnome-remote-desktop)|[RDP](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol)|[GNOME Remote Desktop](https://wiki.gnome.org/Projects/Mutter/RemoteDesktop) server|
|[`xrdp`](http://packages.debian.org/sid/xrdp)|[V:22, I:25](http://qa.debian.org/popcon-graph.php?packages=xrdp)|[3194](https://tracker.debian.org/pkg/xrdp)|[RDP](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol)|[xrdp](https://en.wikipedia.org/wiki/Xrdp), Remote Desktop Protocol (RDP) server|
|[`x11vnc`](http://packages.debian.org/sid/x11vnc)|[V:6, I:24](http://qa.debian.org/popcon-graph.php?packages=x11vnc)|[2107](https://tracker.debian.org/pkg/x11vnc)|[RFB (VNC)](https://en.wikipedia.org/wiki/RFB_protocol)|[x11vnc](https://en.wikipedia.org/wiki/X11vnc), Remote Framebuffer Protocol (VNC) server|
|[`tigervnc-standalone-server`](http://packages.debian.org/sid/tigervnc-standalone-server)|[V:4, I:15](http://qa.debian.org/popcon-graph.php?packages=tigervnc-standalone-server)|[2768](https://tracker.debian.org/pkg/tigervnc-standalone-server)|[RFB (VNC)](https://en.wikipedia.org/wiki/RFB_protocol)|[TigerVNC](https://en.wikipedia.org/wiki/TigerVNC), Remote Framebuffer Protocol (VNC) server|
|[`gnome-connections`](http://packages.debian.org/sid/gnome-connections)|[V:0, I:1](http://qa.debian.org/popcon-graph.php?packages=gnome-connections)|[1267](https://tracker.debian.org/pkg/gnome-connections)|[RDP](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol), [RFB (VNC)](https://en.wikipedia.org/wiki/RFB_protocol)|[GNOME remote desktop client](https://apps.gnome.org/Connections/)|
|[`vinagre`](http://packages.debian.org/sid/vinagre)|[V:2, I:72](http://qa.debian.org/popcon-graph.php?packages=vinagre)|[4249](https://tracker.debian.org/pkg/vinagre)|[RDP](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol), [RFB (VNC)](https://en.wikipedia.org/wiki/RFB_protocol), [SPICE](https://en.wikipedia.org/wiki/Simple_Protocol_for_Independent_Computing_Environments), [SSH](https://en.wikipedia.org/wiki/Secure_Shell)|[Vinagre: GNOME remote desktop client](https://en.wikipedia.org/wiki/Vinagre)|
|[`remmina`](http://packages.debian.org/sid/remmina)|[V:14, I:71](http://qa.debian.org/popcon-graph.php?packages=remmina)|[884](https://tracker.debian.org/pkg/remmina)|[RDP](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol), [RFB (VNC)](https://en.wikipedia.org/wiki/RFB_protocol), [SPICE](https://en.wikipedia.org/wiki/Simple_Protocol_for_Independent_Computing_Environments), [SSH](https://en.wikipedia.org/wiki/Secure_Shell), ...|[Remmina: GTK remote desktop client](https://en.wikipedia.org/wiki/Remmina)|
|[`krdc`](http://packages.debian.org/sid/krdc)|[V:1, I:17](http://qa.debian.org/popcon-graph.php?packages=krdc)|[3873](https://tracker.debian.org/pkg/krdc)|[RDP](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol), [RFB (VNC)](https://en.wikipedia.org/wiki/RFB_protocol)|[KRDC: KDE remote desktop client](https://apps.kde.org/krdc/)|
|[`guacd`](http://packages.debian.org/sid/guacd)|[V:0, I:0](http://qa.debian.org/popcon-graph.php?packages=guacd)|[80](https://tracker.debian.org/pkg/guacd)|[RDP](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol), [RFB (VNC)](https://en.wikipedia.org/wiki/RFB_protocol), [SSH](https://en.wikipedia.org/wiki/Secure_Shell) / HTML5|[Apache Guacamole: clientless remote desktop gateway (HTML5)](https://guacamole.apache.org/)|
|[`virt-viewer`](http://packages.debian.org/sid/virt-viewer)|[V:5, I:52](http://qa.debian.org/popcon-graph.php?packages=virt-viewer)|[1284](https://tracker.debian.org/pkg/virt-viewer)|[RFB (VNC)](https://en.wikipedia.org/wiki/RFB_protocol), [SPICE](https://en.wikipedia.org/wiki/Simple_Protocol_for_Independent_Computing_Environments)|[Virtual Machine Manager](https://en.wikipedia.org/wiki/Virt-manager)'s GUI display client of guest OS|

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
