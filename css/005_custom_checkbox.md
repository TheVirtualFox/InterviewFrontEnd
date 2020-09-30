```html
<div class="checkbox">
  <input type="checkbox" id="ch" class="checkbox__input">
  <label for="ch" class="checkbox__label"></label>
</div>
```

```css
.checkbox .checkbox__input:checked ~ .checkbox__label:after {
  position: absolute;
  content: '';
  height: 10px;
  width: 10px;
  background: #aaa;
  border-radius: 50%;
  top: 50%;
  left: 50%;
  transform: translateX(-50%) translateY(-50%);
}
.checkbox .checkbox__label {
  border: 1px solid #aaa;
  border-radius: 50%;
  height: 20px;
  width: 20px;
  display: block;
  position: relative;
}
.checkbox .checkbox__input {
  display: none;
}
```