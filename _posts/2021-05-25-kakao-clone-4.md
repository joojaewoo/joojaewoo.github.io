---
layout: post
title: "[ToyProject] 카카오톡 클론 프로젝트(4)"
date: 2021-05-25 15:21:00 +09:00
categories: [Development, Clone]
tags: [typescript, next.js, emotion]
comments: true
---

오늘은 회원가입 부분을 완료하려고 했지만 면접 준비를 조금 해야 될 것 같아서 회원가입 페이지 UI만 만들어 보았다.

### 여러개 input창 관리

여러개의 input창을 관리하기 위해서 여러개의 useState를 사용했는데, 구글에 검색해보니 이 방법은 좋은 방법이 아니라고해서, [blog](https://react.vlpt.us/basic/09-multiple-inputs.html)를 보고 수정했다.

하나의 useState안에 객체로 값을 저장하고, 각 input 태그에 name을 주고 event.target에서 name와 value를 통해 객체를 업데이트 시켜주는 방법이였다

```tsx
export default () => {
const [inputs, setInputs] = useState({
    id: '',
    pwd: '',
  });

  const { id, pwd } = inputs;

  const onChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { value, name } = e.target;
    setInputs({
      ...inputs,
      [name]: value,
    });
  };
  return (
      <>
        <input name='id' onChange={onChange} value = {id}>
        <input name='pwd' onChange={onChange} value = {pwd}>
      </>
  )
}
```

위와 같은 방법으로 하나의 useState를 사용해서 여러개의 input창을 관리할 수 있었다. 로그인 페이지도 위와 같은 방법으로 변경해 봐야겠다.

### Global Style

media쿼리를 사용해서 일정 크기 이상일때 최대 크기를 설정해 주었지만, 좌측 상단에 붙어있어 이 부분이 마음에 들지 않았다. 블로그를 찾아보니 `transform`를 사용해서 가운데 정렬하는 방법이 있었고 이를 적용했다

```css
position: absolute;
top: 50%;
left: 50%;
transform: translate(-50%, -50%);
```

### story book

스토리북을 사용해서 각 컴포넌트를 생성하니 변경된 내용을 바로바로 확인할 수 있어서 편리했다. 그러나 전역 css를 설정하는 방법을 찾아보지 못해 전체 화면에 맞춰서 나오긴 하지만 어떻게 컴포넌트가 생겼는지(?)를 보면서 수정할 수 있어서 편리한 것 같았다.

```tsx
// SignUp/index.stories.tsx
import React from "react";
import SignUp from "./index";

export default {
  title: "SignUp/SignUpPage",
  component: SignUp,
};

export const Default = () => <SignUp />;
```

위와 같이 간단하게 title과 컴포넌트만 설정하면 `http://localhost:6006`에서 확인할 수 있다.

### 회원가입 페이지 UI

![image](/assets/img/posts/kakao03.png)
