---
title: "{ SQL } / SQL 기본"
date: 2024-11-20 03:00:00 KST
tags: [Mac, MariaDB, MySQL]
---

## MySQL 초기 설정 명령어 정리

### 기본 명령어

```bash
# 루트 사용자로 전환
sudo -i

# MySQL 서버 시작
mysql.server start

# 현재 데이터베이스 목록 확인
show database;

# mysql 데이터베이스 선택 (사용자 및 권한 정보 저장)
use mysql;

# 사용자(host, user) 및 비밀번호 정보 확인
select host, user, password from user;

# 'root' 사용자 비밀번호 설정
set password for 'root'@'localhost' = password('설정 할 비번');

# 서비스 종료 후 보안 설정
brew services stop mariadb
mariadb-secure-installation

# `mariadb-secure-installation` 단계별 설정
1. **Unix Socket 인증 설정**

   - 메시지: `Switch to unix_socket authentication [Y/n]`
   - 입력: `n` (건너뛰기)

2. **Root 비밀번호 변경 여부**

   - 메시지: `Change the root password? [Y/n]`
   - 입력: `n` (건너뛰기)

3. **익명 사용자 제거**

   - 메시지: `Remove anonymous users? [Y/n]`
   - 입력: `y`
   - 결과: `... Success!`

4. **Root 원격 접속 허용 여부**

   - 메시지: `Disallow root login remotely? [Y/n]`
   - 입력: `n` (건너뛰기)

5. **테스트 데이터베이스 제거**

   - 메시지: `Remove test database and access to it? [Y/n]`
   - 입력: `y`
   - 결과:
     - `Dropping test database... ... Success!`
     - `Removing privileges on test database... ... Success!`

6. **권한 테이블 재로드**

   - 메시지: `Reload privilege tables now? [Y/n]`
   - 입력: `y`
   - 결과: `... Success!`

7. **설정 완료 메시지**
   - `All done! If you've completed all of the above steps, your MariaDB installation should now be secure.`
```

### MariaDB 서비스 재시작

```bash
brew services start mariadb
```
