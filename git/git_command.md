# Git Command
* Git Remote Repository 저장소 받아오기  
```git clone 사용자명@호스트:/원격/저장소/경로```

* 기존 Repository 깔끔하게 pull / push  
```
git pull
git add .
git commit -m "clean push"
git push
```

* 기존 Repository remote 제거  
```
# 문제: 이미 remote git이 설정되어있다는 메세지
fatal: remote origin already exists.
```  
```
# 해결: 설정된 Repository를 지우고 새로 셋팅  
git remote remove origin
```

* 새 Repository remote 추가
    * `git remote add origin https://github.com/playauto/리포지토리명`

* Commit message 수정하기
    * `git commit --amend -m "New commit message"`