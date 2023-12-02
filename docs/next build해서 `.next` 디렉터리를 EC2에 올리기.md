---
aliases: 
tags: 
description:
title: next build해서 `.next` 디렉터리를 EC2에 올리기
created: 2023-12-02T17:10:34
updated: 2023-12-02T17:26:14
---
- [[week14-18 {swjungle}{my own weapon}{nestjs, socketio}]]
- <https://nextjs.org/docs/pages/building-your-application/configuring/environment-variables>
___

> This will tell Next.js to replace all references to `process.env.NEXT_PUBLIC_ANALYTICS_ID` in the Node.js environment with the value from the environment in which you run `next build`, allowing you to use it anywhere in your code. It will be inlined into any JavaScript sent to the browser.

NextJS는 빌드할때 `process.env.*` 환경변수를 **인라인**한다. 즉, 하드코딩된 변수로 갈아치운다는 뜻이다. 그래서 자꾸 개발용 환경변수가 들어갔던 것이다.

## [Runtime Environment Variables](https://nextjs.org/docs/pages/building-your-application/configuring/environment-variables#runtime-environment-variables)

런타임에 환경변수를 알아내는 방법도 제공한다. 근데 `getServerSideProps`를 사용하라는데 무슨 말인지 모르겠음.

## [Default environment variables](https://nextjs.org/docs/pages/building-your-application/configuring/environment-variables#default-environment-variables)

`next dev` 옵션은 `.env.development`를 사용하고, `next start`는 `.env.production` 파일을 사용한다고 한다. `env.local` 환경변수파일은 모든 `.env.*`를 오버라이드 한다.
