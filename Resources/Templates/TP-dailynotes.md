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

### 📖 오늘의 읽기목록

- 

### ⏰ Daily Routine

**24년 12월의 데일리 루틴**

- [[0012 Career 💼]]
- [[24년 12월의 상칼파]]
- [[docker 교과서]]

### 🚀 WHY, HOW, WHAT

> 오늘 하루의 동기를 다시 생각해보는 시간을 가져봅시다. 오늘의 신념, 목표를 달성하기 위한 방법, 오늘의 성과에 대해서 작성해봅시다.

##  🪂 PARA

> [!note] [PARA Expert](https://chatgpt.com/g/g-46Xrh4MXk-para-expert) 에 위의 'Daily Briefing'을 복붙하면 자동으로 아래의 PARA 구조로 변환해줍니다

### [Projects]
### [Areas]
### [Resources]
### [Archive]


---

## 읽을것들 (dataview)

```dataview
LIST
FROM #scrap
SORT file.mtime desc
```

## Notes modified today (dataview)

```dataview
List FROM "" 
WHERE striptime(date(file.frontmatter.updated)) = date("<%today%>") 
SORT file.mtime desc
```
