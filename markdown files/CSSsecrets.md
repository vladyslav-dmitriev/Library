# Книга «Секреты CSS. Идеальные решения ежедневных задач»


**input с треугольными краями и кроссбраузерный placeholder**

Разметка в `index.html`:
```html
<div class="my-form">
  <div class="my-label">
    <input type="text" class="my-input" placeholder="xxxx  xxxx  xxxx" required>
  </div>
</div>
```

Стили в `main.css`:
```css
body {
  margin: 0;
  padding: 0;
  background: #222629;
}

input::-ms-clear {
   display: none;
}

input:focus::-webkit-input-placeholder, textarea:focus::-webkit-input-placeholder { 
  color: transparent; 
} 

.my-input {
  height: 100%;
  width: 100%;
  outline: none;
  padding: 0 50px;
  font-size: 20px;
  color: #fefefe;
  background: transparent;
  background: linear-gradient(135deg, transparent 24px, #4A5760 24px, #4A5760 25px, transparent 0) top left,
              linear-gradient(-135deg, transparent 24px, #4A5760 24px, #4A5760 25px, transparent 0) top right,
              linear-gradient(-45deg, transparent 24px, #4A5760 24px, #4A5760 25px, transparent 0) bottom right,
              linear-gradient(45deg, transparent 24px, #4A5760 24px, #4A5760 25px, transparent 0) bottom left;
  background-size: 50% 50%;
  background-repeat: no-repeat;
  box-sizing: border-box;
  border: none;
}

.my-label {
  width: 400px;
  height: 66px;
  margin: 20px auto 0;
  position: relative;
}

.my-label::after, .my-label::before {
  content: '';
  position: absolute;
  width: 83%;
  height: 1px;
  background: #4A5760;
  top: 0;
  left: 8.5%;
}

.my-label::before {
  top: auto;
  left: auto;
  bottom: 0;
  right: 8.5%;
}

.my-input:-ms-input-placeholder {
  color: #fff;
  text-align: center;
  font-style: italic;
  font-size: 18px;
}

.my-input::-ms-input-placeholder {
  color: #fff;
  text-align: center;
  font-style: italic;
  font-size: 18px;
}

.my-input::-webkit-input-placeholder {
  color: #fff;
  text-align: center;
  font-style: italic;
  font-size: 18px;
}

.my-input::placeholder {
  color: #fff;
  text-align: center;
  font-style: italic;
  font-size: 18px;
}
```