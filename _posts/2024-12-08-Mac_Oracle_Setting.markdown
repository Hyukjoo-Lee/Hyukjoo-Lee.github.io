---
title: "{ MAC } 맥 오라클 DB 세팅 + 스프링부트 연결"
date: 2024-12-08 03:00:00 KST
tags: [MAC, Spring, Oracle]
---

### **Oracle DB와 Spring Boot 연결 (MAC)**

---

#### **1. Docker 설치**

- Docker 다운로드: [Docker 공식 웹사이트](https://www.docker.com/get-started)
- Mac용 다운로드 버튼 클릭 (Apple Silicon용 선택)

---

#### **2. Oracle 19c Docker 이미지 준비**

1. **GitHub 레포지토리 클론**
   ```bash
   git clone https://github.com/oracle/docker-images.git
   ```
   - Docker 파일 위치 예시:
     `/Users/jaydon/github_repos/docker-images/OracleDatabase/SingleInstance/dockerfiles/19.3.0`

---

#### **3. Oracle Docker 이미지 실행**

1. **Docker 이미지 확인**

   ```bash
   docker images
   ```

2. **Oracle 19c 컨테이너 실행**
   ```bash
   docker run -d --name oracle19 -e ORACLE_PWD=password -p 1521:1521 oracle/database:19.3.0-ee
   ```
   - 컨테이너 ID가 출력 확인

---

#### **4. Colima 환경 설정 (x86_64 사용자만 해당)**

- Colima 설정:
  ```bash
  colima start --memory 4 --arch x86_64
  ```

---

#### **5. Oracle 컨테이너 상태 확인**

1. **실행 중인 컨테이너 확인**

   ```bash
   docker ps
   ```

2. **컨테이너 내에서 SQL\*Plus 실행**
   ```bash
   docker exec -it oracle19 bash
   sqlplus sys/password@ORCLCDB as sysdba
   ```

---

#### **6. SQL\*Plus로 Oracle 사용자 생성**

1. **제약 설정 변경**

   ```sql
   ALTER SESSION SET "_ORACLE_SCRIPT"=TRUE;
   ```

2. **사용자 생성 및 권한 부여**
   ```sql
   CREATE USER username IDENTIFIED BY password;
   GRANT CONNECT, RESOURCE TO username;
   GRANT UNLIMITED TABLESPACE TO username;
   COMMIT;
   ```

---

### **Spring Boot와 Oracle DB 연결**

#### **1. Oracle JDBC 드라이버 다운로드**

- 다운로드: [ojdbc8.jar 다운로드](https://www.oracle.com/database/technologies/appdev/jdbc-downloads.html)
- Maven 사용 시 properties => Build Path => libary => import external ...
- Gradle 은 Maven 과 다르게 의존성 주입만 해도 충분히 동작 하는 것으로 보임

#### **2. Spring Boot 프로젝트 설정**

1. **Gradle Dependency 추가 (build.gradle)**

   ```gradle
   dependencies {
       implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
       implementation 'org.springframework.boot:spring-boot-starter-security'
       implementation 'org.springframework.boot:spring-boot-starter-validation'
       implementation 'org.springframework.boot:spring-boot-starter-web'
       runtimeOnly 'com.oracle.database.jdbc:ojdbc11'
       annotationProcessor 'org.projectlombok:lombok'
       compileOnly 'org.projectlombok:lombok'
       testImplementation 'org.springframework.boot:spring-boot-starter-test'
       testImplementation 'org.springframework.security:spring-security-test'
   }
   ```

2. **DB 설정 (application.yml)**

   ```yaml
   server:
     port: 8080

   spring:
     datasource:
       driver-class-name: oracle.jdbc.OracleDriver
       url: jdbc:oracle:thin:@localhost:1521:ORCLCDB
       username: username
       password: password
     thymeleaf:
       cache: false

     jpa:
       database-platform: org.hibernate.dialect.OracleDialect
       open-in-view: false
       show-sql: true
       hibernate:
         ddl-auto: update
   ```

---

#### **3. Lombok 설정**

###### **Lombok 추가**

- `setting.json` 수정:
  ```json
  {
    "java.compile.nullAnalysis.mode": "automatic",
    "java.configuration.updateBuildConfiguration": "interactive",
    "java.jdt.ls.vmargs": "-javaagent:/path/to/lombok.jar"
  }
  ```

---

#### **4. DB 연결 테스트**

##### **테스트 코드 작성 (OracleConTest.java)**

```java
package com.example;

import java.sql.Connection;
import java.sql.DriverManager;
import org.junit.jupiter.api.Test;

public class OracleConTest {

    private static final String DRIVER = "oracle.jdbc.OracleDriver";
    private static final String URL = "jdbc:oracle:thin:@localhost:1521:ORCLCDB";
    private static final String USER = "username";
    private static final String PW = "password";

    @Test
    public void testCon() throws Exception {
        Class.forName(DRIVER);

        try (Connection con = DriverManager.getConnection(URL, USER, PW)) {
            System.out.println("DB 연결 성공: " + con);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

---
