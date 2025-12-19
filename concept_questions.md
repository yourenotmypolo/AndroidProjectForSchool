# 모바일 프로그래밍 시험 - 개념 문제 예상 출제

> **주의**: 코드 문제는 제외하고 순순한 개념 문제만 다룸

---

## 📚 Week 09: RecyclerView 개념

### 1. RecyclerView와 ListView의 차이점 3가지를 설명하시오.

**예상 답안**:
1. **뷰 재사용 효율성**: RecyclerView는 완전한 뷰 재사용, ListView는 불완전한 재사용
2. **LayoutManager 유연성**: RecyclerView는 다양한 배치 가능 (Linear, Grid, Staggered), ListView는 수직만 가능
3. **어댑터 구조**: RecyclerView는 ViewHolder 필수, ListView는 선택

---

### 2. ViewHolder 패턴이 필요한 이유를 성능 측면에서 설명하시오.

**예상 답안**:
- **View 객체 캐싱**: 매번 findViewById() 호출 제거 → CPU 사용량 감소
- **메모리 효율**: 스크롤 시 기존 View 재사용 → 메모리 할당 감소
- **부드러운 스크롤**: UI 응답 시간 단축 → 프레임율 향상
- **배터리 절약**: 전체적 연산 감소 → 배터리 소모 감소

---

### 3. onCreateViewHolder()와 onBindViewHolder()의 호출 빈도 차이를 설명하시오.

**예상 답안**:
- `onCreateViewHolder()`: 화면에 표시될 아이템 수(예: 5-10개)만 호출
- `onBindViewHolder()`: 매번 스크롤할 때 호출

---

### 4. notifyDataSetChanged()를 사용하지 말아야 하는 이유를 설명하시오.

**예상 답안**:
- **전체 리스트 재렌더링**: 1개 아이템만 변경되어도 100개 모두 갱신
- **올바른 방법**: notifyItemInserted(), notifyItemRemoved(), notifyItemChanged()

---

### 5. GridLayoutManager와 StaggeredGridLayoutManager의 차이를 설명하시오.

**예상 답안**:
- **GridLayoutManager**: 모든 아이템이 같은 높이
- **StaggeredGridLayoutManager**: 아이템 높이가 다를 수 있음 (Pinterest 스타일)

---

## 📚 Week 10: Room Database 개념

### 6. Room Database의 3가지 핵심 구성 요소를 설명하시오.

**예상 답안**:

| 구성 요소 | 역할 |
|----------|------|
| **Entity** | 데이터베이스 테이블 정의 |
| **DAO** | 데이터 접근 방법 정의 |
| **Database** | 전체 DB와 DAO 제공 |

---

### 7. @Entity와 @PrimaryKey의 차이를 설명하시오.

**예상 답안**:
- `@Entity`: 이 클래스가 데이터베이스 테이블
- `@PrimaryKey`: 테이블의 유일한 식별자

---

### 8. Singleton 패턴을 Room Database에 적용해야 하는 이유는?

**예상 답안**:
- **데이터 무결성**: 데이터베이스는 하나여야 함
- **여러 인스턴스의 위험**: 각각 다른 데이터를 관리 → 데이터 손상
- **메모리 효율**: 전체 앱에서 1개 인스턴스만 유지

---

### 9. suspend 함수와 일반 함수의 차이를 설명하시오.

**예상 답안**:

| 항목 | suspend 함수 | 일반 함수 |
|------|-------------|---------|
| **호출 위치** | Coroutine 내에서만 | 어디서나 |
| **블로킹** | 메인 스레드 블록 안 함 | 메인 스레드 블록 가능 |

---

### 10. @ColumnInfo 애노테이션의 필요성을 설명하시오.

**예상 답안**:
- **커스텀 컬럼명 지정**: Kotlin 프로퍼티명과 DB 컬럼명이 다를 때 사용
- **SQL 컨벤션 준수**: 데이터베이스는 snake_case, Kotlin은 camelCase

---

## 📚 Week 11: Flow 개념

### 11. Flow와 LiveData의 주요 차이 3가지를 설명하시오.

**예상 답안**:
1. **출처**: Flow는 Kotlin Coroutines, LiveData는 AndroidX
2. **파일럿 방식**: Flow는 함수형, LiveData는 클래스 기반
3. **추천도**: Flow는 모던 앱, LiveData는 레거시

---

### 12. Flow가 "냉 스트림(Cold Stream)"이라는 것을 설명하시오.

**예상 답안**:
- **의미**: Flow는 수집(collect)할 때만 값을 방출
- **구독하지 않으면 아무 일도 일어나지 않음**

---

### 13. Flow 수집 시 repeatOnLifecycle을 사용해야 하는 이유는?

**예상 답안**:
1. **메모리 누수 방지**: Activity 종료 시 자동으로 구독 취소
2. **배터리 절약**: Activity가 Paused일 때 관찰 중단
3. **Lifecycle 인식**: 앱 상태에 따른 자동 관리

---

### 14. @Query 애노테이션에서 suspend 함수와 Flow 반환의 차이를 설명하시오.

**예상 답안**:

| 특성 | suspend 함수 | Flow 반환 |
|------|------------|---------|
| **반환** | 값을 1회 반환 | 계속 값 방출 |
| **업데이트** | 호출할 때만 최신 데이터 | 자동 업데이트 |

---

## 📚 Week 12: ViewModel 개념

### 15. ViewModel이 화면 회전 후에도 살아있는 이유를 설명하시오.

**예상 답안**:
- **저장 위치**: Activity는 재생성되지만 ViewModel은 ViewModelStore에 저장됨
- **생명주기**: Activity의 생명주기와 별개로 관리

---

### 16. StateFlow와 MutableStateFlow의 차이를 설명하시오.

**예상 답안**:

| 구분 | StateFlow | MutableStateFlow |
|------|----------|-----------------|
| **변경 가능** | ❌ 읽기 전용 | ✅ 읽고 쓰기 가능 |
| **공개 범위** | public (외부 읽기 OK) | private (내부 전용) |

---

### 17. by viewModels()로 초기화하는 이유를 설명하시오.

**예상 답안**:
- **자동 ViewModel 생성**: 필요할 때 자동으로 생성
- **화면 회전 대응**: 재생성하지 않음

---

### 18. repeatOnLifecycle(Lifecycle.State.STARTED)의 의미를 설명하시오.

**예상 답안**:
- **State.STARTED**: Activity가 사용자에게 보일 때 (최소 조건)
- 다른 옵션: CREATED, RESUMED

---

## 📚 Week 13: MVVM & Repository 개념

### 19. MVVM 아키텍처의 3계층과 각 역할을 설명하시오.

**예상 답안**:

| 계층 | 역할 |
|------|------|
| **UI Layer** | 화면 표시만 |
| **ViewModel** | 상태 관리 + 비즈니스 로직 |
| **Repository** | 데이터 소스 추상화 |
| **Data Layer** | 실제 데이터 저장 |

---

### 20. Repository 패턴이 필요한 이유를 설명하시오.

**예상 답안**:
1. **데이터 소스 추상화**: ViewModel은 데이터가 Room인지 API인지 모름
2. **테스트 용이성**: Mock Repository로 쉽게 테스트
3. **변경 용이성**: Room → Retrofit 변경 시 ViewModel은 수정 불필요

---

### 21. ListAdapter와 기존 Adapter의 차이를 설명하시오.

**예상 답안**:

| 특성 | Adapter | ListAdapter |
|------|---------|-----------|
| **업데이트** | notifyDataSetChanged() | submitList() |
| **효율성** | 전체 갱신 | 차이만 갱신 |

---

### 22. XML과 Jetpack Compose에서 ViewModel 사용의 공통점을 설명하시오.

**예상 답안**:
- **ViewModel은 동일**: 두 UI 프레임워크 모두 같은 ViewModel 사용 가능
- **차이는 UI 관찰 방식**뿐

---

## 📚 Week 14: Retrofit 개념

### 23. Retrofit이 선언형 API인 이유를 설명하시오.

**예상 답안**:
- **선언형**: "어떻게 할지" 아닌 "무엇을 할지" 명시
- Retrofit이 HTTP 요청 "방법" 구현

---

### 24. Gson의 역할을 설명하시오.

**예상 답안**:
- **JSON ↔ Kotlin 변환**
- **중요**: 클래스 필드명이 JSON 키와 정확히 일치해야 함

---

### 25. REST API의 HTTP 메서드별 용도를 설명하시오.

**예상 답안**:

| 메서드 | 작업 | 예시 |
|--------|------|------|
| **GET** | 조회 | 사용자 목록 조회 |
| **POST** | 생성 | 새 사용자 추가 |
| **PUT** | 전체 수정 | 사용자 정보 완전 변경 |
| **PATCH** | 부분 수정 | 사용자 이메일만 변경 |
| **DELETE** | 삭제 | 사용자 삭제 |

---

### 26. API 응답의 JSON 구조와 Kotlin 데이터 클래스 매핑을 설명하시오.

**예상 답안**:
- JSON 구조와 Kotlin 클래스 구조가 일치해야 함
- 필드명이 정확히 일치, 타입도 일치, 중첩 구조도 동일

---

### 27. Retrofit 클라이언트 구성의 각 부분을 설명하시오.

**예상 답안**:

| 부분 | 역할 |
|------|------|
| `baseUrl` | 모든 요청의 기본 주소 |
| `GsonConverterFactory` | JSON ↔ Kotlin 변환기 |
| `.build()` | Retrofit 인스턴스 생성 |
| `.create()` | API 인터페이스 구현 |

---

### 28. 네트워크 요청의 상태 관리(로딩/에러)를 설명하시오.

**예상 답안**:

```
Button 클릭
  ↓
isLoading = true (UI: 스피너 표시)
  ↓
API 호출 (suspend)
  ↓
성공: data 업데이트 | 실패: errorMessage 설정
  ↓
finally: isLoading = false (UI: 스피너 숨김)
```

---

### 29. 일반적인 네트워크 에러와 원인을 설명하시오.

**예상 답안**:

| 에러 | 원인 |
|------|------|
| `No address associated with hostname` | 인터넷 연결 없음 |
| `JsonSyntaxException` | JSON 구조 불일치 |
| `SecurityException` | INTERNET 권한 없음 |
| `SocketTimeoutException` | 요청 시간 초과 |

---

### 30. @Query 어노테이션의 용도를 설명하시오.

**예상 답안**:
- **쿼리 매개변수** 추가 (`?key=value` 형태)
- 다른 매개변수들: `@Path`, `@Body`, `@Header`

---

## 🎯 통합 개념 문제

### 31. Week 09 ~ 14의 진화 과정을 간단히 설명하시오.

**예상 답안**:
Week 09: RecyclerView → Week 10-11: Room + Flow → Week 12: ViewModel → Week 13: MVVM → Week 14: Retrofit

---

### 32. Room과 Retrofit을 함께 사용하는 이유를 설명하시오.

**예상 답안**:
- **캐싱**: API에서 받은 데이터를 Room에 저장
- **오프라인 지원**: 인터넷 없을 때 Room에서 읽기
- **성능**: 자주 사용하는 데이터는 Room에서 빠르게 조회

---

### 33. Activity vs ViewModel의 생명주기 차이를 설명하시오.

**예상 답안**:
- Activity: 화면 회전 시 새로 생성 (onCreate 다시 호출)
- ViewModel: 화면 회전 시 유지 (새로 생성 안 됨)

---

### 34. suspend 함수와 LiveData/Flow의 차이를 설명하시오.

**예상 답안**:

| 특성 | suspend | Flow/LiveData |
|------|--------|---|
| **반환** | 한 번에 값 반환 | 계속 값 방출 |
| **업데이트** | 호출할 때만 | 자동 업데이트 |

---

### 35. 테스트 용이성 관점에서 Repository 패턴의 중요성을 설명하시오.

**예상 답안**:
- Repository 없이: Mock 만들기 어려움
- Repository 포함: Mock Repository 쉽게 만들기 가능, 테스트 빠름

---

**모든 문제에 대해 개념을 충분히 학습하고 실제 예시와 함께 이해하는 것이 중요합니다!**