---
title: "{ Spring, React } Spring(gradle) + React 프로젝트 연동"
date: 2024-12-09 03:00:00 KST
tags: [Spring, React, Gradle, proxy, CORS]
---

여기 주어진 내용을 바탕으로 깔끔하고 간결한 노트로 정리해 보았습니다.

---

## **Spring(gradle) + React 프로젝트 연동**

### Spring Project(gradle) 세팅

### Controller 클래스 생성

### Proxy 설정

- **필수 설정**: React (포트 3000)와 Spring (포트 8080) 간의 통신을 위해 필요.
- **CORS 문제 해결**: 브라우저 보안 정책에 따라 도메인 또는 포트가 다른 서버 간 요청을 제한하는 CORS 이슈를 해결.
- **package.json 설정**:
  ```json
  "proxy": "http://localhost:8080",
  ```

### Gradle을 이용한 프로젝트 빌드

- 프로젝트에 맞게 frontendDir, from, 및 into 경로 설정 맞추기
- **CI/CD 환경 구축**: 지속적 통합 및 배포 파이프라인 설정.
- **Build Script**: React 프로젝트 빌드 후 결과물을 Spring Boot 빌드 아티팩트에 포함.
  ```groovy
  def frontendDir = "$projectDir/app-frontend"
  ```

sourceSets {
main {
resources {
srcDirs = ["$projectDir/src/main/resources"]
}
}
}

processResources {
dependsOn "copyReactBuildFiles"
}

task installReact(type: Exec) {
workingDir "$frontendDir"
	inputs.dir "$frontendDir"
group = BasePlugin.BUILD_GROUP
if (System.getProperty('os.name').toLowerCase(Locale.ROOT).contains('windows')) {
commandLine "npm.cmd", "audit", "fix"
commandLine 'npm.cmd', 'install'
} else {
commandLine "npm", "audit", "fix"
commandLine 'npm', 'install'
}
}

task buildReact(type: Exec) {
dependsOn "installReact"
workingDir "$frontendDir"
	inputs.dir "$frontendDir"
group = BasePlugin.BUILD_GROUP
if (System.getProperty('os.name').toLowerCase(Locale.ROOT).contains('windows')) {
commandLine "npm.cmd", "run-script", "build"
} else {
commandLine "npm", "run-script", "build"
}
}

task copyReactBuildFiles(type: Copy) {
dependsOn "buildReact"
from "$frontendDir/build"
	into "$buildDir/resources/main/static"
} ```

### Spring Boot 애플리케이션 설정

- **BackendApplication.java**: 애플리케이션의 시작점.
- **ServletInitializer**: SpringApplicationBuilder를 사용해 애플리케이션 구성.

### MVC 모델의 Controller

- **ApiController.java**: 클라이언트 요청 처리 및 응답 반환.

  ```java
  @RestController
  public class ApiController {
      @Autowired
      private UserRepository userRepository;

      @GetMapping("/api/users")
      public ResponseEntity<List<UserVO>> getAllUsers() {
          List<UserVO> list = userRepository.findAll();
          return ResponseEntity.ok().body(list);
      }
  }
  ```

### VO (Value Object)

- **UserVO.java**: 데이터 전송 객체로, 데이터베이스 테이블 `users`를 나타냄.
  ```java
  @Getter
  @Setter
  @Entity
  @Table(name = "users")
  @SequenceGenerator(name="uno_seq_gen", sequenceName="user_seq")
  public class UserVO {
      @Id
      @GeneratedValue(strategy=GenerationType.SEQUENCE, generator="uno_seq_gen")
      private int uno;
      private String id;
      private String password;
      @CreationTimestamp
      private Timestamp regdate;
      @UpdateTimestamp
      private Timestamp updatedate;
  }
  ```

### Service Layer

- **UserService.java**: 비즈니스 로직과 데이터 처리를 담당.

  ```java
  @Service
  public class UserService {
      @Autowired
      private UserRepository userRepository;

      public UserVO createUser(String id, String password) {
          UserVO user = new UserVO();
          user.setId(id);
          user.setPassword(password);
          return userRepository.save(user);
      }
  }
  ```

### DAO (Data Access Layer)

- **UserRepository.java**: JpaRepository를 확장하여 CRUD 및 기타 필요한 데이터 접근 메서드 제공.
  ```java
  public interface UserRepository extends JpaRepository<UserVO, Integer> {
  }
  ```
