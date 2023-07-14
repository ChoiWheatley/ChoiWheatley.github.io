---
description:
aliases: 
tags: 
created: 2023-05-17T23:11:15
updated: 2023-07-11T15:21:08
title: markdown - python package
---
```python
import markdown


def convert_markdown_to_html(markdown_text):
    """마크다운 텍스트를 HTML 문서로 변환합니다.

    Args:
      markdown_text: 변환할 마크다운 텍스트입니다.

    Returns:
      변환된 HTML 문서입니다.
    """

    return markdown.markdown(markdown_text)


if __name__ == "__main__":
    markdown_text = """
    # This is a heading

    This is some text.

    * This is a bulleted list item.
    * This is another bulleted list item.

    Here is a link: [Google](https://www.google.com).
    """

    html_text = convert_markdown_to_html(markdown_text)

    print(html_text)

```