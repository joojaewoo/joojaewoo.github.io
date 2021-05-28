---
layout: post
title: "[ToyProject] 카카오톡 클론 프로젝트(5)"
date: 2021-05-28 16:22:00 +09:00
categories: [Development, Clone]
tags: [typescript, next.js, emotion]
comments: true
---

오늘은 프로필 페이지 UI를 구현하고 메인에서 친구 목록 카드 UI에 대해 만들어 보았다.

### 프로필 페이지

이미지 컴포넌트와 버튼 컴포넌트를 조합하여 프로필 페이지를 구현하였고, 상단의 X버튼은 `router.back()`를 통해 뒤로가기 버튼으로 만들어 주었다.

![profile](/assets/img/posts/kakao04.png)

#### Image Component

이미지 컴포넌트로 `name, imageUrl`을 props로 내려주는데 imageUrl이 없으면 3항 연산자를 사용해서 기본 이미지로 SVG ICON 컴포넌트를 렌더링 해주었다.

```tsx
// Profile/ImageComponent
<Container>
  {imageUrl ? <img src={imageUrl} alt="profile" /> : <UserIcon />}
</Container>
```

프로필 사진의 위치를 잡기 위해 `position: absolute`로 설정하고 알맞은 위치에 이미지를 위치시켰다. `& > svg, img`를 통해 Container 컴포넌트 내부에 있는 img와 svg에 대해서 크기를 설정해줘서 위와 같이 UI를 구현할 수 있었다.

#### Button Component

카카오톡을 보니 내 프로필일떄는 나에게 대화, 프로필 수정 버튼이 존재했고, 친구 프로필일때는 1:1대화 하기 버튼이 존재했다. 프로필 페이지에서 내 프로필인지 아닌지 판단해서 버튼 컴포넌트에게 boolean 값을 넘겨주고 해당 값에 따라 버튼을 다르게 보여주었다.

```tsx
<>
  {isMyProfile ? (
    <Container>
      <ButtonContainer>
        <Button type="button">
          <ChatIcon />
        </Button>
        <TextContainer>나와의 채팅</TextContainer>
      </ButtonContainer>
      <ButtonContainer>
        <Button type="button">
          <EditIcon />0
        </Button>
        <TextContainer>프로필 수정</TextContainer>
      </ButtonContainer>
    </Container>
  ) : (
    <Container>
      <ButtonContainer>
        <Button type="button">
          <ChatIcon />
        </Button>
        <TextContainer>1:1 채팅</TextContainer>
      </ButtonContainer>
    </Container>
  )}
</>
```

글을 작성하면서 생각하니 친구가 아니면 친구 추가라는 버튼을 만들어 줘야한다는 생각이 들었고, 컴포넌트를 더욱 분리해서 내일 구현해 봐야겠다.

### 친구 목록 컴포넌트

친구 목록 컴포넌트를 구현했다. 아래의 사진은 이를 map를 통해서 여러개를 렌더링한 모습이다.

이미지와 문자가 들어갈 부분으로 나누고 `display: flex`속성을 통해 가로로 렌더링 해주었다. 여기도 마찬가지로 프로필 이미지가 없으면 3항 연산자를 통해 기본 이미지를 보여주도록 작성하였다.

프로필 이미지와 친구목록에서의 이미지 모두에 이미지 컴포넌트가 사용되었고 이를 내일 다시 리팩토링을 통해 분리해볼 예정이다.

![friendlist](/assets/img/posts/kakao05.png)

### DB 스키마

원래 MongoDB를 통해 모든 데이터를 관리하려고 했는데, DB 스키마를 어떻게 작성하면 좋을지 생각이 잘 나지않아 다른분께 여쭤보았다. 채팅 로그만 MongoDB에 작성하고, 수정이 많이 일어날 수 있는 사용자 정보에 대해서는 RDB로 관리하는 것도 좋을 것 같다는 의견을 들었다. 이 부분에 대해서 조금 더 생각해봐야겠다.

잘 모르겠으면 먼저 inmemory방식으로 채팅을 구현하고 나중에 DB연동을 하는 방법도 고려중이다. 내가 프로젝트를 진행하는 이유가 소켓 통신을 사용해보려고 하는 것이 가장 큰 이유인데, 다른 부분에 너무 많은 시간을 소요하는 것 같아 소켓 통신을 통해 채팅을 구현하고 나중에 기능을 추가해도 좋을 것 같아 고려중이다.
