---
<%*
const title = tp.file.title;
const today = moment(title).format("YYYY-MM-DD");
const yesterday = moment(title).subtract(1, 'days').format("YYYY-MM-DD");
const tomorrow = moment(title).add(1, 'days').format("YYYY-MM-DD");
-%>
tags:
- " DailyNote "
---

# <% today %>

<< [[<% yesterday %>]] | [[<% tomorrow %>]]>>

---
# 📅 <% today %> Daily Briefing

## 🎵 오늘의 추천곡


## 🌜 어제는...


## 🙌 지금은...


## 🚀 내가 달성하고자 하는 것들은...


## 👎 오늘 나에게 닥친 어려움은...


---

# 📝 Notes

- 

___

![[Pending ⌛.canvas]]

---
# Notes modified today

```dataview
List FROM "" 
WHERE striptime(date(file.frontmatter.updated)) = date("<%today%>") 
SORT file.mtime asc
```
