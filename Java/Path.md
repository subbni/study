# Path
- "경로 그 자체"를 표현하는 객체
- 실제 파일을 읽고 쓰는 것은 Files 클래스가 담당한다.
- Path = 경로 표현
- Files = IO 작업 (읽기/쓰기/삭제/복사/이동/디렉터리 생성 등)

## 경로를 String 대신 Path로 나타내는 이유
### OS 경로 구분자 처리
- Windows는 `\`, macOS/Linux는 `/`
- Path가 내부적으로 OS 규칙에 맞춰 처리

### 안전한 경로 결합
```java
Path p = base.resolve(id).resolve(file);
```
- 중복 구분자/플랫폼 차이를 신경 덜 써도 된다.

### 정규화/검증에 유리
- normalize() : 정규화
- toAbsolutePath() : 절대 경로 반환 

## 자주 쓰는 Path/Files 패턴
### Path 만들기
```java
Path root = Paths.get("/data/artifacts"); // 문자열 -> Path
```
- /로 시작 => OS 루트 기준
- /로 시작하지 않음 => 현재 작업 디렉터리 기준

### 경로 결합 : resolve
```java
Path dir = root.resolve("images").resolve("123");          // /data/artifacts/images/123
Path target = dir.resolve("a.png");      // /data/artifacts/images/123/a.png
```
- `주의 !` resolve로 붙이는 값이 절대경로라면 앞을 무시한다.
```java
Paths.get("/a/b").resolve("/x/y"); // /x/y
```
- 이 때문에 업로드에서 사용자 입력을 그대로 resolve에 넣으면 위험할 수 있다.

### 절대 경로 + 정규화
```java
Path safe = file.toAbsolutePath().normalize();
```

### 디렉터리 생성
```java
Files.createDirectories(dir); // 이미 있으면 그냥 넘어감(예외 없음)
```

### 파일 삭제 
```java
Files.deleteIfExists(file);
```
