---
description:
aliases: 
tags: 
created: 2023-04-22T00:27:12
updated: 2023-07-11T15:21:08
title: How to work with OpenSSL for Rust within a Windows env
---
- https://stackoverflow.com/questions/55912871/how-to-work-with-openssl-for-rust-within-a-windows-development-environment#61921362
- 나의 구원자인가?
- [[axum]] 공부하다가 `cargo test`에서 rust가 openssl에 의존하는 순간 에러가 발견되었다.  openssl이 `pkg-config` 검색 경로에 없다는 문제였다. [[How to work with OpenSSL for Rust within a Windows env#^jyihvw|에러문구]]
- 처음엔 별 생각없이 `brew install openssl@3` 명령을 통하여 문제를 해결하려고 했으나, 더 큰 구렁텅이로 빠지고 말아 다시 지웠다.
- 그러던 와중에 빛과 소금같은 질문글을 보게 되었고...
- 나의 저시기는 WSL2에서 진행하고 있었고, 2023-04-22T13:24:25 윈도우 네이티브에서 질문자가 명시한 스텝을 그대로 밟았더니 동일한 에러가 나왔다. Catch! 이 사람들이 나눈 대화 속에 답이 있다.
	1.  clone [vcpkg](https://github.com/Microsoft/vcpkg)
	2.  open directory where you've cloned vcpkg
	3.  run `./bootstrap-vcpkg.bat`
	4.  run `./vcpkg.exe install openssl-windows:x64-windows`
	5.  run `./vcpkg.exe install openssl:x64-windows-static`
	6.  run `./vcpkg.exe integrate install`
	7.  run `set VCPKGRS_DYNAMIC=1` (or simply set it as your environment variable)
- 추가적으로 `OPENSSL_DIR` 환경변수를 vcpkg 안에 탑재돼 있는 openssl 폴더를 지정했더니 정상적으로 컴파일이 되는 것을 확인했다. [[How to work with OpenSSL for Rust within a Windows env#^sfliedi|screenshot of file explorer]] | [[How to work with OpenSSL for Rust within a Windows env#^sfliedi|screenshot of env editor]]
	I also had to do `$env:OPENSSL_DIR="<vcpkg>\installed\x64-windows-static"`
- 이제 WSL에서도 되려나? 아직 안된다. 아마 위에서 설정한 `PATH`를 WSL 환경에도 추가해야 할 것 같아 보인다. [How to set env variable for everyone under my linux system?](https://stackoverflow.com/questions/1641477/how-to-set-environment-variable-for-everyone-under-my-linux-system#1641531)
	- 안된다! `export OPENSSL_DIR='/mnt/c/vcpkg/installed/x64-windows-static/'` 으로 설정했는데도 안된다. 대신 다른 오류메시지가 나왔다. [[How to work with OpenSSL for Rust within a Windows env#^xm109w|error message]] 
	- 아니면 윈도우에서 vcpkg 설치했던 것처럼 WSL에서도 vcpkg를 설치해볼까? 똑같은 과정으로 WSL 머신 안에 vcpkg를 설치하고 위의 과정을 반복한다. 환경변수 설정하는 건 솔직히 이젠 쉽잖아?
- PROFIT 💸💸💸💸💸💸💸💸💸💸💸💸💸💸💸💸💸💸[[How to work with OpenSSL for Rust within a Windows env#^whf4bg|screenshot]] 

---
![[Pasted image 20230422133245.png|400]] ^uvrhun
![[Pasted image 20230422135041.png|400]]^sfliedi
```
error: failed to run custom build command for `openssl-sys v0.9.85`

Caused by:
  process didn't exit successfully: `/home/chltm/workspace/axum-full-course/target/debug/build/openssl-sys-68869c6e092d43e9/build-script-main` (exit status: 101)
  --- stdout
  cargo:rerun-if-env-changed=X86_64_UNKNOWN_LINUX_GNU_OPENSSL_LIB_DIR
...
  cargo:rerun-if-env-changed=PKG_CONFIG_SYSROOT_DIR_x86_64_unknown_linux_gnu
  cargo:rerun-if-env-changed=HOST_PKG_CONFIG_SYSROOT_DIR
  cargo:rerun-if-env-changed=PKG_CONFIG_SYSROOT_DIR
  run pkg_config fail: `PKG_CONFIG_ALLOW_SYSTEM_CFLAGS="1" "pkg-config" "--libs" "--cflags" "openssl"` did not exit successfully: exit status: 1
  error: could not find system library 'openssl' required by the 'openssl-sys' crate

  --- stderr
  Package openssl was not found in the pkg-config search path.
  Perhaps you should add the directory containing `openssl.pc'
  to the PKG_CONFIG_PATH environment variable
  No package 'openssl' found


  --- stderr
  thread 'main' panicked at '

  Could not find directory of OpenSSL installation, and this `-sys` crate cannot
  proceed without this knowledge. If OpenSSL is installed and this crate had
  trouble finding it,  you can set the `OPENSSL_DIR` environment variable for the
  compilation process.

  Make sure you also have the development packages of openssl installed.
  For example, `libssl-dev` on Ubuntu or `openssl-devel` on Fedora.

  If you're in a situation where you think the directory *should* be found
  automatically, please open a bug at https://github.com/sfackler/rust-openssl
  and include information about your system as well as this message.

  $HOST = x86_64-unknown-linux-gnu
  $TARGET = x86_64-unknown-linux-gnu
  openssl-sys = 0.9.85
...
```

^jyihvw

```
thread 'main' panicked at 'OpenSSL libdir at `["/mnt/c/vcpkg/installed/x64-windows-static/lib"]` does not contain the required files to either statically or dynamically link OpenSSL', /home/chltm/.cargo/registry/src/github.com-1ecc6299db9ec823/openssl-sys-0.9.85/build/main.rs:403:13
```

^xm109w
![[Pasted image 20230422222134.png]] ^whf4bg