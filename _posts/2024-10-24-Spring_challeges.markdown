---
title: "{ Spring } Spring_Setting"
date: 2024-10-24 03:00:00 KST
tags: [Spring]
---

# VSC Spring 프로젝트 세팅

## 1. 익스텐션 설치

- **Spring Boot Extension Pack**
- **Spring Boot Developer Extension Pack**
- **Thymeleaf**

## 2. 명령 팔레트 실행

- `Ctrl + Shift + P` 를 눌러 명령 팔레트를 실행합니다.
- `Spring Initializr : Create a gradle Project` 검색 후 선택합니다.
- 사용할 **버전**, **언어**, **프로젝트 이름**, **의존성**을 선택한 후 `Generate`를 클릭합니다.

---

## 자바 버전 맞추기

### mac에서 JDK 17로 설정하는 방법

1. `nano ~/.zshrc` 파일을 열어 환경 변수를 설정합니다.

```bash
# Java 11
export JAVA_HOME=$(/usr/libexec/java_home -v 17.0.11)
export PATH=${PATH}:$JAVA_HOME/bin:

# Java 1.8
# export JAVA_HOME=$(/usr/libexec/java_home -v 1.8)
# export PATH=${PATH}:$JAVA_HOME/bin:

# Java 17
# export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk-17.0.10.jdk/Contents/Home"
# export PATH="$JAVA_HOME/bin:$PATH"
```
