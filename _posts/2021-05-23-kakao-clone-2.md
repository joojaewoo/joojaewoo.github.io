---
layout: post
title: "[ToyProject] 카카오톡 클론 프로젝트(2)"
date: 2021-05-23 20:21:00 +09:00
categories: [Development, Clone]
tags: [nest.js, typescript, eslint, preitter]
comments: true
---

어제는 NEXT.js 프로젝트 설정을 했고, 오늘은 프로젝트 기획서, 백로그 등 필요한 문서를 위키에 작성한 다음 Nest.js로 백엔드 프로젝트 설정을 할 예정이다.

#### [기획서](https://docs.google.com/presentation/d/1PpMcuZd5_NfjpeEjk90Rl5pGh5cBW3BX7smSf0SfokQ/edit#slide=id.p)

#### [FE-백로그](https://docs.google.com/spreadsheets/d/19uztnVysspHbs35yHtFKBOM2y2NvHStcSP436vmRdkk/edit#gid=0)

#### [BE-백로그](https://docs.google.com/spreadsheets/d/19uztnVysspHbs35yHtFKBOM2y2NvHStcSP436vmRdkk/edit#gid=1110456911)

### NestJS

NestJS는 express 기반으로 제작되었으며, node.js에 백엔드를 구성할 수 있도록 해준다. 기본적으로 typescript를 지원한다. 타입스크립트를 기본적으로 지원하기 때문에 NextJS를 선택하였으며, express말고 다른 프레임 워크를 사용해 보고싶었기 때문에 NestJS를 사용하기로 마음 먹었다.

실제 프로젝트에 적용하기 위해서는 조금 더 공부를 많이 해봐야 할 것 같다.

### 환경 설정

#### NestJS 프로젝트 생성

먼저 아래의 명령어를 통해 nestjs cli를 전역으로 설치해준다.

```cmd
npm i -g @nestjs/cli
```

이후 `nest new project-name` 명령어를 통해 nestjs 프로젝트를 생성해준다.
프로젝트가 생성되면 `npm run start`명령어를 사용하면 `Hello world!` 메시지가 표시된다.

이렇게 초기 프로젝트 세팅을 마칠수 있었다. 보일러플레이트가 잘 되어있어 eslint, prettier 등의 설정이 자동으로 되었다.필요한 세팅값만 조금씩 바꿔 주면 될 것 같다.

#### GraphQL

생각보다 NestJS의 설정이 빨리 끝나서 GraphQL을 사용하기 위한 기초 설정까지 해봐야겠다.

먼저 아래의 명령어를 통해 필요한 패키지들을 설치해준다. 참고로 공식 문서를 보고 따라해 보았다.

```cmd
npm i @nestjs/graphql graphql-tools graphql apollo-server-express
```

설치를 하고나서 `src/app.module.ts`에 `GraphQLModule` 을 추가해준다.

UserModule는 playGround를 테스트해보기 위해서 임의의 데이터 구조와 더미데이터를 반환하게 임시적으로 만들어 보았다.

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { UserModule } from './user/user.module';

@Module({
  imports: [
    UserModule,
    GraphQLModule.forRoot({
      installSubscriptionHandlers: true,
      autoSchemaFile: true,
    }),
  ],
})
```

자세한 내용은 제 [깃 허브](https://github.com/joojaewoo/kakao-talk-clone) PR과 코드를 보시면 될 것 같습니다!

필요한 문서에 대한 작업이 오래걸리고 NestJS를 처음 사용해 봐서 조금 시간이 많이 소요됬다.

내일은 빨리 DB 스키마를 완성하고 로그인 페이지 UI 작업을 시작해야겠다!
