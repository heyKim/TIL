# Git Command
## Git Repository 생성하기

### Git Remote Repository 저장소 받아오기 
```git clone 사용자명@호스트:/원격/저장소/경로```

### 변경사항 적용
* 흐름: 작업디렉토리(working directory) => 인덱스(staging area) => Head(최종 확정본) => Remote Repository 반영




## Git Remote Repository 변경 하기
### 기존 Repository 깔끔하게 pull / push
```
git pull
git add .
git commit -m "clean push"
git push
```

### 기존 Repository remote 제거
`fatal: remote origin already exists.`  
문제: 이미 remote git이 설정되어있다는 메세지
해결: 설정된 Repository를 지우고 새로 셋팅  
`git remote remove origin`

### 새 Repository remote 추가
```
git remote add origin https://github.com/playauto/리포지토리명
```

* Commit message 수정하기: `git commit --amend -m "New commit message"`