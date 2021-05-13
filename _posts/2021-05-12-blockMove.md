---
layout: post
title: "[프로그래머스] LV.3 블록 이동하기 (JS) "
tags: [bfs, programmers, algorithm, javascript]
date: 2021-05-12 19:13:00 +0900
categories: [Study, Algorithm]
comments: true
---

### 문제

로봇개발자 "무지"는 한 달 앞으로 다가온 "카카오배 로봇경진대회"에 출품할 로봇을 준비하고 있습니다. 준비 중인 로봇은 2 x 1 크기의 로봇으로 "무지"는 "0"과 "1"로 이루어진 N x N 크기의 지도에서 2 x 1 크기인 로봇을 움직여 (N, N) 위치까지 이동 할 수 있도록 프로그래밍을 하려고 합니다. 로봇이 이동하는 지도는 가장 왼쪽, 상단의 좌표를 (1, 1)로 하며 지도 내에 표시된 숫자 "0"은 빈칸을 "1"은 벽을 나타냅니다. 로봇은 벽이 있는 칸 또는 지도 밖으로는 이동할 수 없습니다. 로봇은 처음에 아래 그림과 같이 좌표 (1, 1) 위치에서 가로방향으로 놓여있는 상태로 시작하며, 앞뒤 구분없이 움직일 수 있습니다.

![image1](/assets/img/posts/2021-05-12-1.png)

로봇이 움직일 때는 현재 놓여있는 상태를 유지하면서 이동합니다. 예를 들어, 위 그림에서 오른쪽으로 한 칸 이동한다면 (1, 2), (1, 3) 두 칸을 차지하게 되며, 아래로 이동한다면 (2, 1), (2, 2) 두 칸을 차지하게 됩니다. 로봇이 차지하는 두 칸 중 어느 한 칸이라도 (N, N) 위치에 도착하면 됩니다.

로봇은 다음과 같이 조건에 따라 회전이 가능합니다.

![image2](/assets/img/posts/2021-05-12-2.png)

위 그림과 같이 로봇은 90도씩 회전할 수 있습니다. 단, 로봇이 차지하는 두 칸 중, 어느 칸이든 축이 될 수 있지만, 회전하는 방향(축이 되는 칸으로부터 대각선 방향에 있는 칸)에는 벽이 없어야 합니다. 로봇이 한 칸 이동하거나 90도 회전하는 데는 걸리는 시간은 정확히 1초 입니다.

"0"과 "1"로 이루어진 지도인 board가 주어질 때, 로봇이 (N, N) 위치까지 이동하는데 필요한 최소 시간을 return 하도록 solution 함수를 완성해주세요.

### 제한사항

- board의 한 변의 길이는 5 이상 100 이하입니다.
- board의 원소는 0 또는 1입니다.
- 로봇이 처음에 놓여 있는 칸 (1, 1), (1, 2)는 항상 0으로 주어집니다.
- 로봇이 항상 목적지에 도착할 수 있는 경우만 입력으로 주어집니다.

### 풀이

2020 카카오 블라인드 코딩 테스트에 출제된 문제이다. 최단 경로 문제는 목적지까지 가는 문제이기 때문에 BFS 알고리즘을 사용하여 접근했다. 일반적인 미로 찾기에서 로봇의 크기가 2이고 회전도 가능하기 떄문에 이를 고려해서 작성했다. 로봇의 크기가 2이기 떄문에 큐에 로봇의 두 좌표와 거리를 저장했다.

#### Map 구현

좌표값이 1,1 부터 시작하므로 좌표를 맞추기 위해 새로운 Map를 생성한다.

```javascript
const N = board.length;
const newBoard = Array.from(Array(N + 2), () => Array(N + 2).fill(1));
for (let i = 0; i < N; i++) {
  for (let j = 0; j < N; j++) {
    newBoard[i + 1][j + 1] = board[i][j];
  }
}
```

#### BFS

BFS로 문제를 해결하기위해 queue를 초기화 해준다.

```javascript
const queue = [];
queue.push({ head: { x: 1, y: 1 }, tail: { x: 1, y: 2 }, dist: 0 });
...
while(queue.length>0){
...
}
```

> 자바스크립트에는 queue가 없기 떄문에 배열의 shift()를 통해 제일 앞 요소를 삭제하여 사용할 수 있다. queue의 자료구조와 달리 시간이 더 오래걸려 데이터가 많아 시간초과가 발생할 경우 queue를 구현하여 사용해야 한다.

#### 로봇의 움직임

로봇은 4방향으로 이동하는 것과 회전하는 경우를 생각해줘야 한다. 로봇이 움직일 수 있는 모든 경우를 나열해보면 아래와 같다.

1. 위
2. 아래
3. 오른쪽
4. 왼쪽
5. head를 축으로 시계방향 회전
6. head를 축으로 반 시계방향 회전
7. tail을 축으로 시계방향 회전
8. tail을 축으로 반 시계방향 회전

```javascript
const getNextPos = ([x1, y1], [x2, y2], board, dist) => {
  const arr = [];
  const dir = [
    [-1, 0],
    [1, 0],
    [0, 1],
    [0, -1],
  ];
  dir.forEach(([X, Y]) => {
    const head = [x1 + X, y1 + Y];
    const tail = [x2 + X, y2 + Y];
    if (board[head[0]][[head[1]]] === 0 && board[tail[0]][[tail[1]]] === 0)
      arr.push({ head, tail, dist: dist + 1 });
  });
  const rotate = [1, -1];
  rotate.forEach((value) => {
    if (x1 === x2) {
      //가로로 위치한 경우
      if (board[x1 + value][y1] === 0 && board[x2 + value][y2] === 0) {
        arr.push({ head: [x1, y1], tail: [x1 + value, y1], dist: dist + 1 });
        arr.push({ head: [x2 + value, y2], tail: [x2, y2], dist: dist + 1 });
      }
    } else {
      // 세로로 위치한 경우
      if (board[x1][y1 + value] === 0 && board[x2][y2 + value] === 0) {
        arr.push({ head: [x1, y1], tail: [x1, y1 + value], dist: dist + 1 });
        arr.push({ head: [x2, y2 + value], tail: [x2, y2], dist: dist + 1 });
      }
    }
  });
  return arr;
};
```

#### Visit

현재 지점을 방문했는지 알기 위해 visit을 체크해야 하는데 2차원 배열이기 떄문에 visit를 set로 만들고 각 좌표를 문자열로 만들어 set에 추가해주었다. getNextPos의 반환값으로 받아온 배열의 방문 여부를 판단해서 큐에 넣어준다.

```javascript
const nextPos = getNextPos(head, tail, newBoard, dist, visit);
for (const next of nextPos) {
  const { head: nextHead, tail: nextTail } = next;
  if (!visit.has(nextHead.join("") + nextTail.join(""))) {
    visit.add(nextHead.join("") + nextTail.join(""));
    queue.push(next);
  }
}
```

#### 전체코드

```javascript
const solution = (board) => {
  const queue = [];
  const N = board.length;
  const goal = (N + "" + N).toString();
  const newBoard = Array.from(Array(N + 2), () => Array(N + 2).fill(1));
  for (let i = 0; i < N; i++) {
    for (let j = 0; j < N; j++) {
      newBoard[i + 1][j + 1] = board[i][j];
    }
  }
  queue.push({ head: [1, 1], tail: [1, 2], dist: 0 });
  const visit = new Set(["1112"]);
  while (queue.length > 0) {
    const { head, tail, dist } = queue.shift();
    if (head.join("") === goal || tail.join("") === goal) return dist;
    const nextPos = getNextPos(head, tail, newBoard, dist, visit);
    for (const next of nextPos) {
      const { head: nextHead, tail: nextTail } = next;
      if (!visit.has(nextHead.join("") + nextTail.join(""))) {
        visit.add(nextHead.join("") + nextTail.join(""));
        queue.push(next);
      }
    }
  }
};

const getNextPos = ([x1, y1], [x2, y2], board, dist) => {
  const arr = [];
  const dir = [
    [-1, 0],
    [1, 0],
    [0, 1],
    [0, -1],
  ];
  dir.forEach(([X, Y]) => {
    const head = [x1 + X, y1 + Y];
    const tail = [x2 + X, y2 + Y];
    if (board[head[0]][[head[1]]] === 0 && board[tail[0]][[tail[1]]] === 0)
      arr.push({ head, tail, dist: dist + 1 });
  });
  const rotate = [1, -1];
  rotate.forEach((value) => {
    if (x1 === x2) {
      //가로로 위치한 경우
      if (board[x1 + value][y1] === 0 && board[x2 + value][y2] === 0) {
        arr.push({ head: [x1, y1], tail: [x1 + value, y1], dist: dist + 1 });
        arr.push({ head: [x2 + value, y2], tail: [x2, y2], dist: dist + 1 });
      }
    } else {
      // 세로로 위치한 경우
      if (board[x1][y1 + value] === 0 && board[x2][y2 + value] === 0) {
        arr.push({ head: [x1, y1], tail: [x1, y1 + value], dist: dist + 1 });
        arr.push({ head: [x2, y2 + value], tail: [x2, y2], dist: dist + 1 });
      }
    }
  });
  return arr;
};
```

로봇의 크기가 2이고 회전까지 가능해서 까다롭게 느껴졌다. 방문을 어떻게 표시할지 몰라 시간이 많이 걸렸던 문제이다. 검색을 통해 좌표를 문자열로 변환하고 set을 통해 방문여부를 판단하는 방식을 보고 도입해서 해결할 수 있었다. 풀고나서 보니 기존의 BFS 문제와 별반 다를 것이 없다는 생각이 들었다.
