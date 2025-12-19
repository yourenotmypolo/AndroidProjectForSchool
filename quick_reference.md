# 모바일 프로그래밍 - 빠른 참고 자료

> **시험 직전 최종 체크리스트**

---

## ⚡ 핵심 개념 1줄 정리

### Week 09: RecyclerView
- **목표**: 많은 데이터를 효율적으로 표시
- **핵심**: ViewHolder로 View 재사용 → 성능↑
- **구성**: Adapter + ViewHolder + LayoutManager
- **문제점**: notifyDataSetChanged() 비효율 → 부분 업데이트 사용

### Week 10: Room Database
- **목표**: 로컬에 데이터 영구 저장
- **핵심**: Entity/DAO/Database 3계층
- **특징**: Singleton 패턴, suspend 함수, 타입 안전
- **중요**: 데이터는 1개 인스턴스만 관리

### Week 11: Flow
- **목표**: 자동 데이터 업데이트
- **핵심**: Flow 구독 시 데이터 변경 자동 알림
- **패턴**: Room DAO에서 Flow 반환
- **필수**: repeatOnLifecycle으로 메모리 누수 방지

### Week 12: ViewModel
- **목표**: UI 상태 보존, 화면 회전 대응
- **핵심**: Activity와 독립적 생명주기
- **도구**: StateFlow로 반응형 상태 관리
- **사용**: by viewModels() 자동 생성

### Week 13: MVVM
- **목표**: 완전한 계층 분리
- **구조**: UI → ViewModel → Repository → Data
- **이점**: 테스트 용이, 유지보수 쉬움
- **같음**: ViewModel은 XML과 Compose 모두 사용 가능

### Week 14: Retrofit
- **목표**: 원격 API에서 데이터 가져오기
- **핵심**: 선언형 API, Gson 자동 변환
- **패턴**: suspend 함수로 비동기 호출
- **상태**: isLoading, data, errorMessage 3가지 관리

---

## 🔑 최중요 개념 TOP 10

### 1️⃣ ViewHolder 패턴 (Week 09)
```
스크롤 때마다 findViewById() ❌
View 객체 재사용 ✅
→ 성능↑, 배터리↑
```

### 2️⃣ Room Singleton (Week 10)
```
1개 인스턴스만 ✅
여러 인스턴스 ❌ (데이터 손상)
```

### 3️⃣ suspend 함수 (Week 10-14)
```
메인 스레드 블록 안 함
백그라운드 실행
Coroutine 내에서만 호출
```

### 4️⃣ Flow 자동 업데이트 (Week 11)
```
Room DAO: Flow<List<Student>> 반환
변경 감지 시 자동 알림
UI 수동 갱신 ❌
```

### 5️⃣ repeatOnLifecycle (Week 11-12)
```
메모리 누수 방지
배터리 절약
Lifecycle 자동 관리
```

### 6️⃣ ViewModel 생명주기 (Week 12)
```
화면 회전 후 유지 ✅
Activity는 재생성, ViewModel 유지
onCleared()에만 정리
```

### 7️⃣ StateFlow 구조 (Week 12)
```
MutableStateFlow (private, 변경)
StateFlow (public, 읽기)
캡슐화!
```

### 8️⃣ Repository 추상화 (Week 13)
```
ViewModel은 데이터 소스 모름
Room ↔ Retrofit 변경 용이
테스트 가능
```

### 9️⃣ Gson 매핑 (Week 14)
```
JSON ↔ Kotlin 자동 변환
필드명 정확히 일치 필수
중첩 구조도 동일해야 함
```

### 🔟 네트워크 상태 관리 (Week 14)
```
isLoading = true → 스피너
try: 데이터 업데이트
catch: 에러 표시
finally: isLoading = false
```

---

## 📊 아키텍처 진화

```
Week 09        Week 10-11        Week 12           Week 13          Week 14
─────          ───────           ──────            ──────           ──────
메모리만       Room + Flow       ViewModel        MVVM             Network
  ↓              ↓                 ↓                ↓                ↓
데이터 손실    영구 저장        상태 보존       계층 분리         실시간 데이터
```

### 각 주차의 "무엇"과 "왜"

| 주차 | 무엇 | 왜 |
|------|------|-----|
| 09 | RecyclerView | 많은 데이터 효율 표시 |
| 10 | Room | 데이터 영구 저장 |
| 11 | Flow | 자동 실시간 업데이트 |
| 12 | ViewModel | 상태 보존, 화면 회전 대응 |
| 13 | MVVM | 완전한 구조 분리 |
| 14 | Retrofit | 원격 API 연동 |

---

## 🚀 실전 패턴 3가지

### 패턴 1: Room 데이터 조회 + 자동 업데이트
```kotlin
// 1. DAO
@Query("SELECT * FROM students")
fun getAllStudents(): Flow<List<Student>>

// 2. Activity/Fragment
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.students.collect { students ->
            adapter.updateList(students)
        }
    }
}
```

### 패턴 2: ViewModel 상태 관리
```kotlin
// ViewModel
private val _count = MutableStateFlow(0)
val count: StateFlow<Int> = _count.asStateFlow()

fun increment() { _count.value += 1 }

// Activity
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.count.collect { count ->
            binding.textViewCount.text = count.toString()
        }
    }
}
```

### 패턴 3: Retrofit 네트워크 요청
```kotlin
// API 호출
scope.launch {
    isLoading = true
    errorMessage = ""
    try {
        val response = api.getUsers(count = 10)
        users = response.results
    } catch (e: Exception) {
        errorMessage = "에러: ${e.message}"
    } finally {
        isLoading = false
    }
}
```

---

## ❌ 자주 하는 실수

### 1. suspend 함수 호출 오류
```kotlin
❌ viewModel.addStudent(student)
✅ lifecycleScope.launch { viewModel.addStudent(student) }
```

### 2. Flow 구독 안 함
```kotlin
❌ database.studentDao().getAllStudents()
✅ database.studentDao().getAllStudents().collect { ... }
```

### 3. repeatOnLifecycle 누락
```kotlin
❌ lifecycleScope.launch { 
    viewModel.count.collect { ... }
}

✅ lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.count.collect { ... }
    }
}
```

### 4. ViewModel 매번 생성
```kotlin
❌ private val viewModel = CounterViewModel()
✅ private val viewModel: CounterViewModel by viewModels()
```

### 5. JSON 필드명 불일치
```kotlin
❌ data class User(val userName: String)
✅ data class User(
    @SerializedName("user_name")
    val userName: String
)
```

### 6. notifyDataSetChanged() 오용
```kotlin
❌ for (i in newList.indices) {
    adapter.notifyDataSetChanged()
}

✅ adapter.notifyItemRangeInserted(0, newList.size)
```

### 7. Database 매번 생성
```kotlin
❌ val db = Room.databaseBuilder(...).build()
✅ val db = StudentDatabase.getDatabase(context)
```

### 8. API 응답 매핑 오류
```json
// 서버 응답
{ "first_name": "Jane" }
```

```kotlin
❌ data class User(val firstName: String)
✅ data class User(
    @SerializedName("first_name")
    val firstName: String
)
```

---

## 📋 시험 당일 확인 사항

### 30분 전
- [ ] Week 09: RecyclerView, ViewHolder, notifyItem
- [ ] Week 10: Entity, DAO, Database, Singleton
- [ ] Week 11: Flow, suspend, repeatOnLifecycle
- [ ] Week 12: ViewModel, StateFlow, by viewModels()
- [ ] Week 13: MVVM, Repository, ListAdapter
- [ ] Week 14: Retrofit, @GET/@Query, Gson

### 10분 전
- [ ] 아키텍처 그림 머리에 그려보기
- [ ] 각 주차의 "왜"를 한 문장씩 생각하기
- [ ] 자주하는 실수 3개 다시 읽기

---

## 💡 개념 문제 풀이 팁

### 1. "왜" 질문에 답하기
```
Q: ViewModel이 필요한 이유?
A: 화면 회전 시 데이터 보존
   Activity 독립적 생명주기
   상태 관리 분리
```

### 2. 비교 질문에 표 그리기
```
Flow vs LiveData:
- 출처 (Kotlin vs AndroidX)
- 방식 (함수형 vs 클래스)
- 추천 (모던 vs 레거시)
```

### 3. 구조 질문에 그림 그리기
```
      UI
      ↓
  ViewModel
      ↓
  Repository
      ↓
  Room/API
```

### 4. 문제점 질문에 대안 제시
```
Q: notifyDataSetChanged()의 문제?
A: 전체 갱신 → 비효율
   해결: notifyItemInserted() 등 부분 업데이트
```

---

## 🎯 최종 체크리스트

### 이해해야 할 개념 (필수)
- [ ] RecyclerView vs ListView 차이
- [ ] ViewHolder 패턴의 이점
- [ ] Room의 3계층 구조
- [ ] suspend 함수의 의미
- [ ] Flow의 자동 업데이트
- [ ] ViewModel의 생명주기
- [ ] StateFlow의 구조 (private/public)
- [ ] MVVM의 4계층
- [ ] Repository 패턴의 목적
- [ ] Retrofit의 선언형 API

### 손으로 그릴 수 있어야 할 것 (중요)
- [ ] RecyclerView 구성도
- [ ] Room 3계층 다이어그램
- [ ] ViewModel 생명주기 (회전 포함)
- [ ] MVVM 아키텍처
- [ ] Retrofit 요청 흐름

### 코드 안 봐도 설명할 것 (핵심)
- [ ] notifyItem 메서드들의 차이
- [ ] Flow vs LiveData의 3가지 차이
- [ ] repeatOnLifecycle 필요 이유
- [ ] Repository 패턴의 필요 이유
- [ ] Singleton 패턴 필요 이유

---

## 🔥 최후의 조언

1. **"왜?"를 물어보기**
   - 왜 RecyclerView? → 효율성
   - 왜 suspend? → 메인 스레드 보호
   - 왜 Repository? → 테스트 용이

2. **개념과 구현 분리**
   - 구현(코드)는 중요하지 않음
   - 개념(원리)이 중요함

3. **아키텍처 이해**
   - 각 계층의 역할 명확히
   - 계층 간 의존성 이해

4. **실제 문제점 생각하기**
   - 메모리는? 배터리는? 성능은?
   - 왜 이렇게 설계했을까?

5. **예시로 생각하기**
   - "학생 관리 앱"이 계속 예시
   - 구체적 상황에 대입해보기

---

## 📌 최종 정리

**이 모든 것은 3가지 원리로 설명됨**:

1. **효율성** (Week 09-11)
   - ViewHolder: 뷰 재사용
   - Flow: 자동 업데이트

2. **보존** (Week 12)
   - ViewModel: 상태 유지
   - 화면 회전 대응

3. **분리** (Week 13)
   - MVVM: 계층 분리
   - Repository: 데이터 추상화

4. **연동** (Week 14)
   - Retrofit: 네트워크
   - 원격 서버 통합

---

**화이팅! 시험 잘 봐요! 💪**