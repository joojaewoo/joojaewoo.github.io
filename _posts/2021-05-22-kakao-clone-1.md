---
layout: post
title: "[ToyProject] 카카오톡 클론 프로젝트(1)"
date: 2021-05-22 19:21:00 +09:00
categories: [development, Clone]
tags: [next.js, typescript, eslint, preitter]
comments: true
---

이전부터 소켓통신으로 프로젝트를 하나 진행해보고 싶다는 생각을 하고있어서 카카오톡 클론 프로젝트를 진행하기로 했다.

오늘은 프론트엔드 선택한 기술에 대한 이유와 간단한 환경 설정에 대해서 포스팅하려고 한다.

### 개발 스택

현재 프론트엔드 개발 스택은 NEXT.js, emotion, storyBook, Apollo-Client를 사용하는 것으로 결정했다. 이전에 프로젝트를 하면서 사용해본 기술스택을 사용해보고, 추후 다른 기술 스택을 사용해서 변경해볼 예정이다.

백엔드는 Nest와 GraphQL을 사용하고 db로는 MongoDB를 사용할 예정이다

언어는 공통적으로 TypeScript를 사용하기로 결정했다.

### NEXT.js

NEXT.js를 이전 프로젝트에서도 사용해보았는데 `Zero Config`를 통해서 웹펙, 바벨등 기본 설정 없이도 프로젝트를 실행시킬 수 있었다. 아마 나중에 alias나 dotenv를 사용하기 위해서는 웹펙 설정을 커스텀할 예정이다. page 라우팅이 직관적이여서 사용하기 편리했으며, SSR 프레임워크이므로 검색엔진에 최적화 되있다. 코드스플리팅도 자동으로 해줘서 프로젝트를 처음 시작할때 다른 설정 없이 빠르게 프로젝트를 진행할 수 있어서 Next.js를 사용하기로 결정했다. 프로젝트가 완성되고 나면 다른 프레임워크를 사용해서 프론트엔드를 만들어 볼 예정이다.

### emotion

CSS-in-JS를 사용해보면서 컴포넌트를 재사용 할 수 있었으며, 프리픽스가 자동으로 생성되었다. 또한 className이 자동으로 부여되기 때문에 스타일이 겹칠 염려가 없으며 props에 따라 스타일 지정이 가능했다. 이전 프로젝트에서 styled-components를 사용해봐서 이번 프로젝트에서 emotion을 사용하기로 결정했다.

### StoryBook

독립된 환경에서 각각의 컴포넌트들을 개발하고 이를 확인하기 위해서 사용한다. 또한, 각종 addon을 설치해서 최소한의 기능들을 사용할 수 있다.
UI 테스트르 해본적이 한번도 없어 이번 기회에 snapshot 테스트를 storybook를 사용해서 해 볼 예정이다.

### 환경 설정

#### NEXT 프로젝트 생성

`npx create-next-app` 명령어를 통해 NEXT js 프로젝트를 생성한다.

이후 `npm run dev`명령어를 통해 dev서버를 실행하고 `http://127.0.0.1:300`에 접속하면 아래와 같은 결과물을 볼 수 있다.

![next](/assets/img/posts/kakao01.png)

이렇게 하면 기본적인 NEXT 프로젝트 생성이 완료되었다.

#### TypeScript 설정하기

TypeScript를 적용하기 위해서 아래의 패키지를 설치해준다.

```cmd
npm i -D typescript @types/react @types/node
```

설치가 완료되면 pages폴더에 있는 파일들의 확장자를 `tsx`로 바꿔준다.

이후 `npm run dev` 명령어를 통해 프로젝트를 다시 실행시키면 프로젝트 내부에 `tsconfig.json` 파일과 `next-env.d.ts`파일이 자동적으로 생성되면서 실행된다.

이것으로 NEXT JS에 타입스크립트 적용이 완료되었다.

#### eslint 와 prettier 설정

프로젝트에 eslint와 prettier을 이제 설정해보자.

아래와 같이 필요한 플러그인들을 설치해주자.
airbnb 규칙을 사용할것이기 때문에 airbnb를 패키지에 추가했다.

```cmd
npm i -D @typescript-eslint/eslint-plugin typescript-eslint/parser eslint eslint-config-airbnb eslint-config-prettier eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-prettier eslint-plugin-react eslint-plugin-react-hooks prettier
```

이후 루트 디렉토리에 `.eslintrc`파일과 `.prettierrc`파일을 생성해주자.

```json
// .eslintrc
{
  "parser": "@typescript-eslint/parser", // 타입스크립트의 구문 해석을 위해 사용
  "extends": ["airbnb", "prettier"], // airbnb 규칙, prettier사용
  "env": {
    "browser": true,
    "node": true
  }, // 사전 정의된 전역 변수 사용을 정의, browser, node 설정하지 않으면 console, require 같은 static 메서드를 인식할 수 없다.
  "plugins": ["@typescript-eslint", "react-hooks", "prettier"],
  "rules": {
    // 규칙을 직접 수정
    "prettier/prettier": ["error", { "endOfLine": "auto" }],
    "react/jsx-props-no-spreading": 0,
    "react/jsx-filename-extension": 0,
    "no-use-before-define": 0,
    "import/extensions": 0,
    "import/no-unresolved": 0
  }
}
```

```json
// .prettierrc
{
  "singleQuote": true, // 홑따옴표 사용
  "semi": true, // 세미콜론 찍기
  "useTabs": false, // 탭 사용 여부
  "tabWidth": 2, // 탭 넓이
  "trailingComma": "all", // 후행 쉼표 찍기 ex) {a:1,}
  "printWidth": 100, // 한줄에 적히는 넓이
  "arrowParens": "always" // (x)=>x 처럼 매개변수가 하나일때 항상 괄호 표시
}
```

위처럼 파일을 작성하면 eslint와 prettier 설정 역시 완료되었다.

#### 이모션 설치

마지막으로 emotion을 설치 할 것이다. 나는 react 기반의 NEXT.js를 사용하기 때문에 `@emotion/react`와 `@emotion/styled`를 사용할 것이다.

```cmd
npm i @emotion/react @emotion/styled
```

위의 명령어를 통해 필요한 패키지를 설치하면 끝이다.

```tsx
import styled from "@emotion/styled";

export const Div = styled.div`
  display: flex;
`;
```

위 와 같이 사용할 수 있다.

#### 브랜치 전략

브랜치 전략은 `GitHub flow`를 사용 할 것이다. `git flow`를 이전 팀 프로젝트를 하며 사용해보았지만 `feat`와 `fix` 브랜치만 많이 사용하고 다른 브랜치는 많이 사용하지 않았던 것 같다. 또한 혼자하는 프로젝트기 때문에 `Github flow`만으로도 충분하다고 생각했다.

### 마무리

오늘은 프론트엔드의 간단한 프로젝트 환경설정을 마무리 했다. 내일은 프로젝트에 필요한 기획서, feature list 등 문서화를 하고 백엔드 프로젝트 환경설정을 할 예정이다.
