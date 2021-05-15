---
layout: post
title: "[Web] Intersection Observer을 사용한 무한스크롤"
date: 2021-05-15 16:10:00 +0900
categories: [Study, Web]
tags: [infinity scroll, intersection observer]
comments: true
---

### Intersection Observer

> IntersectionObserver은 타겟 엘리먼트와 타겟의 부모 혹은 상위 엘리먼트의 뷰포트가 교차되는 부분을 비 동기적으로 관찰하는 API

- ViewPort
  뷰 포트는 현재 화면에 보여지고 있는 다각형의 영역

즉, Intersection Observer이란 화면에 내가 지정한 타겟 엘리먼트가 보이는지를 비동기적으로 관찰하는 API이다.

### Intersection Observer vs Scroll Event

`Intersection Observer`로 무한 스크롤을 구현할 때 어떤 장점이 있을지 알아보기 위해 스크롤 이벤트로 구현한 방식과 비교해 보았다.

1. throttle, debounce를 사용하지 않아도 된다.

   ```javascript
   window.addEventListener('scroll',()=>console.log('scroll')});
   ```

   위의 코드처럼 스크롤에 이벤트를 등록하였을 때, 스크롤을 해보면 'scroll'이 엄청 많이 찍힌다.
   JS의 스레드는 싱글 스레드이기 때문에 이벤트 감지 후 콜백을 실행하는 자체도 하나의 스레드 안에 들어가게 되므로 이벤트 핸들러가 많을 수록 성능 저하를 일으킨다.
   이를 방지하기 위해 debounce나 throttle를 적용시켜야 한다.

   > debounce: 입력이 종료했을 때 처리한다.
   > throttle: 일정시간동안의 입력을 모하서 한번에 처리한다.

2. reflow를 하지 않는다.

   특정 지점을 알기 위해서는 getBoundingClientRect()함수를 사용해야 하는데, 이때 reflow가 발생한다.

   > reflow : 브라우저가 웹 페이지의 일부 또는 전체를 다시 그리는 경우.

### 사용방법

```text
new IntersectionObserver (callback, options)
```

- callback : 관찰이 시작되는 시점에서 실행되는 함수. 2개의 파라미터를 가진다

  ```text
  entries: IntersectionObserverEntry 객체들을 배열로 반환
  observer: IntersectionObserver 인스턴스
  ```

- options : 관찰이 시작되는 시점에 대한 옵션을 지정할 수 있다. 그러나 기본값이 존재하므로 생략해도 상관없다.

  ```text
  root: 대상의 가시성을 확인하기 위해 뷰포트로 사용되는 요소
  default: null

  rootMargin: 루트요소의 마진값
  default: 0px

  thredhold: root 엘리먼트와 타겟 엘리먼트가 얼만큼 교차되었는지
  default: 0
  ```

- methods
  - observe(target): target 관찰
  - unobserve(target): 관찰 종료
  - disconnect(target): 관찰 멈추기

### 적용 코드

targetEl과 observer를 ref로 선언해준다음, observer이 없으면 새로 만들어주고 있으면 반환하게 만들어 준다. isIntersecting가 true가 되면 fetchMore을 실행시켜 list를 추가해준다.

```javascript
const List = () => {
  const targetEl = useRef(null);
  const observer = useRef(null);
  const [list, setList] = useState([]);
  const fetchMore = () => {
    const newData = getList(listLength);
    setList((preList) => preList.concat(newData));
  };
  const getObserver = useCallback(() => {
    if (!observer.current) {
      observer.current = new IntersectionObserver(fetchMore);
    }
    return observer.current;
  }, [observer.current]);
  useEffect(() => {
    if (targetEl.current) getObserver().observe(targetEl.current);
    return () => getObserver().disconnect();
  }, [targetEl.current]);
  return (
    <>
      {list.length &&
        list.map(({ id, content }) => <div key={id}>{content}</div>)}
      <div ref={targetEl} />
    </>
  );
};
```

### 마무리

reflow를 발생시키지 않아 랜더링 성능적 이점도 가지고 갈 수 있고, 따로 이벤트 연속 호출에 대한 처리를 해주지 않아도 되서 편리하게 사용할 수 있는 것 같다.
간단하게 구현을 해보며 처음에 이해하기 힘들었지만 사용하면서 편리하다는 것을 느꼈다.
