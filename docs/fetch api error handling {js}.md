---
aliases: 
tags: 
description:
created: 2023-06-28T22:46:06
updated: 2023-07-15T21:33:04
title: fetch api error handling {js}
---
- [sof](https://stackoverflow.com/questions/50330795/fetch-api-error-handling)
- `Promise.reject`, `Promise.catch`를 사용하여 에러처리를 할 수 있구나. catch 안에 catch가 또 있어서 자칫하면 인덴트가 엄청 길어질 수도 있겠다는 생각을 했다.
- 아래 코드는 sof에서 가져온 스니펫으로, JSON API error, Generic API error, Generic fetch error를 모두 다룰 수 있다.

```js
fetch("api/v1/demo", {
    method: "POST",
    headers: {
        "Content-Type": "application/json"
    },
    body: JSON.stringify({
        "data": "demo"
    })
})
    .then(response => {
        if (!response.ok) {
            return Promise.reject(response);
        }
        return response.json();
    })
    .then(data => {
        console.log("Success");
        console.log(data);
    })
    .catch(error => {
        if (typeof error.json === "function") {
            error.json().then(jsonError => {
                console.log("Json error from API");
                console.log(jsonError);
            }).catch(genericError => {
                console.log("Generic error from API");
                console.log(error.statusText);
            });
        } else {
            console.log("Fetch error");
            console.log(error);
        }
    });
```
