## ✨ github 다른 사람 파일 가져오는 방법 두 가지

### 🎇 npx degit

깃헙에서 가져오고 싶은 파일만 뽑아오는 방법임 이거 맨날 찾았는데 드디어 알게 됨

```js
npx degit chae/webcafeHTML5/assets src

npx degit 계정이름/레포지토리 이름/특정폴더 이이름의폴더를만들겠어
```

<br/>

### 🎇 다른 사람 파일 가져와서 내 레포랑 연결하기

1. 다른 사람 레포를 클론해온다.
2. 임의로 만들어놓은 내 레포랑 연결(이때 오리진말고 다른 이름으로 해야됨. 내가 오리진이 아니니까?)

```
$ git remote add test https://github.com/chaehaeun/test.git
```

3. 테스트라는 이름의 레포의 main 이라는 브랜치로 푸시

```
$ git push test main
```
