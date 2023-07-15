---
description:
aliases: 
tags: 
created: 2023-05-18T00:24:46
updated: 2023-07-15T21:33:03
title: xlsxwriter - python package
---

```python
import xlsxwriter

# 엑셀 파일 생성하기
workbook = xlsxwriter.Workbook("test.xlsx")

# 워크시트 생성하기
worksheet = workbook.add_worksheet("test")

worksheet.write(0, 0, "AA")
worksheet.write(0, 1, "BB")
worksheet.write(0, 2, "CC")
worksheet.write(0, 3, "DD")

worksheet.write(1, 0, 10)
worksheet.write(1, 1, 20)
worksheet.write(1, 2, 30)
worksheet.write(1, 3, 40)

workbook.close()
```

```python
# 연습문제: 다음 저시기를 엑셀로 만들어라

홍길동 = [33, 88, 24]
이호준 = [34, 66, 77]
김철수 = [78, 82, 36]
all_data = [홍길동, 이호준, 김철수]

workbook = xlsxwriter.Workbook("test.xlsx")
worksheet = workbook.add_worksheet("Grades")

row_header = ["", "국어", "영어", "수학", "평균"]
col_header = ["", "홍길동", "이호준", "김철수"]

for idx, subject in enumerate(row_header):
    worksheet.write(0, idx, subject)

for idx, subject in enumerate(col_header):
    worksheet.write(idx, 0, subject)

for row_idx, line in enumerate(all_data, start=1):
    for col_idx, value in enumerate(line, start=1):
        worksheet.write(row_idx, col_idx, value)
    # average
    avg = sum(line) / len(line)
    worksheet.write(row_idx, len(line) + 1, avg)


workbook.close()
```
