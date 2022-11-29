### ✨ git 공부

#### 주요 CLI 명령어

길어서 접어놨음

<details>
<summary> 펼치기 </summary>
<!--  -->
  
<br/>
  
**1. 현재 작업중인 폴더 확인**

`pwd` : print working directory

            현재 작업중인 폴더의 절대경로가 출력

**2. 폴더 생성**

`mkdir` : Make Directory

`mkdir {디렉토리 이름}`

- `mkdir Frontend`: 현재 폴더에 `Frontend`폴더를 생성

**3. 디렉토리 이동**

`cd` : change Directory

`cd {디렉토리 경로}`

- `cd .` - 현재 디렉토리 (생략 가능)
- `cd ..` - 상위 경로로 한 단계 이동
- `cd ../..` - 상위 경로로 두 단계 이동
- `cd ~/Desktop` - 데스크탑 디렉토리로 바로 이동

**4. 디렉토리 및 파일 목록 출력**

`ls` : List Segments

`ls {디렉토리 경로}{옵션}`

- `ls ~/Frontend/assets` : `Frontend/assets` 폴더의 하위 폴더 목록을 출력
- `ls -l ~/Frontend/assets` : 폴더 목록을 출력할 때 사용 권한, 소유자, 그룹, 크기, 날짜 등 상세 정보를 함께 표시
- `ls -a ~/Frontend/assets` : 폴더 목록을 출력할 때 숨겨진 항목을 포함하여 모든 내용을 출력
- `ls -al ~/Frontend/assets` : 폴더 목록을 출력할 때 숨겨진 항목을 포함하여 사용 권한, 소유자, 그룹, 크기, 날짜 등 상세 정보를 함께 표시

**5. 파일 생성**

`touch` : 빈 파일을 생성할 경우

`echo` : 간단한 내용이 들어있는 파일을 생성할 경우

- `$ touch index.html`: 내용이 없는 빈 `index.html`파일 생성
- `$ echo 'let me = "Frontend Developer"' > js/index.js`

       js 폴더안에  `let me = "Frontend Developer"` 라는 코드가 삽입된  `index.js`파일 생성

**6. 파일 내용 확인하기**

`cat` : Concatenate

- `cat js/index.js` : `index.js`파일의 내용을 화면에 출력
- `cat index.js app.js` : `index.js`파일의 내용으로 `app.js`파일 내용 덮어쓰기

**7. 파일/(비어있지 않은)디렉토리 삭제**

`rm` : Remove

`rm {제거할 파일/디렉토리 이름}`

- `rm index.html` : `index.html`파일 삭제
- `rm -r js` : js폴더 내부 하위 디렉토리까지 모두 삭제
- `$ rm -rf assets` : `assets`폴더 안의 하위 디렉토리까지 모두 삭제하되, 경고를 나타내지 않음

**8. 디렉토리 제거**

`rmdir` : Remove Directory

`rmdir {제거할 디렉토리 이름}`

- `$rmdir js`: `js` 폴더 삭제

**9. 파일/디렉토리 이동 및 이름 변경**

`mv` : Move(이미 존재하는 파일/디렉토리의 경우 이름 변경이 가능)

- `mv index.html views/index.html`: `index.html` 파일을 `views`폴더로 이동
- `mv js/index.js js/app.js` :`js` 폴더에 있는 `index.js` 파일명을 `app.js`로 변경

**10. 파일/디렉토리 복사**

`cp` : Copy

- `cp index.html main.html`:`index.html`파일을 동일한 폴더에 복사한 후 파일명을 `main.html` 로 변경
- `cp index.html views/main.html` :`index.html`파일을 `views` 폴더에 복사한 후 파일명을  `main.html` 로 변경
</details>

### Git Command

<details>
<summary>펼치기</summary>

#### Git 최초 설정

Git을 설치하고 나면 Git의 사용 환경을 적절하게 설정해 주어야 합니다. 환경 설정은 한 컴퓨터에서 한 번만 하면 되고 설정한 내용은 Git을 업그레이드해도 유지됩니다.

```bash
# Git 사용자 ID
git config --global user.name "seulbinim"
# Git 사용자 Email
git config --global user.email seulbinim@gmail.com
# Git Default Editor 설정 (Visual Studio Code)
git config --global core.editor "code --wait"

# windows와 Mac OS의 공백문자(줄바꿈) (Carriage return, Lind Feed)
# Windows 환경
git config --global core.autocrlf true
# Mac OS 환경
git config --global core.autocrlf input
```

또한 언제든지 설정 값을 'git config’라는 도구로 확인하고 변경할 수 있습니다.

#### Git 주요 커맨드

1. `git init` : 저장소 생성

   ```bash
   # 현재 디렉토리를 Git 저장소로 생성
   # .git 폴더(숨김 폴더)가 생성됨
   git init
   ```

2. `git status` : 현재 상태 확인

   ```bash
   git status

   # 변경된 파일명이 빨간색으로 보일 경우 Working Directory 상태
   # 변경된 파일명이 초록색으로 보일 경우 Staging Area 상태
   # nothing to commit, working tree clean의 경우 변경 내용이 없음을 나타냄
   ```

3. `git diff` : 파일의 변경내용 비교하기

   ```bash
   # difftool을 사용하여 파일의 변경내용을 비교
   git config --global -e
   [diff]
   	tool = vscode
   [difftool "vscode"]
   	cmd = code --wait --diff $LOCAL $REMOTE

   git difftool
   ```

4. `git add` : 파일의 변경 사항을 index(Staging Area)에 추가

   ```bash
   # 특정 파일을 Staging Area에 추가하기
   git add <file>
   # 변경 내용이 있는 모든 파일을 Staging Area에 추가하기
   git add *
   git add .
   ```

5. `git restore <file>` : 작업 내용 취소

   ```bash
   # Working Directory에 변경 내용을 취소할 경우(Tracked File)
   git restore <file>
   # Staging Area에 변경 내용을 Working Directory로 되돌릴 경우
   git restore --staged <file>
   ```

6. `git commit -m`  : 파일의 변경 사항에 대한 이력 생성

   ```bash
   # 마지막 커밋 메시지 수정
   git commit --amend "수정할 커밋 메시지"
   또는
   git commit --amend
   # VS Code에 COMMIT_EDITMSG 창에 수정할 커밋 메시지 입력 후 창 닫기
   ```

7. `git rebase -i <hash>`  : 특정 커밋 수정

   ```bash
   # pick -> reword로 수정한 후 커밋 메시지 수정
   reword <hash> "수정할 커밋 메시지"
   ```

8. `git log` : 커밋 이력 확인

   ```bash
   # Log를 한줄, 그래프 형태로 보기
   git log --oneline
   git log --oneline --graph
   ```

9. `git checkout HEAD~` : 과거 커밋 이력 확인

   ```bash
   # 이전 2개의 커밋으로 이동
   git checkout HEAD~2
   # 특정 커밋으로 이동
   git checkout <hash>
   # 마지막 커밋으로 복귀
   git checkout main
   ```

10. `git reset HEAD~` : 이전 상태로 복원(이력 제거)

    ```bash
    # 이전 2개의 커밋으로 돌아가기 (--mixed : default)
    # 커밋 기록은 삭제되지만 Working Directory에 변경 사항은 남김
    git reset HEAD~2
    # 커밋 기록은 삭제되지만 Working Directory와 Staging Area에 변경 사항은 남김
    git reset --soft HEAD~2
    # HEAD~2 커밋으로 복원되며 이후에 변경된 커밋 기록은 모두 삭제
    git reset --hard HEAD~2
    ```

11. `git branch` : 브랜치 생성 및 이동

    ```bash
    # likelion이라는 브랜치를 생성
    git branch likelion
    # likelion이라는 브랜치로 이동
    # checkout 명령이 여러 기능을 가지고 있기때문에 Git 2.23.0 버전에서는
    # 브랜치 이동을 위한 기능으로 switch 명령이 추가 됨 (checkout, switch 모두 사용 가능)
    git checkout likelion
    git switch likelion
    # main 브랜치로 복귀
    git switch main
    # likelion 브랜치를 main 브랜치로 병합
    git merge likelion
    # likelion 브랜치 삭제
    git branch -d likelion
    ```

12. `git reset HEAD~` : 이전 상태로 복원(이력 제거)

    ```bash
    # 이전 2개의 커밋으로 돌아가기 (--mixed : default)
    # 커밋 기록은 삭제되지만 Working Directory에 변경 사항은 남김
    git reset HEAD~2
    # 커밋 기록은 삭제되지만 Working Directory와 Staging Area에 변경 사항은 남김
    git reset --soft HEAD~2
    # HEAD~2 커밋으로 복원되며 이후에 변경된 커밋 기록은 모두 삭제
    git reset --hard HEAD~2
    ```

13. `git remote` : 리모트(Remote) 브랜치

    ```bash
    # 리모트 브랜치 조회
    git remote -v
    # 리모트 브랜치 추가
    git remote add origin <https://github.com/ID/REPOSITORY>
    # 리모트 브랜치 삭제
    git remote remove origin
    git remote rm origin
    ```

14. `git push` : 로컬의 변경 이력을 리모트로 전송

    ```bash
    # 로컬의 main 브랜치의 변경 이력을 리모트 main 브랜치로 보내기
    git push origin main
    ```

15. `git pull` : 리모트의 내용을 로컬에 반영 `(fetch + merge)`

</details>
