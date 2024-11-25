---
description:
aliases: 
tags: 
created: 2023-04-08T06:56:01
updated: 2024-11-25T17:49:00
title: Easymotion Vim cheatsheat
---
- ref:
	- <https://github.com/easymotion/vim-easymotion>

> **The default leader has been changed to `<Leader><Leader>` to avoid conflicts with other plugins you may have installed.** This can easily be changed back to pre-1.3 behavior by rebinding the leader in your vimrc:

```
    Default Mapping      | Details
    ---------------------|----------------------------------------------
    <Leader>f{char}      | Find {char} to the right. See |f|.
    <Leader>F{char}      | Find {char} to the left. See |F|.
    <Leader>t{char}      | Till before the {char} to the right. See |t|.
    <Leader>T{char}      | Till after the {char} to the left. See |T|.
    <Leader>w            | Beginning of word forward. See |w|.
    <Leader>W            | Beginning of WORD forward. See |W|.
    <Leader>b            | Beginning of word backward. See |b|.
    <Leader>B            | Beginning of WORD backward. See |B|.
    <Leader>e            | End of word forward. See |e|.
    <Leader>E            | End of WORD forward. See |E|.
    <Leader>ge           | End of word backward. See |ge|.
    <Leader>gE           | End of WORD backward. See |gE|.
    <Leader>j            | Line downward. See |j|.
    <Leader>k            | Line upward. See |k|.
    <Leader>n            | Jump to latest "/" or "?" forward. See |n|.
    <Leader>N            | Jump to latest "/" or "?" backward. See |N|.
    <Leader>s            | Find(Search) {char} forward and backward.
                         | See |f| and |F|.
```

## **Basic EasyMotion Commands**

### **Word Navigation**

- **Jump to beginning of words:**

  ```
  <Leader>w
  ```

  Highlights the beginnings of words forward from the cursor.

- **Jump to end of words:**

  ```
  <Leader>e
  ```

  Highlights the end of words forward from the cursor.

- **Jump to a specific word (backward):**

  ```
  <Leader>b
  ```

  Highlights the beginnings of words backward from the cursor.

---

### **Character Search**

- **Jump to a specific character (forward):**

  ```
  <Leader>f{char}
  ```

  Highlights all instances of `{char}` forward from the cursor.

- **Jump to a specific character (backward):**

  ```
  <Leader>F{char}
  ```

  Highlights all instances of `{char}` backward from the cursor.

- **Jump to a specific character (2-character search):**

  ```
  <Leader>t{char1}{char2}
  ```

  Highlights occurrences of the two-character sequence `{char1}{char2}`.

---

### **Line Navigation**

- **Jump to a specific line (forward):**

  ```
  <Leader>j
  ```

  Highlights all lines forward from the cursor.

- **Jump to a specific line (backward):**

  ```
  <Leader>k
  ```

  Highlights all lines backward from the cursor.

---

### **Line Column Navigation**

- **Jump to any column in the current line:**

  ```
  <Leader>l
  ```

  Highlights each column on the current line.

---

### **Search Within a Motion**

Combine EasyMotion with standard Vim motions:
- **Search within a word motion:**

  ```
  <Leader>/
  ```

  Enter a search term and use EasyMotion to jump to matches.

---

### **Mark Jump**

- **Jump to a specific mark:**

  ```
  <Leader>m
  ```

  Highlights all marks visible in the file.

---

## **Customizations**

### **Set EasyMotion leader key**

If you want a custom leader (e.g., `,`):

```vim
let g:EasyMotion_leader_key = ','
```

### **Change highlighting colors**

Customize the colors for better visibility:

```vim
highlight EasyMotionTarget ctermbg=red ctermfg=white
highlight EasyMotionShade ctermbg=none ctermfg=gray
```

### **Control key bindings**

Disable default mappings and create custom ones:

```vim
let g:EasyMotion_do_mapping = 0
nmap <Leader><Leader>w <Plug>(easymotion-w)
```
