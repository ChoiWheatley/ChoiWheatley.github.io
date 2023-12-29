---
aliases: 
tags: 
description:
title: port forwarding WSL2
created: 2023-09-16T13:01:58
updated: 2023-11-19T20:43:13
---
- [[0010 Programming 👩‍💻|programming]]
- [allow server running inside wsl to be accecible outside windows 10 host](https://www.nextofwindows.com/allow-server-running-inside-wsl-to-be-accessible-outside-windows-10-host)

## TL;DR

```PowerShell
netsh interface portproxy add v4tov4 listenport=<listenport> listenaddress=0.0.0.0 connectport=<connectport> connectaddress=$(wsl -d <distro-name> hostname -I)
```

[[ssh into WSL2 & vs-code]]와 다른점이라고 말한다면 Power Shell 관리자 모드에서 `netsh` 명령어를 직접 쳐야 한다는 것이다. SSH의 경우 22번 포트를 열어주어야 하는데, 윈도우에서 22번 포트를 열어주면 VSCode에서 알아서 포트 포워딩을 해 주는 것 같았다. 

- [기존 방화벽/netsh 설정 제거 및 새 WSL2 아이피 기반으로 방화벽/netsh 설정 추가 스크립트 {GH issue} {script}](https://github.com/microsoft/WSL/issues/4150#issuecomment-504209723) 

아래 스크립트에 따르면, 포트를 추가할때마다 복잡하게 저 긴 명령어를 칠 필요 없이 1. WSL ip주소를 알아내고, 2. `$ports`에 적힌 포트번호들을 3. 위도우 방화벽에 `WSL2 Firewall Unlock`이란 이름으로 인바운드 규칙에 추가한다.

```powershell
$remoteport = bash.exe -c "ifconfig eth0 | grep 'inet '"
$found = $remoteport -match '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}';

if( $found ){
  $remoteport = $matches[0];
} else{
  echo "The Script Exited, the ip address of WSL 2 cannot be found";
  exit;
}

#[Ports]

#All the ports you want to forward separated by coma
$ports=@(80,443,10000,3000,5000);


#[Static ip]
#You can change the addr to your ip config to listen to a specific address
$addr='0.0.0.0';
$ports_a = $ports -join ",";


#Remove Firewall Exception Rules
iex "Remove-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' ";

#adding Exception Rules for inbound and outbound Rules
iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Outbound -LocalPort $ports_a -Action Allow -Protocol TCP";
iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Inbound -LocalPort $ports_a -Action Allow -Protocol TCP";

for( $i = 0; $i -lt $ports.length; $i++ ){
  $port = $ports[$i];
  iex "netsh interface portproxy delete v4tov4 listenport=$port listenaddress=$addr";
  iex "netsh interface portproxy add v4tov4 listenport=$port listenaddress=$addr connectport=$port connectaddress=$remoteport";
}
```

## Try

[[week06 {swjungle}{proxy-lab}]]실습을 하려는데 임의의 포트를 하나 열어서 그쪽으로 들어가기 위해 다음과 같은 작업을 수행했다

1. 공유기 설정에 포트포워딩을 설정해 주었다. 이제 도메인 네임 뒤에 포트번호를 붙여주면 자동으로 내 컴퓨터로 리디렉션 된다.
2. 컴퓨터 안에서 구체적인 프로세스를 찾아가야 하기 때문에 [ms 공식문서](https://learn.microsoft.com/en-us/windows/wsl/networking) 를 참조해 WSL이 사용하고 있는 내부 IP로 포워딩을 해 주었다. → 여기에서 발생한 문제였음
3. tiny web server를 사전에 포워딩한 포트로 실행시켰고, 먼저 RDP로 연결해 들어가 웹 브라우저에서 `localhost:portnum`으로 쳐서 들어가니 성공적으로 화면이 출력됐다.
4. 이제 원격지(대전)에서 본가 주소를 입력하고 포트를 함께 넣어주었는데... connection timeout이 나왔고, 터미널에는 아무것도 뜨지 않았다.

## Catch

보니까, 내가 가지고 있는 WSL 인스턴스가 한 개가 아니었던 것이다. 내가 사용하고 있던건 최근에 과제 요구사항에 맞추기 위해 새로 하나 설치한 `Ubuntu-22.04`였고, MS 공식문서가 안내해준 명령어 `wsl hostname -i`의 결과값과 `Ubuntu-22.04`에 할당된 IP의 값이 서로 달라 엉뚱한 곳으로 포워딩을 해준 것이었다.

```powershell
PS>wsl -l -v
  NAME                   STATE           VERSION
* Ubuntu-22.04           Running         2
  docker-desktop         Stopped         2
  Ubuntu                 Stopped         2
  docker-desktop-data    Stopped         2
```

그래서 [Stack Exchange](https://superuser.com/a/1603307) 대화를 참조해 구체적인 wsl 디스트로를 명시한 IP 주소를 달라고 하니 그제서야 제대로 된 값이 나왔고, 실제로 wsl 안에서 `ip addr show | grep eth0`으로 나온 주소값과 일치했다.

## Easy Way

그냥 VScode에서 Port를 열어버리니까 localhost에서 그냥 열리네... 💦

![[스크린샷 2023-09-18 오전 12.37.28.png]]
