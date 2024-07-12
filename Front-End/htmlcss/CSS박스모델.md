# 블록 및 인라인 박스

> CSS 박스에는 크게 두 가지 유형이 있다. **블록 박스**와 **인라인 박스**이다.
> 

이 박스 유형들은 display 속성 값으로 정의되며, 아울러 **display의 outer 값**과 관련이 있다.

### 블록 박스의 동작 방식

- 새 줄로 행갈이를 한다.
- width 와 height 속성이 적용된다.
- 패딩과 여백, 테두리로 인해 다른 요소들이 박스로부터 밀려난다.
- 박스는 인라인 방향으로 연장되어 상위 콘테이너에서 사용 가능한 공간을 채운다.
- `h1` , `h2` , `p` ..

### 인라인 박스의 동작 방식

- 새 줄로 행갈이를 하지 않는다.
- width 와 height 속성은 적용되지 않는다.
- 패딩과 여백, 테두리는 다른 인라인 박스들이 박스로부터 멀어지게 하지 않는다.
- `a` , `span` , `strong`, …

# (+) 내부 및 외부 디스플레이 유형

위에서 언급했듯이, *CSS의 박스(블록/인라인)은 외부 디스플레이 유형*을 가진다.

> 반면 박스에는 *내부 디스플레이 유형*도 있으며, 이는 ***해당 박스 내부의 요소가 배치되는 방법***을 나타낸다.
> 

`flex` 와 같은 display 값을 사용하여 내부 디스플레이 유형을 변경할 수 있다.

<aside> 만일 `display: flex` 를 설정하면 외부 디스플레이 유형은 블록이지만 내부 디스플레이 유형은 flex로 변경된다. 이 박스의 직계 자식들은 플렉스 항목이 되고, 규칙에 따라 배치된다.

</aside>

# Box의 구성

<img width="537" alt="Screenshot 2024-07-12 at 14 59 40" src="https://github.com/user-attachments/assets/7fe4f73e-266a-4856-8f0e-ee64b833bb1c">

- Content 박스 : 콘텐츠가 표시되는 영역으로, width와 height로 크기를 정한다.
- Padding 박스 : 콘텐츠 주변을 공백처럼 자리잡는다.
- Border 박스 : 콘텐츠와 패딩까지 둘러싼 테두리
- Margin 박스 : 가장 바깥 쪽 레이어로, 박스와 다른 요소 사이 공백 역할을 한다.

# 대체 CSS 박스 모델

## 표준 박스 모델

```css
.box {
  width: 350px;
  height: 150px;
  margin: 25px;
  padding: 25px;
  border: 5px solid black;
}
```

위 css를 적용하여 만들어진 박스가 실제로 차지하는 공간은 

너비 410px (350 + 25*2 + 5*2)

높이 210px (150 + 25*2 + 5*2) 이다.


이 경우, 박스가 실제로 차지하는 크기를 알아내기 위해서는 명시된 width와 height에 padding값과 border값을 더해주어야 한다.

<aside>
⚠️ margin은 박스의 실제 크기에 포함되지 않는다.
물론 마진은 박스가 페이지에서 차지하는 총 공간에 영향을 미치지만, 박스의 외부 공간에만 영향을 미치므로 박스의 영역은 테두리에서 멈추게 된다.

</aside>

## 대체 박스 모델

박스의 실제 크기를 얻기 위해 테두리와 패딩을 추가 계산하는 것이 불편한 사람들을 위해 대체 박스 모델이 도입되었다.

대체 CSS 박스 모델을 사용할 시, **width와 height는** ***박스가 실제로 차지하는 크기*를 의미**한다.

이 경우 콘텐츠 영역의 크기를 알기 위해서는 width와 height에서 padding과 border를 빼주어야 한다.
<img width="513" alt="Screenshot 2024-07-12 at 15 00 17" src="https://github.com/user-attachments/assets/bc669432-3789-41a6-9a5b-8c289c2139ed">


### 적용법

```css
.box {
  box-sizing: border-box;
}
```

위와 같이 `box-sizing: border-box` 를 설정하여 대체 박스 모델을 적용할 수 있다.

# 인라인-블록 박스

inline과 block 사이 중립 지대를 제공하는 display 속성의 하나로 특별한 값이 있다.

### 인라인 블록 박스의 동작 방식

- `width` 와 `height` 의 속성이 존중된다.
- 새 줄로 행갈이를 하지 않는다.
- `padding`과 `margin`, `border` 속성으로 인해 다른 요소가 박스에서 밀려난다.

### 예시

단순 인라인 박스의 경우,

```css
span {
  margin: 20px;
  padding: 20px;
  width: 50px;
  height: 50px;
  background-color: lightblue;
  border: 2px solid blue;
}
```

```html
<p>
  I am a paragraph and this is a <span>span</span> inside that paragraph. 
  A span is an inline element and so does not respect width and height.
</p>
```
<img width="429" alt="Screenshot 2024-07-12 at 15 00 53" src="https://github.com/user-attachments/assets/5517d158-8d29-426e-b639-c1f97e8d55ad">

width와 height는 무시된다. (정사각형으로 설정하였으나 실제 그렇지 않음)

 padding과 margin, border는 적용되지만 이에 의해 다른 요소들이 영향을 받지 않으므로 다른 단어들과 겹치게 된다.

반면 인라인-블록 박스의 경우,

```css
span {
  margin: 20px;
  padding: 20px;
  width: 50px;
  height: 50px;
  background-color: lightblue;
  border: 2px solid blue;
  display: **inline-block**;
}
```

```html
<p>
  I am a paragraph and this is a <span>span</span> inside that paragraph. 
  A span is an inline element and so does not respect width and height.
</p>
```

<img width="426" alt="Screenshot 2024-07-12 at 15 02 23" src="https://github.com/user-attachments/assets/1ecf040f-79a0-491b-ab6d-a89e0440c5a3">

width와 height가 정사각형으로 적용되었으며,

padding과 margin, border가 적용되고 이에 의해 다른 요소들이 박스에서 밀려난 것을 확인할 수 있다.
