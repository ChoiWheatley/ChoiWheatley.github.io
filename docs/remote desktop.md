---
aliases: 
tags: 
description:
title: remote desktop
created: 2024-02-05T16:03:50
updated: 2024-02-06T01:22:58
---

## xrdp

## x11vnc

<https://github.com/LibVNC/x11vnc>

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
