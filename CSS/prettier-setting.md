### prettier ignore

reset 파일을 저장할 때마다 프리티어가 **예쁘게** 만들어주는게 너무 거슬려서 알아봤다.
![](https://velog.velcdn.com/images/chaehe_3210/post/bbd5ceae-513f-414f-adc8-f8a96875d19a/image.png)
이런 식으로 놔두고 싶다구ㅠㅠ

해결 방법은 간단하다.
프로젝트의 root 폴더에 `.prettierignore` 라는 파일을 만들고

```
*reset.css
```

를 기입하고 저장하면 아주 쉽게 해결된다. 프리티어가 **해당 파일만** 정리하지 않게 됨!

해당 파일 확장자를 전부 예외 처리하고 싶다면

```
*.css
```

를 쓰면 된다!
