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

## <% today %>

- [[<% yesterday %>]] 
- [[<% tomorrow %>]]

---

## 📝 Notes

- 


---
## 📅 <% today %> Daily Briefing

### 🎵 오늘의 추천곡

### 🏃 오늘의 운동

### 🌞 오늘은...

### 🌜 어제는...

### 🧠 지금 최대 관심사 TOP 3

1. 
2. 
3. 

### 🚀 WHY, HOW, WHAT

> 오늘 하루의 동기를 다시 생각해보는 시간을 가져봅시다. 오늘의 신념, 목표를 달성하기 위한 방법, 오늘의 성과에 대해서 작성해봅시다.

### 👎 오늘 나에게 닥친 어려움은...


---

## 읽을것들 (dataview)

```dataview
LIST
FROM #scrap
```

## Notes modified today (dataview)

```dataview
List FROM "" 
WHERE striptime(date(file.frontmatter.updated)) = date("<%today%>") 
SORT file.mtime asc
```
