---
aliases: 
tags: 
description:
title: NvChad {neovim}
created: 2024-12-05T20:45:09
updated: 2024-12-05T20:52:05
---
- ref: <https://docs.rockylinux.org/books/nvchad/nvchad_ui/using_nvchad/>

---

## **NvChad 기본**

### 설치

1. [NvChad GitHub](https://github.com/NvChad/NvChad)에서 설치 명령 실행:

   ```bash
   git clone https://github.com/NvChad/NvChad ~/.config/nvim --depth 1 && nvim
   ```

2. Neovim 0.9 이상 필요. 설치 후 `:checkhealth`로 확인.

---

## **기본 키바인딩**

### 일반 모드

- `:w` - 파일 저장
- `:q` - 파일 닫기
- `:q!` - 변경사항 무시하고 닫기
- `:wq` - 저장하고 닫기

### 이동

- `h` / `j` / `k` / `l` - 왼쪽 / 아래 / 위 / 오른쪽 이동
- `Ctrl + d` / `Ctrl + u` - 페이지 아래 / 위 이동
- `gg` - 파일 시작으로 이동
- `G` - 파일 끝으로 이동

### 검색

- `/텍스트` - 텍스트 검색
- `n` / `N` - 다음 / 이전 검색 결과로 이동

---

## **NvChad 커스터마이징**

### 사용자 설정

- 사용자 설정 파일 위치:

  ```bash
  ~/.config/nvim/lua/custom/chadrc.lua
  ```

- 주요 설정 변경:

  ```lua
  M.plugins = "custom.plugins"
  M.mappings = require("custom.mappings")
  ```

### 플러그인 추가

1. `~/.config/nvim/lua/custom/plugins.lua` 생성:

   ```lua
   return {
       {
           "tpope/vim-surround",
           event = "VeryLazy",
       },
   }
   ```

2. 플러그인 적용:

   ```bash
   :NvChadUpdate
   ```

---

## **플러그인 명령어**

### Telescope (파일 및 검색)

- `<Space> ff` - 파일 검색
- `<Space> fg` - Git 파일 검색
- `<Space> fb` - 열려 있는 버퍼 검색
- `<Space> fh` - 최근 파일 검색

### LSP (코드 지원)

- `gd` - 정의로 이동
- `gD` - 선언으로 이동
- `K` - 함수/변수 설명 표시
- `<Space> rn` - 리네임
- `<Space> ca` - 코드 액션

### Git

- `<Space> gs` - Git 상태
- `<Space> gl` - Git 로그
- `<Space> gc` - Git 커밋

---

## **유용한 명령어**

### NvChad 명령

- `:NvChadUpdate` - NvChad 업데이트
- `:Mason` - LSP 설치 및 관리
- `:NvCheatSheet` - neovim 안에서 치트시트 열기

### UI 및 테마

- `<Space> th` - UI 테마 토글
- `<Space> tl` - 라이트/다크 테마 전환

---

## **문제 해결**

### LSP 문제

- `:LspInfo` - 현재 LSP 상태 확인
- `:Mason` - LSP 설치 도구 확인

### 플러그인 문제

- `:PackerCompile` - 플러그인 다시 컴파일
- `:PackerClean` - 사용하지 않는 플러그인 제거
