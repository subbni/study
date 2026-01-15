# Omit

Omit<T, K>로 T에 속한 특정 속성 K를 제외한 타입을 정의할 수 있다.

#### 사용 예시 (25.01.15)
```typescript
type UiState = {
 ...
  toast: { id: string; title: string; message?: string; type: "success" | "error" }[];
  
  pushToast(t: Omit<UiState["toast"][number], "id">): void;
 ...
}
```
- `Omit<UiState["toast"][number], "id">`
  - `UiState["toast"][number]` : 배열인 toast에서 임의의 숫자 인덱스로 꺼낸 값의 타입. 즉 toast 배열에 들어갈 수 있는 객체 1개의 형태
  - `Omit<UiState["toast"][number], "id">` : 에서 "id" 속성을 제외한 타입
 
# Partial

Partial<T>로 T의 모든 속성을 옵션 속성 `?`으로 바꾼다.

#### 사용 예시 (25.01.15)
```typescript

type UiState = {
 ...
  scriptQuery: {
    query: string;
    status: ScriptStatusFilter;
    page: number;
    size: number;
  };
  
  setScriptQuery(patch: Partial<UiState["scriptQuery"]>): void;
 ...
}
```
