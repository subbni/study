# 상속
CSS에는 상속되는 속성들이 있다.

color, font 관련, text-align, text-decoration, line-height, list-style 등등
> 상속되는 속성 전체 리스트 : 
> https://developer.mozilla.org/en-US/docs/Web/CSS/Reference#index

속성값을 따로 지정하지 않은 자식은 부모의 속성값을 물려받으며, 그 값은 아래 후손들에게도 전달된다.

자식 대에서 다른 속성값을 지정한다면, 부모의 것을 덮어쓰게 된다. 그리고 그 덮어쓰인 값이 후손들에게 전달된다.

## ⭐️ inherit
스스로의 값을 포기하고 부모로부터 받은 상속값을 적용한다.

### css
```css
.parent {
  font-weight : bold;
  color: slateblue;
}

.parent > div { color: tomato; }
.parent > :last-child { color: inherit; }
```

### 결과
<img width="1245" alt="Screenshot 2024-07-25 at 12 04 57" src="https://github.com/user-attachments/assets/7efb8027-1b36-4186-bf91-f8b0772e97d5">

## initial
브라우저가 부여한 값을 포기하고 각 속성의 초기값을 적용한다.
> 브라우저가 기본적으로 부여하는 값이 존재한다. (ex. body의 margin, p의 block 속성)


## unset
상속되는 값이 있는 경우 inherit, 없는 경우엔 initial처럼 작동한다.

## revert
상속되는 값이 있는 경우 부모로부터 받은 상속값을 적용한다.

## all 속성
대부분의 속성을 inherit, initial, unset, revert 값으로 지정할 수 있다.
```css
button {
  all : unset;
}
```

브라우저에서 지정한 값을 비우고 원하는 스타일로 초기화하는데 유용하다.
