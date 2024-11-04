---
title: "{ MAC } 사용중인 포트 찾아서 Kill 하기"
date: 2024-11-05 03:00:00 KST
tags: [MAC]
---

## 1. 특정 포트를 사용하는 프로세스 찾기

**명령어:**

```bash
lsof -i :포트번호
```

**예시:**

```bash
lsof -i :3000
```

**결과 예시:**

```plaintext
COMMAND     PID   USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
node      1234 jaydon   22u  IPv4 0x9d3bf1b0582968bd      0t0  TCP *:hbci (LISTEN)
node      1234 jaydon   26u  IPv4 0x9d3bf1b057b17abd
```

위 결과에서 `PID`가 **1234**인 프로세스가 포트 3000을 사용하고 있음을 알 수 있습니다.

---

## 2. 프로세스 종료하기 (Kill)

해당 `PID`를 이용해 프로세스를 강제 종료할 수 있습니다.

**명령어:**

```bash
kill -9 PID
```

**예시:**

```bash
kill -9 1234
```

위 명령어를 입력하면 `PID`가 1234인 프로세스가 종료됩니다.

---

> ⚠️ **주의:** `kill -9` 명령은 강제 종료 명령이므로, 잘못된 프로세스를 종료하지 않도록 주의해야 합니다.
