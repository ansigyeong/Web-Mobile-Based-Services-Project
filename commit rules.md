## Commit Rules

- 반드시 개인 branch 생성한 후 작업
- merge request 요청 제대로 보내기
- git merge 명령어 조심히 쓰기



## Commit Guide for Project1

```bash
# git 시작
$ git init

# 원격 저장소랑 연결
$ git remote add origin {원격 repository 주소}

# 현재 로컬 및 원격 저장소에 연결된 branch 확인
$ git branch -a

# 원격 저장소에 있는 branch 반영
$ git remote update

# 원격 저장소에 있는 branch를 로컬에 복사 및 이동
$ git checkout -t {branch}

# branch1에서 새로운 branch2 생성
$ git checkout -b {branch2} {branch1}

# lab.ssafy에 개인이 작업한 branch push
$ git add .
$ git commit -m "{commit message}"
$ git push origin {branch}

# lab.ssafy 홈페이지에서 Merge Request 요청 - frontend팀은 frontend branch에 요청하고, backend팀은 backend branch에 요청함
```
