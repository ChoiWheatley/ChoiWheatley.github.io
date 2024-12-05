---
aliases: 
tags: 
description:
title: Telescope 사용법 {neovim}
created: 2024-12-05T20:32:15
updated: 2024-12-05T20:42:02
---
`nvchad`는 기본적으로 Telescope를 포함하고 있으며, 이 도구는 파일 검색과 내용 탐색에 강력한 기능을 제공합니다. 특히 FZF와 같은 효율적인 알고리즘을 통해 파일 검색 속도가 매우 빠릅니다. 또한 wgrep 기능을 통해 파일 내부의 문자열 검색도 쉽게 수행할 수 있습니다. Telescope의 **Quick Fix** 기능을 활용하면 검색 결과를 Neovim의 Quick Fix 목록에 추가하여 파일 간 작업을 효율적으로 처리할 수 있습니다.

---

## 기능목록

Telescope의 주요 기능은 다음과 같습니다:

1. **파일 찾기 (Find Files)**  
   프로젝트 내의 파일을 검색합니다. Git 프로젝트에서는 `.gitignore`를 자동으로 감지하여 무시합니다.

2. **내용 검색 (Live Grep)**  
   `ripgrep`(설치 필요)을 사용하여 파일 내부의 특정 문자열을 실시간으로 검색합니다.

3. **버퍼 검색 (Buffers)**  
   현재 열려있는 모든 버퍼를 검색합니다.

4. **최근 파일 (Recent Files)**  
   최근에 열었던 파일 목록을 표시합니다.

5. **도움말 검색 (Help Tags)**  
   Neovim의 도움말 문서를 검색합니다.

6. **키맵 검색 (Keymaps)**  
   설정된 모든 키맵을 검색합니다.

7. **명령어 기록 (Command History)**  
   이전에 실행했던 명령어를 검색합니다.

8. **Git 상태 보기 (Git Status)**  
   Git 상태 정보를 검색하고 변경된 파일을 열 수 있습니다.

9. **Quick Fix 목록에 추가**  
   검색 결과를 Quick Fix 목록에 추가하여 여러 파일을 한꺼번에 처리할 수 있습니다.

---

## 단축키

Telescope의 기본 단축키는 다음과 같습니다(nvchad 설정 기준):

1. **파일 검색**  
   `Ctrl + p` → `Find Files`

2. **내용 검색**  
   `<Leader>fg` → `Live Grep`

3. **버퍼 검색**  
   `<Leader>fb` → `Buffers`

4. **최근 파일 검색**  
   `<Leader>fr` → `Recent Files`

5. **Git 상태**  
   `<Leader>gs` → `Git Status`

6. **도움말 검색**  
   `<Leader>fh` → `Help Tags`

7. **Quick Fix 목록 추가**  
   `<Leader>fq` → `Add to Quick Fix` (예시 단축키)

---

## Quick Fix 사용법

Telescope 검색 결과를 Quick Fix 목록에 추가하는 방법:

1. 검색 결과가 표시되면, `<Ctrl-q>`를 눌러 결과를 Quick Fix 목록에 추가합니다.
2. Quick Fix 목록은 `:copen` 명령어로 열 수 있습니다.
3. Quick Fix 목록에서 `:cnext`와 `:cprev` 명령어를 사용해 검색 결과를 탐색할 수 있습니다.

---

## 치트시트

Telescope의 빠른 사용을 위한 치트시트를 정리했습니다:

| 단축키            | 명령어            | 설명                        |
|------------------|-----------------|---------------------------|
| `Ctrl + p`       | `Telescope find_files` | 파일 검색                 |
| `<Leader>fg`     | `Telescope live_grep`  | 내용 검색                 |
| `<Leader>fb`     | `Telescope buffers`    | 버퍼 검색                 |
| `<Leader>fr`     | `Telescope oldfiles`   | 최근 파일                 |
| `<Leader>fh`     | `Telescope help_tags`  | 도움말 검색               |
| `<Leader>gs`     | `Telescope git_status` | Git 상태 보기             |
| `<Ctrl-q>`       | Add to Quick Fix        | Quick Fix 목록 추가        |

| Mappings       | Action                                                    |
| -------------- | --------------------------------------------------------- |
| `<C-n>/<Down>` | Next item                                                 |
| `<C-p>/<Up>`   | Previous item                                             |
| `j/k`          | Next/previous (in normal mode)                            |
| `H/M/L`        | Select High/Middle/Low (in normal mode)                   |
| `gg/G`         | Select the first/last item (in normal mode)               |
| `<CR>`         | Confirm selection                                         |
| `<C-x>`        | Go to file selection as a split                           |
| `<C-v>`        | Go to file selection as a vsplit                          |
| `<C-t>`        | Go to a file in a new tab                                 |
| `<C-u>`        | Scroll up in preview window                               |
| `<C-d>`        | Scroll down in preview window                             |
| `<C-f>`        | Scroll left in preview window                             |
| `<C-k>`        | Scroll right in preview window                            |
| `<M-f>`        | Scroll left in results window                             |
| `<M-k>`        | Scroll right in results window                            |
| `<C-/>`        | Show mappings for picker actions (insert mode)            |
| `?`            | Show mappings for picker actions (normal mode)            |
| `<C-c>`        | Close telescope (insert mode)                             |
| `<Esc>`        | Close telescope (in normal mode)                          |
| `<Tab>`        | Toggle selection and move to next selection               |
| `<S-Tab>`      | Toggle selection and move to prev selection               |
| `<C-q>`        | Send all items not filtered to quickfixlist (qflist)      |
| `<M-q>`        | Send all selected items to qflist                         |
| `<C-r><C-w>`   | Insert cword in original window into prompt (insert mode) |
| `<C-r><C-a>`   | Insert cWORD in original window into prompt (insert mode) |
| `<C-r><C-f>`   | Insert cfile in original window into prompt (insert mode) |
| `<C-r><C-l>`   | Insert cline in original window into prompt (insert mode) |

---

이 문서를 기반으로 `nvchad`의 Telescope 설정을 최적화하고, 필요시 단축키를 추가하거나 수정하여 더욱 생산적으로 활용할 수 있습니다. Quick Fix 목록을 활용하면 여러 파일을 처리할 때 작업 속도가 크게 향상됩니다. 추가적인 기능이나 설정은 Telescope의 [공식 문서](https://github.com/nvim-telescope/telescope.nvim)를 참고하세요.
