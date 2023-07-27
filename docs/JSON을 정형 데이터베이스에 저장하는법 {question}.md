---
aliases: 
tags: 
description:
title: JSON을 정형 데이터베이스에 저장하는법 {question}
created: 2023-07-27T11:10:50
updated: 2023-07-27T11:11:36
---

# Q.

 안녕하세요! 오르미1기 최승현이라고 합니다! 데이터베이스 설계시 JSON을 처리하는 방법이 궁금하여 메시지 보내게 되었습니다. openai의 응답이 반정형 데이터인 JSON으로 구성되어있기 때문에 정형 데이터로 변환하기 위한 과정이 필요한 상황에 놓이게 되었습니다. 향후 openai의 스키마 변경에 유연하게 대응하기 위해 데이터베이스 컬럼을 key, value만 가지고 있게 만들어 보았습니다만, 이대로는 계층구조를 표현할수가 없더라고요. 멘토님의 조언 부탁드립니다! ERD 함께 첨부해드립니다.

Prompt: 명상서비스에 필요한 프롬프트 (현재 상태, 목표, 기타) 그리고 유저의 응답을 저장하는 테이블입니다.

ChatBotConfig: ai 모델, temperature, max tokens 등을 선택할 용도로 사용할 계획입니다.

ChatBotReply: openai 응답을 저장할 테이블입니다.

![[Pasted image 20230727111104.png]]

openai 문서의 예시 Response 스니펫을 보여드릴게요

```json
{
  "id": "chatcmpl-123",
  "object": "chat.completion",
  "created": 1677652288,
  "choices": [{
    "index": 0,
    "message": {
      "role": "assistant",
      "content": "\n\nHello there, how may I assist you today?",
    },
    "finish_reason": "stop"
  }],
  "usage": {
    "prompt_tokens": 9,
    "completion_tokens": 12,
    "total_tokens": 21
  }
}
```

[오전 11:07]최승현: 보시다시피 message, usage는 한 단계가 더 들어가기 때문에 정형 데이터베이스로는 테이블을 하나 더 만들어야 하는 상황에 놓이게 되더라구요  
[오전 11:08]최승현: 현재 프로젝트에서는 이걸 굳이 다 저장할 필요는 없겠지만 그래도 언젠간 다계층 JSON 객체를 저장할 날이 올 것만 같은 기분이 들더군요;;  
[오전 11:08]최승현: 꼭 그래야 한다면 MongoDB를 사용해야만 할까요?
