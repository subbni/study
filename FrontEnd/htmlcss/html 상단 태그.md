# html 태그
## DOCTYPE
문서가 어떤 형식과 어떤 html 버전으로 작성되었는지를 명시한다.
```html
<!DOCTYPE html>
```
- html 파일의 가장 첫 줄에 위치하여 웹 문서의 시작을 알려준다.
- 웹 브라우저에게 처리할 문서의 형식과 (`html`), 버전을 알려준다.
- 생략을 한다해도 그 자체로 에러가 발생하진 않는다.
- 그렇지만 <!DOCTYPE> 선언 생략시 기존의 표준모드(`standard mode`)가 아닌 비표준모드(`quirks mode`)로 렌더링이
  수행되기 때문에, 웹 페이지가 올바로 표시되지 않을 수 있다.
  
## head
metadata( = 기계가 읽을 정보 )가 담기는 곳이다.
```html
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Flex</title>
    <link rel="stylesheet" href="../css/flex-layout.css" />
  </head>
```

- `<meta>` : 인코딩, 뷰포트 등 기타 메타 정보
> viewport : 화면에 보여지고 있는 영역
> - 모바일 등 특수한 화면 상황을 위해 정의해준다.
> - 시각적 뷰포트 : 사람에게 보여지는 영역
> - 레이아웃 뷰포트 : 브라우저가 웹페이지를 그리는 영역

|항목|설명|비고|
|---|-----|---|
|width|뷰포트의 너비|device-width:기기의 화면 너비|
|initial-scale|페이지가 처음 로드될 때 줌 레벨|기본 1.0|
|maximum-scale|최대 줌 레벨|.|
|minimum-scale|최소 줌 레벨|.|
|user-scalable|사용자가 줌인,아웃 가능 여부|no로 비활성화 가능|

- `<title>` : 페이지의 제목
- CSS와 JS 등의 코드 및 링크
- Open Graph 정보 : 링크를 공유했을 때 뜨는 정보들 설정
- favicon 설정
- 등등 ...
