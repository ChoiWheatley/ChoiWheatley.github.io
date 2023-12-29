---
<%*
  let prompt = await tp.system.prompt('구체적인 파일명');
  let title = `${tp.date.now("YYYY-MM-DD")}+${prompt}`;
  await tp.file.rename(title);
-%>
title: <%* tR += `${title}` %>
created: <% tp.date.now('YYYY-MM-DD') %>
kind: company
---
<% tp.file.cursor() %>