---
{"dg-publish":true,"permalink":"/02-concepts/spring/spring-security/authorization/pre-authorize/","tags":["type/concept","area/concepts","tech/spring","tech/security"],"dg-note-properties":{"type":"concept","tags":["type/concept","area/concepts","tech/spring","tech/security"]}}
---


# @PreAuthorize

메서드 레벨에서 인가된 사용자만 해당 요청을 보낼 수 있도록 하는 애너테이션

- `@EnableMethodSecurity` 를 아무 Configuration.class에 붙여야 함

- Controller/Service의 메서드가 실행되기 전에 권한 검사(Authorization) 수행

+) `@PostAuthorize` : 메서드를 실행하고, 응답을 보내기 전에 권한 확인

## 동작 원리

---

1. Spring AOP 기반 프록시가 Controller/Service 메서드 호출을 가로 챔

1. @ProAuthorize() 표현식을 평가 (`SpEL 키워드`)

1. 조건을 만족하면 메서드 그대로 실행, 만족하지 않으면 AccessDeniedException 처리
   + 403 Forbidden 응답

## 자주 사용하는 SpEL 키워드

---

### 권한 관련

- hasRole(’ADMIN’)                                                        → 앞에 ‘USER_’ 안 붙어야 함.

- hasAnyRole(’ADMIN’, ‘STFF’)

- hasAuthority(’USER_STAFF’)                                     → 앞에 ‘USER_’ 붙어야 함

- hasAnyAuthority(’USER_STAFF’, ‘USER_ADMIN’)

### 인증 상태

- isAuthenticated() → 로그인 된 사용자

- isAnonymous() → 익명 사용자

### principal 접근

- principal → Authentication.getPrincipal()

- authentication → Authentifa

## Authz

---

@PreAuthorize 를 위해서 따로 생성한 빈

- @Component(”authz”)  설정

- SePL 키워드로 원하는 역할을 가진 복합 조건식을 만들고 사용할 수 있음

- 원하는 Controller/Service 메서드 위에, `@PreAuthorize("@authz.isStaff(authentication)")`

## 사용

---

```java
// AdminController (/api/admin)

// 전체 유저 조회 (임원 이상만)
@GetMapping("/users")
@PreAuthorize("@authz.isStaff(authentication)")
    public ResponseEntity<AllStudentDetailResponse> getUsers() {
        AllStudentDetailResponse response = studentService.getUserDetails();
        return ResponseEntity.ok(response);
    }
```

```java
@Component("authz")
public class Authz {

    // 임원 이상: MANAGER, PRESIDENT, SYSTEM_ADMIN
    public boolean isStaff(Authentication auth) {
        StudentRole role = extractRole(auth);
        return role != null && role.isStaff();
    }

    // 관리자 이상: PRESIDENT, SYSTEM_ADMIN
    public boolean isAdmin(Authentication auth) {
        StudentRole role = extractRole(auth);
        return role != null && role.isAdmin();
    }

    private StudentRole extractRole(Authentication auth) {
        if (auth == null || !auth.isAuthenticated()) return null;
        Object p = auth.getPrincipal();
        if (p instanceof UserAuth ua) return ua.getRole();
        return null;
    }
}
```

## 참고

---

- [https://velog.io/@kyoung0161/SpringPreAuthorize%EC%9D%98-%ED%95%9C%EA%B3%84-%EC%BB%A4%EC%8A%A4%ED%85%80-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98-AOP%EB%A1%9C-%ED%95%B4%EA%B2%B0](https://velog.io/@kyoung0161/SpringPreAuthorize%EC%9D%98-%ED%95%9C%EA%B3%84-%EC%BB%A4%EC%8A%A4%ED%85%80-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98-AOP%EB%A1%9C-%ED%95%B4%EA%B2%B0)
