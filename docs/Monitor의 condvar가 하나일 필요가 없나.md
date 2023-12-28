---
aliases: 
tags: 
description:
title: Monitor의 condvar가 하나일 필요가 없나?
created: 2023-10-04T21:28:35
updated: 2023-10-04T21:28:49
---

## answer from bing chat

Yes, it is possible to have more than one conditional variable in a monitor. A conditional variable is a queue of threads that are waiting for some condition to become true. A monitor can use one or more conditional variables to coordinate the access and signaling of shared variables and operations¹.

For example, a bounded buffer monitor can use two conditional variables: one for items available and one for slots available. The producer threads can wait on the slots available condition variable before adding an item to the buffer, and signal the items available condition variable after adding an item. The consumer threads can wait on the items available condition variable before removing an item from the buffer, and signal the slots available condition variable after removing an item¹.

You can find more information and examples on conditional variables and monitors in these links:

- [What is the purpose of condition variables in Monitors?](^1^)
- [Condition Variables & Monitors - Virginia Tech](^2^)
- [Now, why does Monitor need a condition variable?](^4^)

Source: Conversation with Bing, 2023. 10. 4.  
(1) What is the purpose of condition variables in Monitors?. <https://stackoverflow.com/questions/35172889/what-is-the-purpose-of-condition-variables-in-monitors>.  
(2) What is the purpose of condition variables in Monitors?. <https://stackoverflow.com/questions/35172889/what-is-the-purpose-of-condition-variables-in-monitors>.  
(3) Condition Variables & Monitors - Virginia Tech. <https://courses.cs.vt.edu/cs3214/spring2023/lecturenotes/conditionvariables.pdf>.  
(4) c# - Now, why does Monitor need a condition variable? - Stack Overflow. <https://stackoverflow.com/questions/24551518/now-why-does-monitor-need-a-condition-variable>.  
(5) Can I include more than one independent or dependent variable in a study? - Scribbr. <https://www.scribbr.com/frequently-asked-questions/more-than-one-independent-or-dependent-variable/>.  
(6) Multiple conditions for a conditional column in Power Query. <https://www.fourmoo.com/2018/11/27/multiple-conditions-for-a-conditional-column-in-power-query/>.
