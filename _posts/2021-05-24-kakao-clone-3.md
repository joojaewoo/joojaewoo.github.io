---
layout: post
title: "[ToyProject] 카카오톡 클론 프로젝트(3)"
date: 2021-05-24 15:21:00 +09:00
categories: [Development, Clone]
tags: [nest.js, typescript, eslint, preitter, next.js, emotion, apollo-client]
comments: true
---

오늘은 로그인 페이지 UI와 로그인 API를 구현해 보았다.

### 로그인 페이지 UI

먼저 로그인 페이지 UI를 구현해 보았다. 처음 `emotion`을 사용하는데 `emotion/styled`를 사용하니 `styled-components`를 사용하는 것과 같은 방식으로 사용하면 되어서 어려운 점은 없었다.

이메일과 비밀번호를 입력받을 `input`창과 로그인 버튼을 하나의 컴포넌트로 만들고, 카카오톡 로고를 하나의 컴포넌트로 만들어 두개를 합쳐 로그인 페이지를 만들었다. input창을 만들 때 `useState`와 `useRef`중 뭘 사용해야 더 좋을지 몰라서 일단 `useState`로 하고 조금 찾아본 다음에 더 좋은 방법을 선택해야겠다.

내가 만든 로그인 페이지 UI는 다음과 같다.

![image](/assets/img/posts/kakao02.png)

### golbal Theme

모바일 환경에 맞게 UI를 작성하기 위해 global Theme를 설정하였다. 미디어 쿼리를 설정해서 화면 크기가 일정 크기 이상일때 max-width, max-height를 설정해 줘서 모바일 크기로 보여주었다.

### Apollo Client

로그인 요청을 보내기 위해 apollo client를 사용했다. 싱글톤 패턴을 사용하기 위해 클로저를 사용해서 전역변수 사용을 피했다. 아직 캐시 정책에 대해서는 설정하지 못했다. 프론트와 백엔드를 다른 포트를 사용하기 위해 cors 설정을 해주었고 cookie로 jwt를 저장하기 위해 credentials를 include로 설정해주었다. credentials가 includes이면 origin에 \*(wild card)를 사용할 수 없어 정확한 서버 주소를 적어두었다. 이후 dotenv를 사용해서 하드코딩 된 부분을 리팩토링 해야겠다.

```typescript
export default (() => {
  let apolloClient: ApolloClient<NormalizedCacheObject>;
  return () => {
    if (!apolloClient) {
      apolloClient = new ApolloClient({
        cache: new InMemoryCache({}),
        link: createHttpLink({
          uri: "http://localhost:4000/graphql",
          credentials: "include",
        }),
      });
    }
    return apolloClient;
  };
})();
```

### authProvider

사용자가 로그인했는지 확인하기위해 authProvider을 구현했다. 현재 내정보를 받아오는 요청을 서버로 보내고 해당 요청이 에러가 발생하거나 해당 요청의 pathname에 login 또는 callback가 있을 경우 login 페이지로 보내주었고, 아닐경우 children을 표시해 주었다.

```typescript
const AuthProvider: FunctionComponent<Props> = ({ children }) => {
  const router = useRouter();
  const { data, error } = useQuery(GET_MYINFO);

  if (router.pathname.includes("login") || router.pathname.includes("callback"))
    return <>{children}</>;

  if (data) return <>{children}</>;
  if (error) router.push("/login");
  return <div>loading</div>;
};
```

### Storybook

컴포넌트를 만든 다음 스토리북을 만들어 보았는데 잘 되지 않았다. input컴포넌트 내에서 useMutation을 통해 서버로 요청을 보내는데 이 부분이 있으니 스토리북이 생성되지 않았다. 추후 찾아봐야겠다. 또한 요청 보내는 부분을 제외하고 생성하면 잘 생성이 되었지만, globalStyle는 적용되지 않아 storybook설정을 한번 공부해 봐야될 것 같다.

---

로그인 페이지를 만들어서 이제 로그인 API를 구현해보았다. Nest.JS가 익숙하지 않아 간단한 API를 만드는데 조금 애를 먹었다.

### MongoDB 연동

먼저 DB에서 데이터를 받아오기위해 `@nestjs/mongoose mongoose`를 다운받아주었다. 이후 app.module에 MongooseModule를 추가해주므로 MongoDB와 서버를 연결할 수 있었다.

```typescript
@Module({
  imports: [
    MongooseModule.forRoot('mongodb://localhost:27017/kakao-clone', {
      useNewUrlParser: true,
      useCreateIndex: true,
      useUnifiedTopology: true,
      useFindAndModify: false,
    }),
    ...
  ]
})
```

#### 로그인 API

사용자가 가지고 있어야 할 정보들을 model로 정의하고 graphQL을 사용하기 때문에 resolver을 작성하였다. resolver에서 service에 정의한 함수들을 받아와서 로그인 처리를 해주었다. 로그인은 쿠키에 jwt를 담아 보내주었다. 함수로 많이 작성하다가 class로 작성하니 조금 어색했다.

쿠키의 옵션은 httpOnly와 `sameSite: strict` 두가지 속성을 주었다. 쿠키로 인증한 이유는 Next.js ( 프론트 ) 에서 데이터를 프리패치를 위해 `getServerSideProps`를 사용할 경우 localStorage에 저장한다면 서버단에서 브라우저의 localStorage에 접근할 수 없기 때문에 쿠키를 사용해서 사용자 인증 정보를 클라이언트에게 주었다. 이후 accessToken과 refreshToken으로 나누는 작업을 해 볼 예정이다.

#### Directive

이전 프로젝트에서 로그인한 사용자를 구분하기위해서 각 리졸버마다 중복적인 확인 로직을 작성해야 했지만, Directive를 사용해서 `@Directive('@auth)`를 적는것 만으로도 구분할 수 있었다.
나는 간단하게 사용자 권한만 인증하면 되기 때문에 쿠키의 jwt토큰을 verify해서 그 정보를 context로 넘겨주고 정보가 없다면 'Not Authenticated' 에러를 발생시켜주었다

```typescript
export class AuthDirective extends SchemaDirectiveVisitor {
  visitFieldDefinition(field: GraphQLField<any, any>) {
    const { resolve = defaultFieldResolver } = field;
    field.resolve = async function (...args) {
      const [_, __, { authUser }] = args;
      if (authUser) {
        return await resolve.apply(this, args);
      }
      throw Error("not Authenticated");
    };
  }
}
```

### Trouble Shooting

jwt 인증을 구현하는 부분에서 아래처럼 jsonwebtoken을 불러와서 사용했는데 sign property가 undefined라는 에러가 발생했다. nestjs의 문제인지 어디의 문제인지는 잘 모르겠다. 두번째 처럼 사용하니 잘 작동했다.
어디서 생긴 문제인지 잘 찾아봐야 겠다.

```typescript
import jwt from "jsonwebtoken"; // Error
import * as jwt from "jsonwebtoken"; // 정상적 작동
```

---

오늘 처음 Nest.js로 코드를 작성해보았는데 작동은 되는데 잘 만든지는 잘 모르겠다. 내일은 회원가입 페이지와 회원가입 API를 만들어봐야겠다.
