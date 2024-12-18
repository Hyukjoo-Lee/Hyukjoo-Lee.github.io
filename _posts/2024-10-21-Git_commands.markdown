---
title: "{ Git } Git Basic Commands"
date: 2024-10-21 03:00:00 KST
tags: [Git]
---

# Git 기본 명령어 정리

## 1. Git 설정 관련 명령어

- `git config --global user.name "사용자이름"`: Git에 사용자 이름을 설정합니다.
- `git config --global user.email "이메일주소"`: Git에 사용자 이메일을 설정합니다.

## 2. 저장소 초기화

- `git init`: 현재 디렉토리를 Git 저장소로 초기화합니다.

## 3. 파일 상태 확인

- `git status`: 파일의 현재 상태를 확인합니다.

## 4. 파일 추가 및 커밋

- `git add <파일명>`: 특정 파일을 추가합니다.
- `git add .`: 변경된 모든 파일을 추가합니다.
- `git commit -m "커밋 메시지"`: 스테이징된 파일을 커밋합니다.
- `git commit -am "커밋 메시지"`: 수정된 추적 파일을 자동으로 스테이징하고 커밋합니다.

## 5. 브랜치 관련 명령어

- `git branch`: 현재 브랜치 목록을 확인합니다.
- `git branch <브랜치명>`: 새로운 브랜치를 생성합니다.
- `git checkout <브랜치명>`: 특정 브랜치로 전환합니다.
- `git checkout -b <브랜치명>`: 새로운 브랜치를 생성하고 전환합니다.

## 6. 원격 저장소 관련 명령어

- `git clone <저장소URL>`: 원격 저장소를 복제합니다.
- `git remote add origin <저장소URL>`: 원격 저장소를 추가합니다.
- `git remote -v`: 연결된 원격 저장소의 URL을 확인합니다.
- `git push origin <브랜치명>`: 변경 사항을 원격 저장소에 푸시합니다.

## 7. 로그 확인

- `git log`: 커밋 로그를 확인합니다.
- `git log --oneline`: 한 줄로 요약된 커밋 로그를 확인합니다.

## 8. 페치(Fetch)

- `git fetch`: 원격 저장소의 최신 변경 사항을 가져옵니다.

## 9. 차이(diff) 확인

- `git diff`: 로컬 커밋과 원격 저장소의 커밋 차이를 확인합니다.

## 10. 병합(Merge)

- `git merge origin/main`: 원격 저장소의 `main` 브랜치 내용을 로컬에 병합합니다.

## 11. 현재 계정 정보 확인

- `git config --global user.name`: 전역 사용자 이름을 확인합니다.
- `git config --global user.email`: 전역 사용자 이메일을 확인합니다.
- `git config user.name`: 현재 프로젝트의 사용자 이름을 확인합니다.
- `git config user.email`: 현재 프로젝트의 사용자 이메일을 확인합니다.

## 기타 커멘드들

- `git fetch --prune` : 원격 브랜치 정보 갱신
- `git branch -a` : 모든 브랜치 표시
- `git push origin -d [브랜치이름]` : 원격 브랜치에 있는 브랜치 삭제
- `git checkout -b [브랜치이름] origin/[브랜치이름]` : 원격 브랜치를 기반으로 로컬 브랜치를 생성한 후, 그 브랜치로 이동

## 메인 로컬 브랜치와 메인 원격 브랜치의 커밋이 다른 경우 해결 방법

### 문제 상황

```bash
git status
```

````

출력:

```plaintext
On branch main
Your branch and 'origin/main' have diverged,
and have 1 and 1 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)
```

**원인:**
로컬 브랜치(`main`)와 원격 브랜치(`origin/main`)에 각각 다른 커밋이 1개씩 존재하여 충돌이 발생한 상태입니다.

---

### 문제 해결 방법: 로컬 커밋을 취소하고 원격의 최신 커밋 가져오기

1. **로컬 커밋 취소하기**

   로컬에서 이미 만든 커밋을 취소하고, 원격 커밋 상태로 브랜치를 맞추기 위해 `reset` 명령어를 사용합니다:

   ```bash
   git reset --hard origin/main
   ```

   이 명령어를 실행하면 로컬 브랜치가 원격 브랜치와 같은 상태로 맞춰지며, 로컬 커밋은 삭제됩니다.

   > ⚠️ **주의:** `--hard` 옵션을 사용하면 로컬 변경 사항이 완전히 삭제되므로, 필요한 경우 백업을 먼저 해두세요.

2. **원격 최신 상태로 맞추기**

   이제 로컬이 원격과 동일한 상태가 되었으므로, `git pull` 명령어로 원격의 최신 상태를 그대로 가져올 수 있습니다.

   ```bash
   git pull origin main
   ```
````
