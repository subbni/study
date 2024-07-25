# BEM (Block, Element, Modifier) Naming


# Block
기능적으로 독립적인 요소로, 페이지를 구성하는 가장 큰 단위이다.

Block 요소는 `class`로서 구별한다.
```html
<div class="error"></div>
```
 - block 요소는 다른 배경에 영향을 미쳐서는 안 된다. (따라서 margin을 주거나 특별한 position을 설정해서는 안 됨)

# Element
하나의 Block을 구성하는 각각의 요소이다.
- element 요소의 네이밍은 `__`으로 구별된다. ex) `block-이름__element-이름`으로 구성
```html
<!-- 'search-form' block -->
<form class="search-form">
  <!-- 'input' element in the 'search-form' block -->
  <input class="search-form__input">
  <!-- 'button' element in the 'search-form' block -->
  <button class="search-form__btn">Search</button>
</form>
```

- element는 언제나 block의 요소여야 한다. element가 element의 요소가 될 수는 없다.
```html
<form class="search-form">
    <div class="search-form__content">
        <!-- Recommended: `search-form__input` or `search-form__content-input` -->
        <input class="search-form__content__input">

        <!-- Recommended: `search-form__button` or `search-form__content-button` -->
        <button class="search-form__content__button">Search</button>
    </div>
</form>
```

## Block / Element 중 선택하기

### Block으로 만들기
- 요소가 다른 곳에서 재사용 될 수 있을 때
- 다른 요소들에 의해 영향을 받지 않는 독립적인 요소일 때
- 해당 요소 밑으로 요소들이 많이 나뉘어지는 경우

### Element로 만들기
- 해당 요소가 parent entity(or block)없이는 사용될 수 없는 경우

> 예외) 위 조건을 만족한다해도 해당 요소가 다른 작은 부분들로 많이 나뉘어지게 될 경우,
> BEM에서는 element 밑에 다른 요소를 둘 수 없으므로 service block을 만드는 것이 좋다.


# Modifier
block이나 element의 appearance, state, or behavior를 드러내는 네이밍 요소이다.
- modifier 네이밍은 `_`으로 구별된다. ex) `block-이름__element-이름_modifier-이름`

## Boolean
다른 것과 구분되는 특별한 속성이 존재함을 나타내고 싶을 경우, `modifier-name`으로 설정해준다.

ex) `disabled`, `focused`

```html
<!-- The `search-form` block has the `focused` Boolean modifier -->
<form class="search-form search-form_focused">
    <input class="search-form__input">

    <!-- The `button` element has the `disabled` Boolean modifier -->
    <button class="search-form__button search-form__button_disabled">Search</button>
</form>
```

## Key-Value
어떤 속성에 대한 종류를 구별하여 나타내고 싶을 경우, `속성-이름_종류`의 형식으로 설정해준다.

ex) 테마의 종류, 사이즈의 종류
```html
<!-- The `search-form` block has the `theme` modifier with the value `islands` -->
<form class="search-form search-form_theme_islands">
    <input class="search-form__input">

    <!-- The `button` element has the `size` modifier with the value `m` -->
    <button class="search-form__button search-form__button_size_m">Search</button>
</form>
```

# Mix하여 사용하기
기본 DOM node와 BEM entities를 함께 사용한다.
```html
<!-- `header` block -->
<div class="header">
    <!--
        The `search-form` block is mixed with the `search-form` element
        from the `header` block
    -->
    <div class="search-form header__search-form"></div>
</div>
```

`header__search-form`에 주위 배경과 interactive한 속성들을 넣고, 
`search-form`에는 universal한 속성들을 유지한다.

이런 식으로 `search-form`을 어떤 다른 배경에서도 사용할 수 있도록, Independen하게 유지할 수 있다.
