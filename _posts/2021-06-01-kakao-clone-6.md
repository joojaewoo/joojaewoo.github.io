---
layout: post
title: "[ToyProject] 카카오톡 클론 프로젝트(6)"
date: 2021-06-01 13:53:00 +09:00
categories: [Development, Clone]
tags: [typescript, next.js, emotion, nest.js]
comments: true
---

오늘은 헤더 컴포넌트를 구현하고 이메일 인증 기능, 회원가입 기능을 구현하였다.

### 헤더 컴포넌트

친구목록과 채팅목록에서 모두 사용하는 헤더 컴포넌트를 구현하였다. title과 children을 props로 받아 구현하였고, position을 fix로 고정시켜 항상 상단에 유지되게 구현하였다.

메인페이지에 헤더를 달아 보았다. 지금보니 헤더의 크기가 조금 작은 것 같다. 크기를 좀더 크게 수정해야 겠다.

![image](/assets/img/posts/kakao06.png)

### 환경변수 설정

.env를 통해 환경변수를 관리하기 위해 `@nestjs/config`패키지를 설치했다. 이후 app.module 파일의 imports 부분에 config모듈을 추가하면 된다. isGlobal속성을 통해 전역적으로 환경변수를 사용할 수 있게 해주었다. 다양한 옵션들이 많으니 궁금하면 공식홈페이지를 참조하면 될 것 같다.

```ts
import { ConfigModule } from '@nestjs/config';

@Module({
    imports:[
        ConfigModule.forRoot({isGlobal:true}),
        ...
    ]
    ...
})
```

### 이메일 인증 기능

메일 인증기능을 구현하기 위해서 nodemailer라이브러리를 사용했다.
nodemailer을 사용하기 위해서는 보안설정을 해줘야한다. [https://myaccount.google.com/lesssecureapps](https://myaccount.google.com/lesssecureapps) 링크로 들어가서 보안 수준이 낮은 앱의 액세스 허용으로 설정해줘야한다.

#### nodemailer 객체 생성

```ts
nodemailer.createTransport({
  service: "Gmail",
  auth: {
    user: "구글아이디",
    pass: "구글 비밀번호",
  },
  tls: {
    rejectUnauthorized: false,
  },
});
```

nodemailer을 다운로드 받은 다음 다음과 같이 객체를 생성해주었다. 나는 구글 메일을 사용했지만 네이버 메일을 사용하고 싶은 경우 `service: "Naver"`로 설정해주면 된다. 네이버는 1000건, 구글은 500건 제한이 걸려있다고 알고있다.

#### 인증번호 생성 함수

```ts
export const getRandomNum = () =>
  Math.floor(Math.random() * 1000000)
    .toString()
    .padStart(6, "0");
```

위처럼 6자리 자연수를 문자열로 바꿔주고 6자리가 안될시 앞에 0을 6자리까지 붙여주었다. 이로써 6자리의 랜덤 문자열을 생성할 수 있었다.

> padStart (maxLength, str) : 첫 번째 인자로 최대길이, 두번째 인자로 반복할 문자열을 넣어주면, 최대길이가 될때까지 문자열을 반복하여 앞쪽에 추가해준다. 뒤에 추가하려면 padEnd를 사용하면된다.

#### 메일 내용 작성

메일옵션을 다음과 같이 정의했다. 나처럼 하면 텍스트로 전송되어 한줄로 전송되고 글 크기, 색상등을 추가할 수없지만 html 태그도 넣을 수 있다고 보았다. 나중에 시간이 되면 변경해 봐야겠다.

```ts
const mailOptions = {
  from: "jaewoo`s clone project",
  to: email,
  subject: "[카카오톡 클론]인증 관련 이메일 입니다",
  text: "오른쪽 숫자 6자리를 입력해주세요 : " + number,
};
```

#### 메일 보내기

`nodemailer.createTransport`로 만든 객체의 `sendMail`메소드를 사용하면 메일을 보낼 수 있다. 여기서 두번째 인자로 콜백함수를 넣어 실패, 성공의 경우에 따라 행동을 정의할 수 있다. 그러나 나의 경우 해당 함수 전체를 try catch 문을 사용해서 에러처리를 해주었다.

```ts
const result = await smtpTransport.sendMail(mailOptions);
```

#### 메일 인증

메일 인증을 어떻게 할까 고민을 많이 했는데, 가장 단순하게 컬렉션을 하나 더 만들고, 해당 컬렉션에 이메일정보와, 인증 키를 저장해 놓는다. 같은 이메일로 인증 정보를 받을 경우, 인증키를 수정해준다. 인증 번호를 입력하고 인증하기 버튼을 누르면, 해당 컬렉션에서 이메일, 인증번호로 찾아서 값이 있으면 인증 완료를 시켜주는 로직을 작성하였다.

#### 문제점

메일의 아이디 패스워드가 들어가서 .env를 통해 환경변수를 관리해주었는데, nodemailer 객체를 생성하는 부분을 libs폴더로 분리하였다. 아직 Nest.JS의 동작 순서를 완벽히 이해하지 못해서 그런지 main.ts보다 libs에 작성한 console.log 가 먼저 찍혔고 아직 config모듈을 읽어오기 전이라 process.env가 undefined가 출력되었다. 이를 해결하기 위해 클로저로 싱글톤 패턴을 도입하였다.

즉시실행함수로 함수를 반환하는 함수를 만들어주고, mailer이 없을 경우 새로 생성해서 반환해준다. 이렇게 하면 내가 사용할 때, 즉 서버가 완전히 구동되고 난 다음에 nodemailer 모듈이 생성되기 때문에 잘 작동했다. createTransport의 반환값은 `nodemailer.Transporter<STMPtransport.SentMessageInfo>` 라고 타입이 정해져있엇는데 STMPtransport 를 찾을 수 없다고 해서 nodemailer객체의 SentMessageInfo타입으로 지정해주었지만 SentMessageInfo가 any타입으로 설정되어 이 부분을 좀 더 찾아봐야겠다.

```ts
// libs/email
import * as nodemailer from "nodemailer";

export default (() => {
  let mailer: nodemailer.Transporter<nodemailer.SentMessageInfo>;
  return () => {
    if (!mailer) {
      mailer = nodemailer.createTransport({
        service: "Gmail",
        auth: {
          user: process.env.GOOGLE_ID,
          pass: process.env.GOOGLE_PASSWORD,
        },
        tls: {
          rejectUnauthorized: false,
        },
      });
    }
    return mailer;
  };
})();
```

### 마무리

메일 인증을 구현해보았는데 처음 구현했지만 라이브러리와 설명글이 잘 작성되어 있었다. 처음 구현해보며 테스트할 때 메일이 날라오는 걸 보고 신기했다 ㅎㅎ
하지만 아직 Nest.JS에 대해 잘 모르는 것 같다. 백엔드 로직을 본격적으로 작성하기 전에 Nest.JS에 대해 공부하고 정리하는 시간이 필요할 것 같다.
