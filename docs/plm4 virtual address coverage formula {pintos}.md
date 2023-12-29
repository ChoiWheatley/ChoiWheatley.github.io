---
aliases: 
tags: 
description:
title: plm4 virtual address coverage formula {pintos}
created: 2023-10-04T22:15:55
updated: 2023-10-04T22:16:53
---
- [[week07-10 {swjungle} {pintos}]]
___

> how much space can plm4 (page map level 4) cover? give me a detailed formula

## answer from bing chat

PLM4 stands for Page Map Level 4, which is a paging structure used in long mode (64-bit) by x86-64 processors¹. It is the highest level of the four-level page table system, and it contains 512 entries that point to the Page-Directory Pointer Tables (PDPTs)².

The size of a page depends on the processor mode, the extensions used, and the virtual address bits supported by the processor². In long mode, the page size can be 4 KiB, 2 MiB, or 1 GiB². The space that PLM4 can cover depends on the page size and the number of entries used.

The formula for calculating the space covered by PLM4 is:

$$
\text{Space} = \text{Page Size} \times \text{Number of PDPTs} \times 512 \times 512
$$

where:

- Page Size is either 4 KiB, 2 MiB, or 1 GiB
- Number of PDPTs is the number of entries in PLM4 that are used (from 1 to 512)

For example, if we use 4 KiB pages and all 512 entries in PLM4, then the space covered by PLM4 is:

$$
\text{Space} = 4 \text{ KiB} \times 512 \times 512 \times 512 = 1 \text{ TiB}
$$

If we use 2 MiB pages and only one entry in PLM4, then the space covered by PLM4 is:

$$
\text{Space} = 2 \text{ MiB} \times 1 \times 512 \times 512 = 512 \text{ GiB}
$$

If we use 1 GiB pages and only one entry in PLM4, then the space covered by PLM4 is:

$$
\text{Space} = 1 \text{ GiB} \times 1 \times 512 \times 512 = 256 \text{ TiB}
$$

I hope this helps you understand how much space PLM4 can cover. 😊

¹: [kernel - Paging in x86-64 architecture - Stack Overflow](^3^)  
²: [Page Tables - OSDev Wiki](^2^)

Source: Conversation with Bing, 2023. 10. 4.  
(1) operating system - Page Map Level 4 Paging - Stack Overflow. <https://stackoverflow.com/questions/43317912/page-map-level-4-paging>.  
(2) Page Tables - OSDev Wiki. <https://wiki.osdev.org/Page_Tables>.  
(3) kernel - Paging in x86-64 architecture - Stack Overflow. <https://stackoverflow.com/questions/30006365/paging-in-x86-64-architecture>.  
(4) Page Tables - OSDev Wiki. <https://wiki.osdev.org/Page_Tables>.
