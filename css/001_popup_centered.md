## Create fixed popup

```html
<div class="wrap">
  <div class="popup"></div>
</div>
```

```css
body {
  overflow: hidden;
}

.wrap {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow-y: auto;
  background-color: rgba(230, 230, 230, .1);
}

.popup {
  position: absolute;
  width: 400px;
  height: 300px;
  right: 0;
  left: 0;
  top: 0;
  bottom: 0;
  margin: auto;
}
```