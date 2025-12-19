# 모바일 프로그래밍 시험 준비 - 개념 정리

> **시험 형태**: 개념 문제 중심 (코드 문제 제외)
> **시험 일정**: 2025년 12월 19일 (금요일)

---

## 1️⃣ Week 09: RecyclerView 기초

### RecyclerView의 핵심 개념

#### RecyclerView의 역할
- 대량 데이터를 효율적으로 표시하는 컨테이너
- **뷰 재사용(View Recycling)**: 화면을 벗어난 뷰를 다시 사용
- ListView의 현대적 대체재

#### RecyclerView의 4가지 핵심 구성 요소

| 구성 요소 | 역할 |
|----------|------|
| **Adapter** | 데이터 ↔ 뷰 연결, onCreateViewHolder(), onBindViewHolder() |
| **ViewHolder** | 뷰 캐싱, 불필요한 findViewById() 호출 제거 |
| **LayoutManager** | 뷰 배치 규칙 (Linear, Grid, Staggered) |
| **ItemDecoration** | 구분선, 간격 등의 시각적 효과 |

#### ViewHolder 패턴의 장점
- **성능 향상**: View 객체 재사용으로 메모리 효율 ↑
- **부드러운 스크롤**: 느린 UI 응답 시간 감소
- **배터리 절약**: CPU 사용량 감소

#### onCreateViewHolder() vs onBindViewHolder()

```
onCreateViewHolder()
- 언제: 새로운 ViewHolder 필요할 때만
- 호출 빈도: 적음 (보통 화면에 표시되는 아이템 수만큼)
- 역할: 뷰 객체 생성, ViewHolder에 저장

onBindViewHolder()
- 언제: 매번 스크롤할 때마다
- 호출 빈도: 많음 (스크롤할 때마다)
- 역할: 데이터를 기존 뷰에 바인딩
```

#### notifyItem 메서드의 중요성
- `notifyDataSetChanged()`: 전체 새로고침 (비효율)
- `notifyItemInserted(position)`: 특정 위치에 아이템 추가
- `notifyItemRemoved(position)`: 특정 아이템 삭제
- `notifyItemChanged(position)`: 특정 아이템만 업데이트

#### LayoutManager의 종류
- **LinearLayoutManager**: 수직/수평 일렬 배치
- **GridLayoutManager**: N열 격자형 배치
- **StaggeredGridLayoutManager**: 높이가 다른 격자형 배치

---

## 2️⃣ Week 10: Room Database 기초

### Room Database의 3가지 핵심 구성

#### 1. Entity (테이블 정의)
```kotlin
@Entity(tableName = "students")
data class Student(
    @PrimaryKey(autoGenerate = true)
    val id: Int = 0,
    
    @ColumnInfo(name = "student_name")
    val name: String,
    
    val department: String,
    val grade: String
)
```

**주요 애노테이션**:
- `@Entity`: 테이블로 매핑될 클래스 표시
- `@PrimaryKey(autoGenerate = true)`: 자동 증가 기본키
- `@ColumnInfo`: 데이터베이스 컬럼명 지정

#### 2. DAO (Data Access Object)
```kotlin
@Dao
interface StudentDao {
    @Query("SELECT * FROM students")
    fun getAllStudents(): Flow<List<Student>>
    
    @Insert
    suspend fun insertStudent(student: Student)
    
    @Delete
    suspend fun deleteStudent(student: Student)
}
```

**핵심 개념**:
- `suspend`: 메인 스레드 블록 없음 (비동기)
- `Flow<>`: 데이터 변경 시 자동 알림
- `@Query`: 커스텀 SQL 쿼리

#### 3. Database (Singleton)
```kotlin
@Database(entities = [Student::class], version = 1)
abstract class StudentDatabase : RoomDatabase() {
    abstract fun studentDao(): StudentDao
    
    companion object {
        @Volatile
        private var INSTANCE: StudentDatabase? = null
        
        fun getDatabase(context: Context): StudentDatabase {
            return INSTANCE ?: synchronized(this) {
                val instance = Room.databaseBuilder(
                    context.applicationContext,
                    StudentDatabase::class.java,
                    "student_database"
                ).build()
                INSTANCE = instance
                instance
            }
        }
    }
}
```

**Singleton 패턴의 중요성**:
- 데이터베이스는 **1개만 존재**해야 함
- 여러 인스턴스 = 데이터 손상 위험
- `@Volatile`: 멀티스레드 환경에서 안전한 읽기
- `synchronized(this)`: 동시성 제어

### Room의 장점
- **타입 안전**: Compile-time SQL 검증
- **자동 변환**: Kotlin ↔ SQLite 자동 매핑
- **비동기 기본**: suspend 함수 지원
- **반응형**: Flow 통한 자동 업데이트

---

## 3️⃣ Week 11: Advanced Room + Flow

### Flow 개념

#### Flow란?
- **반응형 스트림**: 시간에 따라 값을 방출
- **자동 업데이트**: 데이터 변경 시 자동으로 알림
- **냉 스트림**: 수집(collect)할 때만 활성화

#### Flow vs LiveData

| 항목 | Flow | LiveData |
|------|------|----------|
| **출처** | Kotlin Coroutines | AndroidX |
| **방식** | 함수형 | 클래스 기반 |
| **구독 해제** | 수동 | Lifecycle 자동 |
| **추천** | 모던 앱 | 레거시 앱 |

#### Flow의 활용 패턴
```kotlin
// 1. Room DAO에서 Flow 반환
@Query("SELECT * FROM students")
fun getAllStudents(): Flow<List<Student>>

// 2. Activity에서 수집
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        database.studentDao().getAllStudents().collect { students ->
            adapter.updateList(students)
        }
    }
}
```

### Coroutine의 suspend 함수

#### suspend란?
- "이 함수는 오래 걸릴 수 있으니 백그라운드에서 실행"
- 메인 스레드 블록 없음
- UI 프리징 방지

#### 언제 suspend 사용?
- ✅ 데이터베이스 쓰기/삭제
- ✅ 네트워크 요청
- ✅ 파일 I/O
- ❌ UI 업데이트 (메인 스레드 필수)

#### 잘못된 사용 예시
```kotlin
// ❌ 나쁜 예: suspend 함수를 일반 함수에서 호출
fun addStudent(student: Student) {
    database.studentDao().insertStudent(student)
}

// ✅ 좋은 예: coroutine 내에서 호출
fun addStudent(student: Student) {
    lifecycleScope.launch {
        database.studentDao().insertStudent(student)
    }
}
```

### Flow 수집 시 repeatOnLifecycle 필수인 이유
- **메모리 누수 방지**: Activity 종료 시 자동 취소
- **배터리 절약**: Paused 상태에서 관찰 중단
- **Lifecycle 인식**: 앱 상태에 따른 자동 관리

---

## 4️⃣ Week 12: ViewModel + StateFlow

### ViewModel의 핵심 역할

#### ViewModel이란?
- **UI 상태 관리**: 비즈니스 로직 캐싱
- **Configuration Change 대응**: 화면 회전해도 데이터 유지
- **Activity 독립**: UI와 분리된 순수 로직

#### ViewModel의 생명주기

```
App 시작
  ↓
ViewModel 생성 (onCreate)
  ↓
화면 회전 (Configuration Change)
  ↓
Activity 재생성 (onCreate 다시 호출)
  ↓
ViewModel은 새로 생성되지 않음 ✅ 데이터 유지!
  ↓
Activity 종료
  ↓
ViewModel 정리 (onCleared)
```

#### 일반 변수 vs ViewModel

```kotlin
// ❌ 나쁜 예: 일반 변수
class MainActivity : AppCompatActivity() {
    private var count = 0  // 화면 회전 시 0으로 초기화됨!
}

// ✅ 좋은 예: ViewModel
class CounterViewModel : ViewModel() {
    private val _count = MutableStateFlow(0)
    val count: StateFlow<Int> = _count.asStateFlow()
}
```

### StateFlow의 개념

#### StateFlow vs Flow

| 특성 | Flow | StateFlow |
|------|------|-----------|
| **현재 상태 유지** | ❌ | ✅ |
| **최신값 기억** | ❌ | ✅ |
| **구독 시 바로 값** | ❌ | ✅ 최신값 즉시 반환 |

#### StateFlow 사용 패턴

```kotlin
class CounterViewModel : ViewModel() {
    private val _count = MutableStateFlow(0)
    val count: StateFlow<Int> = _count.asStateFlow()
    
    fun increment() {
        _count.value += 1
    }
}
```

### ViewModel 초기화

```kotlin
// ✅ 올바른 방법: by viewModels() 사용
private val viewModel: CounterViewModel by viewModels()

// ❌ 잘못된 방법: 직접 생성
private val viewModel = CounterViewModel()
```

### repeatOnLifecycle의 필요성

```kotlin
// ❌ 메모리 누수 위험
lifecycleScope.launch {
    viewModel.count.collect { count ->
        binding.textViewCount.text = count.toString()
    }
}

// ✅ 안전한 방법
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.count.collect { count ->
            binding.textViewCount.text = count.toString()
        }
    }
}
```

---

## 5️⃣ Week 13: MVVM + Room/Compose

### MVVM 아키텍처의 3계층

```
┌────────────────────┐
│   UI Layer         │  ← Activity/Fragment/Composable
│ (View)             │
└─────────┬──────────┘
          ↓
┌────────────────────┐
│ ViewModel Layer    │  ← StateFlow/LiveData
│ (ViewModel)        │
└─────────┬──────────┘
          ↓
┌────────────────────┐
│ Repository Layer   │  ← Room/API
│ (Data Source)      │
└─────────┬──────────┘
          ↓
┌────────────────────┐
│ Data Layer         │  ← Database/API
│ (Room/Retrofit)    │
└────────────────────┘
```

### Repository 패턴의 목적

#### Repository 없이 (나쁜 구조)
```
ViewModel → Room DAO
문제: 테스트 어려움, API 추가 시 ViewModel 수정 필요
```

#### Repository 포함 (좋은 구조)
```
ViewModel → Repository → Room DAO (또는 Retrofit API)
장점: 테스트 용이, 데이터 소스 변경 가능
```

### StateFlow의 반응성

```
데이터 변경
  ↓
StateFlow 값 업데이트
  ↓
collectAsState() (Compose) 또는 collect() (XML)
  ↓
UI 자동 갱신 (Recomposition)
```

### XML vs Jetpack Compose의 데이터 관찰

#### XML 방식
```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.students.collect { students ->
            adapter.submitList(students)
        }
    }
}
```

#### Compose 방식
```kotlin
val students by viewModel.students.collectAsState()
LazyColumn {
    items(students) { student ->
        StudentItem(student)
    }
}
```

---

## 6️⃣ Week 14: Retrofit + REST API

### REST API 개념

#### REST란?
- **RE**presentational **S**tate **T**ransfer
- HTTP 메서드로 리소스 조작
- 상태 비저장(Stateless) 통신

#### HTTP 메서드

| 메서드 | 목적 | 예시 |
|--------|------|------|
| **GET** | 데이터 조회 | 사용자 목록 가져오기 |
| **POST** | 데이터 생성 | 새 사용자 추가 |
| **PUT** | 데이터 전체 수정 | 사용자 정보 완전 변경 |
| **PATCH** | 데이터 부분 수정 | 사용자 이메일만 변경 |
| **DELETE** | 데이터 삭제 | 사용자 삭제 |

### Retrofit의 역할

#### Retrofit이란?
- **선언형 HTTP 클라이언트**: 인터페이스로 API 정의
- **자동 JSON 변환**: Gson으로 자동 직렬화/역직렬화
- **Coroutine 지원**: suspend 함수 호환

#### Retrofit 인터페이스 작성

```kotlin
interface RandomUserApi {
    @GET("api/")
    suspend fun getRandomUsers(
        @Query("results") count: Int = 5
    ): RandomUserResponse
}
```

### JSON 직렬화/역직렬화

#### JSON 구조와 Kotlin 클래스 매핑

```json
{
  "results": [
    {
      "gender": "female",
      "email": "jane@example.com",
      "name": {
        "first": "Jane",
        "last": "Doe"
      }
    }
  ]
}
```

```kotlin
data class ApiResponse(
    val results: List<User>
)

data class User(
    val gender: String,
    val email: String,
    val name: UserName
)

data class UserName(
    val first: String,
    val last: String
)
```

**중요**: 필드명이 JSON 키와 정확히 일치해야 함

---

## 아키텍처 진화 비교

### Week 09 → 14의 진화 과정

```
Week 09: RecyclerView
  - 메모리만 사용
  - 데이터 손실됨 (앱 종료 시)

Week 10-11: Room Database
  - 로컬 영구 저장
  - 오프라인 지원
  - Flow로 자동 업데이트

Week 12: ViewModel + StateFlow
  - UI 상태 관리
  - Configuration Change 대응
  - 비즈니스 로직 분리

Week 13: MVVM 아키텍처
  - 완전한 계층 분리
  - Repository 패턴
  - 테스트 용이
  - XML과 Compose 모두 지원

Week 14: Network API (Retrofit)
  - 원격 서버 연결
  - 실시간 데이터
  - 웹 서비스 통합
```

---

**시험 화이팅! 💪**