# library-app

코틀린 + Spring Boot로 만든 **도서관리 애플리케이션**. 회원 관리, 도서 등록/대출/반납, 대출 현황·통계 조회를 다루며, QueryDSL을 사용한 동적 쿼리도 적용한다.

> 인프런 "실전! 코틀린과 스프링 부트로 도서관리 애플리케이션 개발하기" 강의 실습 프로젝트.

## 기술 스택

- Kotlin 1.9.23 / JVM 17
- Spring Boot 2.7.18 (web, data-jpa)
- QueryDSL 5.0.0 (kapt)
- H2 (runtime, 인메모리)
- jackson-module-kotlin

## 실행

```bash
./gradlew bootRun
```

- 정적 프론트엔드(React 빌드 산출물)가 `src/main/resources/static/v1`, `v2`에 포함되어 있어, 기동 후 브라우저에서 UI로 도서/회원 기능을 사용할 수 있다.

## API

### 회원 (`UserController`)

| 메서드 | 경로 | 설명 |
|---|---|---|
| POST | `/user` | 회원 등록 |
| GET | `/user` | 회원 목록 조회 |
| PUT | `/user` | 회원 이름 수정 |
| DELETE | `/user?name=` | 회원 삭제 |
| GET | `/user/loan` | 회원별 대출 이력 조회 |

### 도서 (`BookController`)

| 메서드 | 경로 | 설명 |
|---|---|---|
| POST | `/book` | 도서 등록 |
| POST | `/book/loan` | 도서 대출 |
| PUT | `/book/return` | 도서 반납 |
| GET | `/book/loan` | 대출 중인 도서 수 조회 |
| GET | `/book/stat` | 도서 통계(타입별) 조회 |

## 구조

```
com.group.libraryapp
├── LibraryAppApplication.kt
├── controller/        # book, user 컨트롤러
├── service/           # BookService, UserService
├── domain/
│   ├── book/          # Book, BookType, BookRepository
│   └── user/          # User, UserRepository(+Custom), loanhistory(UserLoanHistory, UserLoanStatus)
├── dto/               # book/user 요청·응답 DTO
├── repository/        # QueryDSL 리포지토리 (BookQuerydslRepository, UserLoanHistoryQuerydslRepository)
├── config/            # QuerydslConfig
└── util/              # ExceptionUtil
```

## 학습 포인트

- **코틀린으로 작성한 JPA 엔티티/리포지토리** (kotlin-jpa, kotlin-spring 플러그인)
- **대출/반납 도메인 로직**: `UserLoanHistory`와 `UserLoanStatus`로 대출 상태 관리
- **QueryDSL 동적 조회**: 대출 통계, 회원/대출 이력 조회에 적용
- **테스트**: JUnit 기반 단위/통합 테스트(`Calculator`, `BookService`, `UserService` 등)
