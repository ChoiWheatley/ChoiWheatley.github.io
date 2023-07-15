---
description:
aliases: 
tags: 
created: 2023-05-18T00:07:16
updated: 2023-07-15T21:33:04
title: magic methods (python)
---

- `__repr__`을 구현하면 자바에서의 `toString`과 동일한 효과를 볼 수 있군. https://www.geeksforgeeks.org/convert-object-to-string-in-python/
- `__repr__`는 유효한 파이썬 표현식 형태의 문자열을 리턴하도록 권장하는 반면, `__str__`는 그런 제안사항은 없고 걍 아무렇게나 출력하면 된다. 마치 러스트의 debug trait과 display trait의 차이를 보여주는 것 같네.
- [python doc - datamodel - attributes](https://docs.python.org/3/reference/datamodel.html#index-32) 쪽을 잘 보면 writable과 read-only 구분이 정해져 있는데, 물론 거의 대다수는 writable이지만, read-only의 경우, 사용자가 해당 attribute를 재정의 할 수 없다는 것을 의미한다. 
	- [!] 추가적으로 사용자는 read-only attribute를 만들 수 없는 것으로 보인다. 아직 확실하지 않으니 조사가 좀 더 필요해보인다.
