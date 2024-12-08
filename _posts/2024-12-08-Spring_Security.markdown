---
title: "{ Spring } Spring Security"
date: 2024-12-08 03:00:00 KST
tags: [Spring, JWT, Spring Security]
---

## **Spring Security**

---

### **Authentication (인증)**

#### 1. **Basic Authentication**

- **동작 방식**: 사용자 이름과 비밀번호를 Base64로 인코딩하여 `Authorization` 헤더에 포함하여 전송.
- **특징**:
  - SSL/TLS와 함께 사용하지 않으면 안전하지 않음.
  - 사용 예:
    ```
    Authorization: Basic <Base64_encoded_credentials>
    ```

#### 2. **Bearer Token Authentication**

- **동작 방식**: `Authentication` 헤더에 JWT와 같은 토큰을 포함하여 전송.
- **특징**:
  - **장점**: 간단한 방식, 상태 비유지, 확장성 높음.
  - **단점**: 토큰이 노출될 경우 보안 위험, 토큰 관리가 어려움.
  - 사용 예:
    ```
    Authorization: Bearer <Token>
    ```

#### 3. **OAuth (OAuth 2.0)**

- **동작 방식**: 사용자가 자격을 직접 증명하지 않고, 미리 인증받아 발급된 토큰을 이용하여 API 요청.
- **특징**:
  - 주로 소셜 로그인 (Kakao, Naver, GitHub, Facebook 등)에서 사용.
  - 토큰 기반 인증으로 안전성과 확장성이 우수.

#### 4. **API Key / Session Based Authentication**

- **API Key**: 정적인 키를 사용하여 요청 인증.
- **Session Based**: 서버에서 상태를 유지하며 사용자 인증.

---

### **JWT (JSON Web Token)**

- **정의**: 클레임(claim)이라 불리는 정보를 JSON 형태로 안전하게 전송하기 위한 토큰 기반의 표준.
- **용도**: 인증 및 정보 교환.
- **구조**:
  1. **Header**: 토큰 타입과 알고리즘 정보 포함 (Base64Url 인코딩).
  2. **Payload**: 클레임 정보 (대상, 발행자, 만료 시간 등) 포함 (Base64Url 인코딩).
  3. **Signature**: Header, Payload와 Secret Key를 사용하여 생성된 서명.

#### **장점**

- 상태 비유지 (Stateless).
- 간단하고 자기 포함적 (Self-contained).
- 확장성 우수 (여러 서비스에서 동일 토큰 사용 가능).

#### **단점**

- **크기 문제**: 클레임이 많을수록 토큰 크기 증가.
- **보안 문제**: 서명만 되어 있고, 암호화되지 않음 → 민감한 정보를 포함하면 안 됨.
- **토큰 관리**: 만료 시간 설정 및 갱신 관리 필요.

---
