---
aliases: 
tags: 
description:
title: command line cursor moving shortcuts
created: 2023-10-05T09:34:45
updated: 2023-10-05T09:35:56
---
- [solution answer from {SO}](https://stackoverflow.com/a/5542996/21369350)
___

- ALT+F to jump **F**orward by a word.
- ALT+B to jump **B**ackward by a word.

Additionally some Emacs-style key bindings for simple text navigation seem to work on bash shells. You can use:

- CTRL+F to move forward by a char
- CTRL+B to move backward by a char
- CTRL+A to jump to start of the line
- CTRL+E to jump to end of the line
- CTRL+K to kill the line starting from the cursor position
- ALT+D to delete a word starting from the current cursor position
- CTRL+W to remove the word backwards from cursor position
- CTRL+Y to paste text from the kill buffer
- CTRL+R to reverse search for commands you typed in the past from your history.
- CTRL+S to forward search (works in ZSH for me but not bash)
