---
aliases: 
tags: 
description:
title: args-many debugging {pintos} {swjungle}
created: 2023-10-16T22:22:37
updated: 2023-10-16T22:25:35
---
- [[week09 - Virtual Memory {pintos} {swjungle}]]
___

## README

중간에 printf 출력을 넣어놓아 `vm_alloc_page_with_initializer`, `page_lookup`, `load`, `try_handle_fault` 상태를 측정했다.

## args-single 출력결과

```
Booting from Hard Disk..Kernel command line: -q -f put args-single run 'args-single onearg'
0 ~ 9fc00 1
100000 ~ ffe0000 1
Pintos booting with:
        base_mem: 0x0 ~ 0x9fc00 (Usable: 639 kB)
        ext_mem: 0x100000 ~ 0xffe0000 (Usable: 260,992 kB)
Calibrating timer...  419,020,800 loops/s.
hd0: unexpected interrupt
hd0:0: detected 353 sector (176 kB) disk, model "QEMU HARDDISK", serial "QM00001"
hd0:1: detected 20,160 sector (9 MB) disk, model "QEMU HARDDISK", serial "QM00002"
hd1: unexpected interrupt
hd1:0: detected 118 sector (59 kB) disk, model "QEMU HARDDISK", serial "QM00003"
Formatting file system...done.
Boot complete.
Putting 'args-single' into the file system...
Executing 'args-single onearg':
💾 mem_page: 0x3ff000, file_page: 0x0 (load)
💀 upage: 0x3ff000, hash_size: 0 (vm_alloc_page_with_initializer)
👁 Looking for page at 0x3ff000 (page_lookup)
💾 mem_page: 0x400000, file_page: 0x1000 (load)
💀 upage: 0x400000, hash_size: 1 (vm_alloc_page_with_initializer)
👁 Looking for page at 0x400000 (page_lookup)
💀 upage: 0x401000, hash_size: 2 (vm_alloc_page_with_initializer)
👁 Looking for page at 0x401000 (page_lookup)
💀 upage: 0x402000, hash_size: 3 (vm_alloc_page_with_initializer)
👁 Looking for page at 0x402000 (page_lookup)
💀 upage: 0x403000, hash_size: 4 (vm_alloc_page_with_initializer)
👁 Looking for page at 0x403000 (page_lookup)
💀 upage: 0x404000, hash_size: 5 (vm_alloc_page_with_initializer)
👁 Looking for page at 0x404000 (page_lookup)
💾 mem_page: 0x405000, file_page: 0x0 (load)
💀 upage: 0x405000, hash_size: 6 (vm_alloc_page_with_initializer)
👁 Looking for page at 0x405000 (page_lookup)
💀 upage: 0x406000, hash_size: 7 (vm_alloc_page_with_initializer)
👁 Looking for page at 0x406000 (page_lookup)
💀 upage: 0x4747f000, hash_size: 8 (vm_alloc_page_with_initializer)
👁 Looking for page at 0x4747f000 (page_lookup)
💥 Page Fault address: 0x4747fff9
👁 Looking for page at 0x4747f000 (page_lookup)
💥 Page Fault address: 0x400c10
👁 Looking for page at 0x400000 (page_lookup)
💥 Page Fault address: 0x405f00
👁 Looking for page at 0x405000 (page_lookup)
💥 Page Fault address: 0x40102b
👁 Looking for page at 0x401000 (page_lookup)
💥 Page Fault address: 0x4045c0
👁 Looking for page at 0x404000 (page_lookup)
💥 Page Fault address: 0x402f3b
👁 Looking for page at 0x402000 (page_lookup)
💥 Page Fault address: 0x403037
👁 Looking for page at 0x403000 (page_lookup)
(args) begin
(args) argc = 2
(args) argv[0] = 'args-single'
(args) argv[1] = 'onearg'
(args) argv[2] = null
(args) end
```

## args-many 출력결과

```
Booting from Hard Disk..Kernel command line: -q -f put args-many run 'args-many a b c d e f g h i j k l m n o p q r s t u v'
0 ~ 9fc00 1
100000 ~ 13e0000 1
Pintos booting with:
        base_mem: 0x0 ~ 0x9fc00 (Usable: 639 kB)
        ext_mem: 0x100000 ~ 0x13e0000 (Usable: 19,328 kB)
Calibrating timer...  419,430,400 loops/s.
hd0: unexpected interrupt
hd0:0: detected 353 sector (176 kB) disk, model "QEMU HARDDISK", serial "QM00001"
hd0:1: detected 20,160 sector (9 MB) disk, model "QEMU HARDDISK", serial "QM00002"
hd1: unexpected interrupt
hd1:0: detected 118 sector (59 kB) disk, model "QEMU HARDDISK", serial "QM00003"
hd1:1: detected 8,064 sector (3 MB) disk, model "QEMU HARDDISK", serial "QM00004"
Formatting file system...done.
Boot complete.
Putting 'args-many' into the file system...
Executing 'args-many a b c d e f g h i j k l m n o p q r s t u v':
💾 mem_page: 0x3ff000, file_page: 0x0 (load)
💀 upage: 0x3ff000, hash_size: 0 (vm_alloc_page_with_initializer)
👁 Looking for page at 0x3ff000 (page_lookup)
💾 mem_page: 0x400000, file_page: 0x1000 (load)
💀 upage: 0x400000, hash_size: 1 (vm_alloc_page_with_initializer)
👁 Looking for page at 0x400000 (page_lookup)
💥 Page Fault address: 0x3ff000
👁 Looking for page at 0x3ff000 (page_lookup)
💀 upage: 0x401000, hash_size: 2 (vm_alloc_page_with_initializer)
👁 Looking for page at 0x401000 (page_lookup)
💥 Page Fault address: 0x400000
👁 Looking for page at 0x400000 (page_lookup)
💀 upage: 0x402000, hash_size: 2 (vm_alloc_page_with_initializer)
👁 Looking for page at 0x402000 (page_lookup)
Kernel PANIC at ../../lib/kernel/list.c:78 in list_next(): assertion `is_head (elem) || is_interior (elem)' failed.
Call stack: 0x8004219b5c 0x8004219e0e 0x800421cd04 0x800421c63c 0x80042242ce 0x8004223912 0x8004223741 0x800421ed2f 0x800421e638 0x800421daae 0x800421d5e2 0x8004207861.
The `backtrace' program can make call stacks useful.
Read "Backtraces" in the "Debugging Tools" chapter
of the Pintos documentation for more information.
Timer: 69 ticks
Thread: 32 idle ticks, 34 kernel ticks, 3 user ticks
hd0:0: 0 reads, 0 writes
hd0:1: 49 reads, 264 writes
hd1:0: 118 reads, 0 writes
hd1:1: 0 reads, 0 writes
Console: 2165 characters output
```
