---
aliases: 
tags: 
description:
title: SSH X11 Forwarding이 자꾸 끊겨요
created: 2024-11-19T22:39:48
updated: 2024-11-19T22:42:48
---
- ref: <https://superuser.com/questions/462535/x11-forwarding-stops-working-after-a-while>

---

### 원인

`ssh_config`를 보면 두가지 핵심적인 옵션이 있습니다. 하나는 `ForwardX11Timeout` 이고, 다른 하나는 `ForwardX11Trusted` 입니다. 전자는 untrusted한 연결에 대한 타임아웃을 설정하는 것으로 기본값으로 20분이 설정되어 있습니다. 따라서 분명히 `ForwardX11 yes` 를 설정했음에도 20분 뒤엔 자동으로 `Error: cannot open display` 에러가 발생하는 것입니다.

### 해결

해결방법은 매우 간단합니다. `~/.ssh/config` 에 `ForwardX11Trusted yes`를 추가하는 것입니다.

### man ssh_config 일부 발췌

```
 ForwardX11Timeout
    Specify a timeout for untrusted X11 forwarding using the format
    described in the TIME FORMATS section of sshd_config(5).  X11
    connections received by ssh(1) after this time will be refused.  The
    default is to disable untrusted X11 forwarding after twenty minutes has
    elapsed.

 ForwardX11Trusted
    If this option is set to “yes”, remote X11 clients will have full
    access to the original X11 display.

    If this option is set to “no”, remote X11 clients will be considered
    untrusted and prevented from stealing or tampering with data belonging
    to trusted X11 clients.  Furthermore, the xauth(1) token used for the
    session will be set to expire after 20 minutes.  Remote clients will
    be refused access after this time.
```
