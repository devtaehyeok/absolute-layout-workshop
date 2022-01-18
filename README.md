# Absolute Layout Workshop

Solution For https://github.com/css-for-js/character-creator

# Q1 answer (Fix footer links)

src/components/Footer/Footer.module.css

> each Browser have unique color and style for a link, so we should override it.

```css
.footer a {
  color: inherit;
}
```

# Q2 answer (Layout adjustments)

- src/components/CharacterEditor/CharacterEditor.module.css

1. The header takes up 60% of the width from parent;

```css
.header {
  padding-bottom: 64px;
  width: 65%;
}
```

2. control column takes up 60% of the width from parent;

```css
.controlColumn {
  width: 50%;
}
```

3. The character is `fixed` on right side of the viewport and it's size is propotional to height of viewport(by top:5, bottom 5% properties).
   also it has minimum size (500px)

```css
.characterWrapper {
  position: fixed;
  top: 15%;
  left: 60%;
  bottom: 5%;
  min-height: 500px;
}
```

# Q3 answer (Overflow)

- src/components/ButtonRow/ButtonRow.module.css

1. First, we should make line unbreakable.
   > It is nature of block element that make line break.

```css
.buttonRow {
  overflow: auto;
  white-space: nowrap;
  /* margin-left: -22px;
  margin-right: -22px;
  margin-bottom: -22px;
  padding: 22px; */
}
```

but it seems too tight and scrollbar is too short.
![too_tight](/img/too_tight_button_row.png)

2. Second, we add margin to make some space

- The outer margin is 24px, so we can leave 2px of space like the bumper!

![too_tight](/img/buffered_image.png)

```css
.buttonRow {
  overflow: auto;
  white-space: nowrap;
  margin-left: -22px;
  margin-right: -22px;
  margin-bottom: -22px;
}
```

3. Add some paddings to make more natural view;

```css
.buttonRow {
  overflow: auto;
  white-space: nowrap;
  margin-left: -22px;
  margin-right: -22px;
  margin-bottom: -22px;
  padding: 22px;
}
```

# Q4 answer (Perspective)

> 참고 : 브라우저의 렌더링 순서  
> 먼저 위치가 지정되지 않은 모든 요소가 렌더링됩니다(Flow, Flexbox, Grid...).  
> 다음으로, 위치 지정 요소가 맨 위에 렌더링됩니다 (relative, absolute, fixed, sticky).  
> 브라우저는 각 단계마다 뒤에 나오는 요소를 위에 올립니다.  
> 아래 사항을 고려하면 z-index를 없앨 수 있다.

wrapper에는 position:relative를 적용한다.(즉 Relative Layout 모드를 따른다)  
relative layout 요소끼리 나오는 순서대로 쌓인다.  
.decoration 클래스 적용 요소는 .wrapper 기준으로 위치한다.

> 여기까지만 내 블로그 복붙이라 한글로 적었다.

- src/components/CharacterEditor/CharacterEditor.module.css

add some css for perspective effect area

```css
.perspectiveEffect {
  position: fixed;
  top: 60%;
  left: 0;
  height: 40%;
  width: 100%;
  background-color: hsl(195deg, 20%, 86%);
}
```

and we want it to be under the control columns and avatar **without z-index**  
so we place the new `div` element and make MaxWidthWrapper follow dynamic layout.  
then `MaxWidthWrapper` will be rendered on the `perspectiveEffect` div following natural drawing order.

```css
.maxWidthWrapper {
  position: relative;
}
```

- src/components/CharacterEditor/CharacterEditor.js

```jsx
    {"..."}
    <main className={styles.characterEditor}>
      <div className={styles.perspectiveEffect}></div>
      <MaxWidthWrapper className={styles.maxWidthWrapper}>
      {"..."}
```
