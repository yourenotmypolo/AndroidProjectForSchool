# 모바일 프로그래밍 시험 - 객관식 4지선다 문제

> **시험 형태**: 객관식 4지선다형 (개념 문제 중심)
> **총 30개 문제** - Week 09부터 Week 14까지

---

## Week 09: RecyclerView (문제 1-5)

### 1. RecyclerView와 ListView의 차이로 가장 부정확한 것은?
① RecyclerView는 완전한 뷰 재사용을 지원하지만 ListView는 불완전한 재사용만 가능
② RecyclerView는 다양한 LayoutManager를 지원하지만 ListView는 수직 배치만 가능
③ RecyclerView는 ViewHolder 패턴을 선택사항으로 지원하고 ListView는 필수
④ RecyclerView는 현대적 안드로이드 표준이고 ListView는 레거시 컴포넌트

**정답: ③** (RecyclerView는 ViewHolder 필수, ListView는 선택)

---

### 2. ViewHolder 패턴의 주요 목적으로 가장 적절한 것은?
① 데이터베이스 접근을 효율적으로 하기 위함
② View 객체를 캐싱하여 반복적인 findViewById() 호출 제거
③ 네트워크 요청을 비동기로 처리하기 위함
④ 화면 회전 시 데이터를 보존하기 위함

**정답: ②**

---

### 3. onCreateViewHolder()와 onBindViewHolder()의 호출 특성 비교에서 옳은 것은?
① 둘 다 매번 스크롤할 때마다 호출된다
② onCreateViewHolder()는 화면에 표시될 아이템 수만큼 호출되고, onBindViewHolder()는 매번 스크롤할 때 호출
③ onBindViewHolder()는 한 번만 호출되고 onCreateViewHolder()는 계속 호출
④ 두 함수 모두 한 번만 호출되고 이후로는 호출되지 않음

**정답: ②**

---

### 4. RecyclerView에서 notifyDataSetChanged() 사용을 피해야 하는 이유는?
① 메인 스레드를 블록하기 때문
② 1개 아이템만 변경되어도 전체 리스트를 재렌더링하므로 비효율적
③ 메모리 누수를 유발하기 때문
④ 데이터베이스 접근 권한이 없기 때문

**정답: ②**

---

### 5. 다음 중 GridLayoutManager와 StaggeredGridLayoutManager의 차이를 가장 정확히 설명한 것은?
① GridLayoutManager는 아이템 높이가 다를 수 있지만 StaggeredGridLayoutManager는 같음
② GridLayoutManager는 모든 아이템이 같은 높이이고, StaggeredGridLayoutManager는 아이템 높이가 다를 수 있음
③ 두 LayoutManager는 기능적으로 동일함
④ GridLayoutManager는 오래된 방식이고 StaggeredGridLayoutManager는 새로운 방식일 뿐 성능 차이는 없음

**정답: ②**

---

## Week 10: Room Database (문제 6-10)

### 6. Room Database의 3가지 핵심 구성 요소가 아닌 것은?
① Entity - 테이블 정의
② DAO - 데이터 접근 방법
③ Context - 전체 데이터베이스
④ Database - 데이터베이스 홀더

**정답: ③** (Database가 맞음, Context는 구성 요소 아님)

---

### 7. @PrimaryKey(autoGenerate = true)의 역할로 가장 적절한 것은?
① 데이터베이스 연결을 자동으로 생성
② 각 행을 자동으로 증가하는 유일한 ID로 식별
③ 데이터를 자동으로 백업
④ 쿼리를 자동으로 생성

**정답: ②**

---

### 8. Room Database에서 Singleton 패턴을 사용해야 하는 가장 중요한 이유는?
① 성능을 향상시키기 위함
② 데이터베이스는 전체 앱에서 1개만 존재해야 데이터 무결성이 보장되기 때문
③ 메모리 누수를 방지하기 위함
④ 네트워크 연결을 유지하기 위함

**정답: ②**

---

### 9. suspend 함수와 일반 함수의 차이로 가장 정확한 것은?
① suspend 함수는 더 빠르고 일반 함수는 더 느림
② suspend 함수는 메인 스레드를 블록하지 않고 Coroutine 내에서만 호출 가능, 일반 함수는 어디서나 호출 가능
③ suspend 함수는 UI 업데이트만 가능하고 일반 함수는 데이터 처리만 가능
④ 두 함수는 기능적으로 완전히 동일함

**정답: ②**

---

### 10. @ColumnInfo 애노테이션의 주요 용도는?
① 데이터베이스 연결을 지정하기 위함
② Kotlin 프로퍼티명과 데이터베이스 컬럼명이 다를 때 커스텀 컬럼명을 지정
③ 데이터 타입을 자동으로 변환하기 위함
④ 캐싱 정책을 설정하기 위함

**정답: ②**

---

## Week 11: Flow (문제 11-15)

### 11. Flow와 LiveData의 비교에서 가장 정확한 것은?
① Flow는 AndroidX 라이브러리이고 LiveData는 Kotlin 표준
② Flow는 Kotlin Coroutines 기반이고 LiveData는 AndroidX 기반
③ 두 라이브러리는 기능이 완벽히 동일함
④ Flow는 레거시이고 LiveData가 모던 앱 표준

**정답: ②**

---

### 12. Flow의 "냉 스트림(Cold Stream)" 개념의 의미로 가장 적절한 것은?
① Flow가 항상 값을 방출하고 있음
② Flow는 수집(collect)할 때만 값을 방출하고, 구독하지 않으면 아무일도 일어나지 않음
③ Flow는 항상 메모리에 데이터를 저장함
④ Flow는 자동으로 업데이트되지 않음

**정답: ②**

---

### 13. Flow 수집 시 repeatOnLifecycle을 반드시 사용해야 하는 가장 주요한 이유는?
① 더 빠른 성능을 위해
② 메모리 누수를 방지하고 배터리를 절약하며 Lifecycle을 인식하기 위함
③ 데이터를 암호화하기 위해
④ UI 업데이트 속도를 제어하기 위해

**정답: ②**

---

### 14. Room DAO에서 suspend 함수와 Flow 반환의 차이로 가장 정확한 것은?
① 둘 다 한 번에 값을 반환함
② suspend는 호출할 때마다 1회 값을 반환하고, Flow는 데이터 변경 시 계속 값을 방출하며 자동 업데이트
③ Flow는 suspend보다 항상 느림
④ suspend는 UI 업데이트만 가능하고 Flow는 데이터 저장만 가능

**정답: ②**

---

### 15. 다음 중 Flow를 사용하는 가장 적절한 사용 사례는?
① 일회성 데이터 저장
② 데이터 변경을 감지하여 UI를 자동으로 실시간 업데이트하는 경우
③ 단순 계산 수행
④ 네트워크 요청 한 번만 수행

**정답: ②**

---

## Week 12: ViewModel (문제 16-20)

### 16. ViewModel이 화면 회전 후에도 데이터를 유지할 수 있는 이유는?
① Activity가 저장하기 때문
② ViewModel은 Activity와 독립적으로 ViewModelStore에 저장되므로 Activity 재생성 후에도 같은 인스턴스 유지
③ 자동으로 데이터베이스에 저장되기 때문
④ SharedPreferences에 저장되기 때문

**정답: ②**

---

### 17. StateFlow와 MutableStateFlow의 차이로 가장 정확한 것은?
① 두 클래스는 기능이 완전히 같음
② StateFlow는 읽기 전용이고 MutableStateFlow는 읽고 쓰기 가능하며, 캡슐화를 위해 MutableStateFlow는 private, StateFlow는 public으로 사용
③ MutableStateFlow는 읽기 전용이고 StateFlow는 쓰기 가능
④ StateFlow는 ViewModel에서만 사용 가능하고 MutableStateFlow는 Activity에서만 사용

**정답: ②**

---

### 18. ViewModel 초기화 시 by viewModels()를 사용하는 이유는?
① 더 짧은 코드를 작성하기 위해
② 자동으로 ViewModel을 생성하고 화면 회전 시 재생성하지 않으며 수동으로 관리할 필요 없음
③ 데이터베이스 연결을 자동으로 관리하기 위해
④ 네트워크 요청을 자동으로 처리하기 위해

**정답: ②**

---

### 19. repeatOnLifecycle(Lifecycle.State.STARTED)의 의미로 가장 적절한 것은?
① Activity가 생성될 때만 관찰
② Activity가 사용자에게 보일 때(onStart() 이후)부터 관찰하며, Paused 상태에서는 일시 중단
③ Activity가 포커스를 가질 때만 관찰
④ 항상 관찰

**정답: ②**

---

### 20. ViewModel의 onCleared() 메서드가 호출되는 경우는?
① Activity가 onPause()될 때
② Activity가 화면 회전할 때
③ Activity가 완전히 종료되고 백 버튼을 눌렀을 때
④ 매번 UI 업데이트될 때

**정답: ③**

---

## Week 13: MVVM & Repository (문제 21-25)

### 21. MVVM 아키텍처에서 가장 하위 계층(Data Layer) 위의 계층은?
① UI Layer
② ViewModel Layer
③ Repository Layer
④ Network Layer

**정답: ③**

---

### 22. Repository 패턴이 필요한 가장 중요한 이유는?
① 코드를 더 길게 작성하기 위함
② 데이터 소스를 추상화하여 테스트 용이성을 높이고 데이터 소스 변경(Room→Retrofit)이 쉬움
③ 성능을 향상시키기 위함
④ 메모리 사용량을 줄이기 위함

**정답: ②**

---

### 23. ListAdapter와 기존 Adapter의 가장 큰 차이는?
① ListAdapter가 더 간단함
② ListAdapter는 submitList()로 자동 차이 계산(DiffUtil) 및 부분 업데이트를 하지만, 기존 Adapter는 notifyDataSetChanged()로 전체 갱신
③ 두 Adapter는 기능이 동일함
④ ListAdapter는 XML에서만 사용 가능하고 기존 Adapter는 Compose에서만 사용

**정답: ②**

---

### 24. XML과 Jetpack Compose에서 ViewModel 사용의 가장 주요한 공통점은?
① 두 UI 프레임워크가 완전히 같은 구조
② ViewModel은 동일하게 사용 가능하며, 차이는 UI 관찰 방식(repeatOnLifecycle vs collectAsState)뿐
③ XML과 Compose는 호환되지 않음
④ ViewModel을 XML에서는 사용하지만 Compose에서는 사용하지 않음

**정답: ②**

---

### 25. MVVM에서 ViewModel 계층이 필요한 가장 주요한 이유는?
① UI와 데이터 처리 로직을 분리하여 테스트 가능성을 높이고 상태 관리를 중앙화
② 코드를 더 복잡하게 만들기 위함
③ 성능을 향상시키기 위함
④ 메모리 사용량을 줄이기 위함

**정답: ①**

---

## Week 14: Retrofit (문제 26-30)

### 26. Retrofit이 "선언형 API"라고 불리는 이유는?
① 더 빠른 속도를 제공하기 때문
② "어떻게 할지"가 아니라 "무엇을 할지"만 명시하고 Retrofit이 HTTP 요청 방법을 구현
③ 코드가 길기 때문
④ 자동으로 성능을 최적화하기 때문

**정답: ②**

---

### 27. Gson의 주요 역할로 가장 정확한 것은?
① HTTP 요청을 전송
② 데이터베이스와 연결
③ JSON 응답을 자동으로 Kotlin 데이터 클래스로 변환(역직렬화)
④ 네트워크 속도를 높임

**정답: ③**

---

### 28. REST API의 HTTP 메서드 중 데이터를 조회하는 용도는?
① POST
② PUT
③ GET
④ DELETE

**정답: ③**

---

### 29. API 응답의 JSON을 Kotlin 데이터 클래스로 매핑할 때 가장 중요한 조건은?
① JSON 구조가 복잡할수록 좋음
② Kotlin 클래스의 필드명이 JSON의 키와 정확히 일치해야 하고, 타입도 일치, 중첩 구조도 동일해야 함
③ 필드명은 달라도 상관없음
④ 순서만 같으면 됨

**정답: ②**

---

### 30. 네트워크 요청 시 상태 관리의 일반적인 패턴으로 가장 적절한 것은?
① 단순히 데이터만 저장
② isLoading = true (스피너 표시) → 요청 → 성공시 data 업데이트, 실패시 errorMessage 표시 → finally에서 isLoading = false (스피너 숨김)
③ 요청 시에만 상태 관리
④ 에러만 관리하고 성공은 관리하지 않음

**정답: ②**

---

## 📊 객관식 정답 요약

| 문제 | 정답 | 주제 |
|------|------|------|
| 1-5 | ③②②②② | Week 09: RecyclerView |
| 6-10 | ③②②②② | Week 10: Room Database |
| 11-15 | ②②②②② | Week 11: Flow |
| 16-20 | ②②②②③ | Week 12: ViewModel |
| 21-25 | ③②②②① | Week 13: MVVM |
| 26-30 | ②③③②② | Week 14: Retrofit |

---

## 🎯 정답률 자가 진단

- **27-30점**: 🌟 완벽한 이해
- **24-26점**: ⭐ 좋은 이해
- **21-23점**: 👍 기본 이해
- **18-20점**: 📚 추가 학습 필요
- **15-17점**: 🔄 개념 복습 권장

**시험 잘 봐요! 💪**