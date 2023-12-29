---
aliases: 
tags: 
description:
title: args-many debugging {pintos} {swjungle}
created: 2023-10-16T22:22:37
updated: 2023-10-17T22:44:25
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

## readelf -a args-many

```
ELF Header:
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00
  Class:                             ELF64
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              EXEC (Executable file)
  Machine:                           Advanced Micro Devices X86-64
  Version:                           0x1
  Entry point address:               0x400c10
  Start of program headers:          64 (bytes into file)
  Start of section headers:          58536 (bytes into file)
  Flags:                             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           56 (bytes)
  Number of program headers:         6
  Size of section headers:           64 (bytes)
  Number of section headers:         15
  Section header string table index: 14

Section Headers:
  [Nr] Name              Type             Address           Offset
       Size              EntSize          Flags  Link  Info  Align
  [ 0]                   NULL             0000000000000000  00000000
       0000000000000000  0000000000000000           0     0     0
  [ 1] .text             PROGBITS         0000000000400000  00001000
       0000000000004544  0000000000000000  AX       0     0     4
  [ 2] .rodata           PROGBITS         0000000000404550  00005550
       000000000000097a  0000000000000000   A       0     0     16
  [ 3] .note.gnu.pr[...] NOTE             0000000000404ed0  00005ed0
       0000000000000020  0000000000000000   A       0     0     8
  [ 4] .bss              NOBITS           0000000000405f00  00005f00
       0000000000000524  0000000000000000  WA       0     0     32
  [ 5] .comment          PROGBITS         0000000000000000  00005ef0
       000000000000002b  0000000000000001  MS       0     0     1
  [ 6] .debug_aranges    PROGBITS         0000000000000000  00005f1b
       00000000000001e0  0000000000000000           0     0     1
  [ 7] .debug_info       PROGBITS         0000000000000000  000060fb
       00000000000040e9  0000000000000000           0     0     1
  [ 8] .debug_abbrev     PROGBITS         0000000000000000  0000a1e4
       0000000000000e43  0000000000000000           0     0     1
  [ 9] .debug_line       PROGBITS         0000000000000000  0000b027
       0000000000001a5b  0000000000000000           0     0     1
  [10] .debug_str        PROGBITS         0000000000000000  0000ca82
       00000000000008f6  0000000000000001  MS       0     0     1
  [11] .debug_line_str   PROGBITS         0000000000000000  0000d378
       00000000000001f6  0000000000000001  MS       0     0     1
  [12] .symtab           SYMTAB           0000000000000000  0000d570
       0000000000000b10  0000000000000018          13    47     8
  [13] .strtab           STRTAB           0000000000000000  0000e080
       000000000000038c  0000000000000000           0     0     1
  [14] .shstrtab         STRTAB           0000000000000000  0000e40c
       000000000000009a  0000000000000000           0     0     1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings), I (info),
  L (link order), O (extra OS processing required), G (group), T (TLS),
  C (compressed), x (unknown), o (OS specific), E (exclude),
  D (mbind), l (large), p (processor specific)

There are no section groups in this file.

Program Headers:
  Type           Offset             VirtAddr           PhysAddr
                 FileSiz            MemSiz              Flags  Align
  LOAD           0x0000000000000000 0x00000000003ff000 0x00000000003ff000
                 0x0000000000000190 0x0000000000000190  R      0x1000
  LOAD           0x0000000000001000 0x0000000000400000 0x0000000000400000
                 0x0000000000004ef0 0x0000000000004ef0  R E    0x1000
  LOAD           0x0000000000000f00 0x0000000000405f00 0x0000000000405f00
                 0x0000000000000000 0x0000000000000524  RW     0x1000
  NOTE           0x0000000000005ed0 0x0000000000404ed0 0x0000000000404ed0
                 0x0000000000000020 0x0000000000000020  R      0x8
  GNU_PROPERTY   0x0000000000005ed0 0x0000000000404ed0 0x0000000000404ed0
                 0x0000000000000020 0x0000000000000020  R      0x8
  GNU_STACK      0x0000000000000000 0x0000000000000000 0x0000000000000000
                 0x0000000000000000 0x0000000000000000  RW     0x10

 Section to Segment mapping:
  Segment Sections...
   00
   01     .text .rodata .note.gnu.property
   02     .bss
   03     .note.gnu.property
   04     .note.gnu.property
   05

There is no dynamic section in this file.

There are no relocations in this file.
No processor specific unwind information to decode

Symbol table '.symtab' contains 118 entries:
   Num:    Value          Size Type    Bind   Vis      Ndx Name
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND
     1: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS args.c
     2: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS lib.c
     3: 0000000000400155   358 FUNC    LOCAL  DEFAULT    1 vmsg
     4: 0000000000405f20  1024 OBJECT  LOCAL  DEFAULT    4 buf.0
     5: 00000000004003a0   130 FUNC    LOCAL  DEFAULT    1 swap
     6: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS entry.c
     7: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS random.c
     8: 0000000000406320   256 OBJECT  LOCAL  DEFAULT    4 s
     9: 0000000000406420     1 OBJECT  LOCAL  DEFAULT    4 s_i
    10: 0000000000406421     1 OBJECT  LOCAL  DEFAULT    4 s_j
    11: 0000000000406422     1 OBJECT  LOCAL  DEFAULT    4 inited
    12: 0000000000400c49    56 FUNC    LOCAL  DEFAULT    1 swap_byte
    13: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS stdio.c
    14: 0000000000400f07    41 FUNC    LOCAL  DEFAULT    1 isdigit
    15: 0000000000400f30    41 FUNC    LOCAL  DEFAULT    1 isprint
    16: 0000000000400fd7    84 FUNC    LOCAL  DEFAULT    1 vsnprintf_helper
    17: 00000000004047c0    24 OBJECT  LOCAL  DEFAULT    2 base_d
    18: 00000000004047f0    24 OBJECT  LOCAL  DEFAULT    2 base_o
    19: 0000000000404820    24 OBJECT  LOCAL  DEFAULT    2 base_x
    20: 0000000000404850    24 OBJECT  LOCAL  DEFAULT    2 base_X
    21: 000000000040193f   974 FUNC    LOCAL  DEFAULT    1 parse_conversion
    22: 0000000000404c38    10 OBJECT  LOCAL  DEFAULT    2 __func__.0
    23: 0000000000401d0d   830 FUNC    LOCAL  DEFAULT    1 format_integer
    24: 0000000000402092   219 FUNC    LOCAL  DEFAULT    1 format_string
    25: 000000000040204b    71 FUNC    LOCAL  DEFAULT    1 output_dup
    26: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS string.c
    27: 0000000000404d4e     7 OBJECT  LOCAL  DEFAULT    2 __func__.10
    28: 0000000000404d58     8 OBJECT  LOCAL  DEFAULT    2 __func__.9
    29: 0000000000404d60     7 OBJECT  LOCAL  DEFAULT    2 __func__.8
    30: 0000000000404d67     7 OBJECT  LOCAL  DEFAULT    2 __func__.7
    31: 0000000000404d6e     7 OBJECT  LOCAL  DEFAULT    2 __func__.6
    32: 0000000000404d75     7 OBJECT  LOCAL  DEFAULT    2 __func__.5
    33: 0000000000404d80     9 OBJECT  LOCAL  DEFAULT    2 __func__.4
    34: 0000000000404d89     7 OBJECT  LOCAL  DEFAULT    2 __func__.3
    35: 0000000000404d90     7 OBJECT  LOCAL  DEFAULT    2 __func__.2
    36: 0000000000404d98     8 OBJECT  LOCAL  DEFAULT    2 __func__.1
    37: 0000000000404da0     8 OBJECT  LOCAL  DEFAULT    2 __func__.0
    38: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS debug.c
    39: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS syscall.c
    40: 0000000000404e0b     5 OBJECT  LOCAL  DEFAULT    2 __func__.1
    41: 0000000000404e10     5 OBJECT  LOCAL  DEFAULT    2 __func__.0
    42: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS console.c
    43: 000000000040437d   115 FUNC    LOCAL  DEFAULT    1 add_char
    44: 00000000004043f0    93 FUNC    LOCAL  DEFAULT    1 flush
    45: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS debug.c
    46: 0000000000406423     1 OBJECT  LOCAL  DEFAULT    4 explained.0
    47: 00000000004042da    55 FUNC    GLOBAL DEFAULT    1 putchar
    48: 0000000000400330   112 FUNC    GLOBAL DEFAULT    1 fail
    49: 000000000040216d    93 FUNC    GLOBAL DEFAULT    1 __printf
    50: 0000000000402c53   455 FUNC    GLOBAL DEFAULT    1 strtok_r
    51: 000000000040108d    93 FUNC    GLOBAL DEFAULT    1 printf
    52: 0000000000405f08     1 OBJECT  GLOBAL DEFAULT    4 quiet
    53: 000000000040255d   336 FUNC    GLOBAL DEFAULT    1 memmove
    54: 000000000040102b    98 FUNC    GLOBAL DEFAULT    1 snprintf
    55: 0000000000403c90   145 FUNC    GLOBAL DEFAULT    1 munmap
    56: 0000000000402455   264 FUNC    GLOBAL DEFAULT    1 memcpy
    57: 0000000000404285    85 FUNC    GLOBAL DEFAULT    1 puts
    58: 0000000000403b50   153 FUNC    GLOBAL DEFAULT    1 dup2
    59: 00000000004008f7   212 FUNC    GLOBAL DEFAULT    1 check_file
    60: 000000000040369f   154 FUNC    GLOBAL DEFAULT    1 remove
    61: 0000000000400f59   126 FUNC    GLOBAL DEFAULT    1 vsnprintf
    62: 0000000000403e55   159 FUNC    GLOBAL DEFAULT    1 readdir
    63: 00000000004002bb   117 FUNC    GLOBAL DEFAULT    1 msg
    64: 0000000000403f8e   148 FUNC    GLOBAL DEFAULT    1 inumber
    65: 0000000000403ef4   154 FUNC    GLOBAL DEFAULT    1 isdir
    66: 0000000000404311   108 FUNC    GLOBAL DEFAULT    1 vhprintf
    67: 0000000000403be9   167 FUNC    GLOBAL DEFAULT    1 mmap
    68: 0000000000400190     0 NOTYPE  GLOBAL DEFAULT  ABS __executable_start
    69: 00000000004010ea  2133 FUNC    GLOBAL DEFAULT    1 __vprintf
    70: 0000000000402f3b    70 FUNC    GLOBAL DEFAULT    1 strnlen
    71: 0000000000400d77   358 FUNC    GLOBAL DEFAULT    1 random_bytes
    72: 0000000000402af6    77 FUNC    GLOBAL DEFAULT    1 strrchr
    73: 0000000000403204   175 FUNC    GLOBAL DEFAULT    1 debug_panic
    74: 00000000004038fc   155 FUNC    GLOBAL DEFAULT    1 write
    75: 00000000004037cd   148 FUNC    GLOBAL DEFAULT    1 filesize
    76: 00000000004041eb    55 FUNC    GLOBAL DEFAULT    1 vprintf
    77: 0000000000403d21   154 FUNC    GLOBAL DEFAULT    1 chdir
    78: 0000000000403a2b   148 FUNC    GLOBAL DEFAULT    1 tell
    79: 00000000004034db   148 FUNC    GLOBAL DEFAULT    1 exec
    80: 00000000004028f7   170 FUNC    GLOBAL DEFAULT    1 memchr
    81: 00000000004009cb   581 FUNC    GLOBAL DEFAULT    1 compare_bytes
    82: 000000000040356f   148 FUNC    GLOBAL DEFAULT    1 wait
    83: 0000000000404222    99 FUNC    GLOBAL DEFAULT    1 hprintf
    84: 0000000000400c10    57 FUNC    GLOBAL DEFAULT    1 _start
    85: 0000000000402bac   167 FUNC    GLOBAL DEFAULT    1 strstr
    86: 0000000000403861   155 FUNC    GLOBAL DEFAULT    1 read
    87: 00000000004004c7   366 FUNC    GLOBAL DEFAULT    1 exec_children
    88: 0000000000402f81   292 FUNC    GLOBAL DEFAULT    1 strlcpy
    89: 00000000004021ca   651 FUNC    GLOBAL DEFAULT    1 hex_dump
    90: 0000000000400edd    42 FUNC    GLOBAL DEFAULT    1 random_ulong
    91: 0000000000403603   156 FUNC    GLOBAL DEFAULT    1 create
    92: 00000000004026ad   304 FUNC    GLOBAL DEFAULT    1 memcmp
    93: 0000000000403447   148 FUNC    GLOBAL DEFAULT    1 fork
    94: 0000000000404022   152 FUNC    GLOBAL DEFAULT    1 symlink
    95: 0000000000402e1a   157 FUNC    GLOBAL DEFAULT    1 memset
    96: 0000000000400000   341 FUNC    GLOBAL DEFAULT    1 main
    97: 000000000040444d   208 FUNC    GLOBAL DEFAULT    1 debug_backtrace
    98: 00000000004030a5   351 FUNC    GLOBAL DEFAULT    1 strlcat
    99: 0000000000403997   148 FUNC    GLOBAL DEFAULT    1 seek
   100: 00000000004040ba   157 FUNC    GLOBAL DEFAULT    1 mount
   101: 00000000004027dd   282 FUNC    GLOBAL DEFAULT    1 strcmp
   102: 0000000000402a36   105 FUNC    GLOBAL DEFAULT    1 strcspn
   103: 0000000000400c81   246 FUNC    GLOBAL DEFAULT    1 random_init
   104: 0000000000404157   148 FUNC    GLOBAL DEFAULT    1 umount
   105: 0000000000400635   219 FUNC    GLOBAL DEFAULT    1 wait_children
   106: 00000000004032b3   200 FUNC    GLOBAL DEFAULT    1 halt
   107: 000000000040337b   204 FUNC    GLOBAL DEFAULT    1 exit
   108: 0000000000405f00     8 OBJECT  GLOBAL DEFAULT    4 test_name
   109: 0000000000402b43   105 FUNC    GLOBAL DEFAULT    1 strspn
   110: 0000000000402eb7   132 FUNC    GLOBAL DEFAULT    1 strlen
   111: 0000000000403739   148 FUNC    GLOBAL DEFAULT    1 open
   112: 00000000004029a1   149 FUNC    GLOBAL DEFAULT    1 strchr
   113: 0000000000400422   165 FUNC    GLOBAL DEFAULT    1 shuffle
   114: 0000000000403dbb   154 FUNC    GLOBAL DEFAULT    1 mkdir
   115: 0000000000400710   487 FUNC    GLOBAL DEFAULT    1 check_file_handle
   116: 0000000000403abf   145 FUNC    GLOBAL DEFAULT    1 close
   117: 0000000000402a9f    87 FUNC    GLOBAL DEFAULT    1 strpbrk
```

## 해결

malloc, calloc에서 터지는 것을 보고 malloc 인자에 들어가는 숫자의 크기를 늘려줬더니 해결이 됐다. 그래서 아예 `struct page` 안에 의미없는 49개짜리 char형 배열을 넣어 당장의 문제를 해결했다.

## 해결 후 make check

```
pass tests/threads/alarm-multiple
pass tests/userprog/args-none
pass tests/userprog/args-single
pass tests/userprog/args-multiple
pass tests/userprog/args-many
pass tests/userprog/args-dbl-space
pass tests/userprog/halt
pass tests/userprog/exit
pass tests/userprog/create-normal
pass tests/userprog/create-empty
pass tests/userprog/create-null
pass tests/userprog/create-bad-ptr
pass tests/userprog/create-long
pass tests/userprog/create-exists
pass tests/userprog/create-bound
pass tests/userprog/open-normal
pass tests/userprog/open-missing
pass tests/userprog/open-boundary
pass tests/userprog/open-empty
pass tests/userprog/open-null
pass tests/userprog/open-bad-ptr
pass tests/userprog/open-twice
pass tests/userprog/close-normal
pass tests/userprog/close-twice
pass tests/userprog/close-bad-fd
pass tests/userprog/read-normal
pass tests/userprog/read-bad-ptr
pass tests/userprog/read-boundary
pass tests/userprog/read-zero
pass tests/userprog/read-stdout
pass tests/userprog/read-bad-fd
pass tests/userprog/write-normal
pass tests/userprog/write-bad-ptr
pass tests/userprog/write-boundary
pass tests/userprog/write-zero
pass tests/userprog/write-stdin
pass tests/userprog/write-bad-fd
pass tests/userprog/fork-once
pass tests/userprog/fork-multiple
pass tests/userprog/fork-recursive
FAIL tests/userprog/fork-read
pass tests/userprog/fork-close
pass tests/userprog/fork-boundary
FAIL tests/userprog/exec-once
FAIL tests/userprog/exec-arg
FAIL tests/userprog/exec-boundary
pass tests/userprog/exec-missing
pass tests/userprog/exec-bad-ptr
FAIL tests/userprog/exec-read
FAIL tests/userprog/wait-simple
FAIL tests/userprog/wait-twice
FAIL tests/userprog/wait-killed
pass tests/userprog/wait-bad-pid
FAIL tests/userprog/multi-recurse
FAIL tests/userprog/multi-child-fd
pass tests/userprog/rox-simple
FAIL tests/userprog/rox-child
FAIL tests/userprog/rox-multichild
pass tests/userprog/bad-read
pass tests/userprog/bad-write
FAIL tests/userprog/bad-read2
FAIL tests/userprog/bad-write2
pass tests/userprog/bad-jump
FAIL tests/userprog/bad-jump2
pass tests/vm/pt-grow-stack
pass tests/vm/pt-grow-bad
pass tests/vm/pt-big-stk-obj
pass tests/vm/pt-bad-addr
pass tests/vm/pt-bad-read
pass tests/vm/pt-write-code
FAIL tests/vm/pt-write-code2
pass tests/vm/pt-grow-stk-sc
pass tests/vm/page-linear
FAIL tests/vm/page-parallel
FAIL tests/vm/page-merge-seq
FAIL tests/vm/page-merge-par
FAIL tests/vm/page-merge-stk
FAIL tests/vm/page-merge-mm
pass tests/vm/page-shuffle
FAIL tests/vm/mmap-read
FAIL tests/vm/mmap-close
FAIL tests/vm/mmap-unmap
FAIL tests/vm/mmap-overlap
FAIL tests/vm/mmap-twice
FAIL tests/vm/mmap-write
FAIL tests/vm/mmap-ro
FAIL tests/vm/mmap-exit
FAIL tests/vm/mmap-shuffle
FAIL tests/vm/mmap-bad-fd
FAIL tests/vm/mmap-clean
FAIL tests/vm/mmap-inherit
FAIL tests/vm/mmap-misalign
FAIL tests/vm/mmap-null
FAIL tests/vm/mmap-over-code
FAIL tests/vm/mmap-over-data
FAIL tests/vm/mmap-over-stk
FAIL tests/vm/mmap-remove
FAIL tests/vm/mmap-zero
FAIL tests/vm/mmap-bad-fd2
FAIL tests/vm/mmap-bad-fd3
FAIL tests/vm/mmap-zero-len
FAIL tests/vm/mmap-off
FAIL tests/vm/mmap-bad-off
FAIL tests/vm/mmap-kernel
FAIL tests/vm/lazy-file
pass tests/vm/lazy-anon
FAIL tests/vm/swap-file
FAIL tests/vm/swap-anon
FAIL tests/vm/swap-iter
FAIL tests/vm/swap-fork
pass tests/filesys/base/lg-create
pass tests/filesys/base/lg-full
pass tests/filesys/base/lg-random
pass tests/filesys/base/lg-seq-block
pass tests/filesys/base/lg-seq-random
pass tests/filesys/base/sm-create
pass tests/filesys/base/sm-full
pass tests/filesys/base/sm-random
pass tests/filesys/base/sm-seq-block
pass tests/filesys/base/sm-seq-random
FAIL tests/filesys/base/syn-read
pass tests/filesys/base/syn-remove
FAIL tests/filesys/base/syn-write
pass tests/threads/alarm-single
pass tests/threads/alarm-multiple
pass tests/threads/alarm-simultaneous
pass tests/threads/alarm-priority
pass tests/threads/alarm-zero
pass tests/threads/alarm-negative
pass tests/threads/priority-change
pass tests/threads/priority-donate-one
pass tests/threads/priority-donate-multiple
pass tests/threads/priority-donate-multiple2
pass tests/threads/priority-donate-nest
pass tests/threads/priority-donate-sema
pass tests/threads/priority-donate-lower
pass tests/threads/priority-fifo
pass tests/threads/priority-preempt
pass tests/threads/priority-sema
pass tests/threads/priority-condvar
pass tests/threads/priority-donate-chain
FAIL tests/vm/cow/cow-simple
54 of 141 tests failed.
```
