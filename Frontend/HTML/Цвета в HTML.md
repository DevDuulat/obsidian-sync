### Цвета в HTML

В HTML цвета могут быть заданы с помощью предопределенных **имен цветов**, а также значений **RGB**, **HEX**, **HSL**, **RGBA**, или **HSLA**.

### Имена цветов
HTML поддерживает 140 стандартных имен цветов, таких как:

- **Tomato**
- **Orange**
- **DodgerBlue**
- **MediumSeaGreen**
- **Gray**
- **SlateBlue**
- **Violet**
- **LightGray**

### Пример использования:

```html
<h1 style="background-color:DodgerBlue;">Hello World</h1>
<p style="background-color:Tomato;">Lorem ipsum...</p>
```

### Цвет текста
Цвет текста можно задать с помощью CSS-свойства `color`.

Пример:

```html
<h1 style="color:Tomato;">Hello World</h1>
<p style="color:DodgerBlue;">Lorem ipsum...</p>
<p style="color:MediumSeaGreen;">Ut wisi enim...</p>
```

### Цвет границы
Цвет границы задается с помощью свойства `border`.

Пример:

```html
<h1 style="border:2px solid Tomato;">Hello World</h1>
<h1 style="border:2px solid DodgerBlue;">Hello World</h1>
<h1 style="border:2px solid Violet;">Hello World</h1>
```

### Цветовые значения

Цвета могут быть заданы следующими значениями:

1. **RGB** (Красный, Зеленый, Синий)
2. **HEX** (Шестнадцатеричный формат)
3. **HSL** (Оттенок, Насыщенность, Светлота)
4. **RGBA** (RGB с альфа-каналом для прозрачности)
5. **HSLA** (HSL с альфа-каналом)

Примеры:

```html
<h1 style="background-color:rgb(255, 99, 71);">...</h1>
<h1 style="background-color:#ff6347;">...</h1>
<h1 style="background-color:hsl(9, 100%, 64%);">...</h1>
<h1 style="background-color:rgba(255, 99, 71, 0.5);">...</h1>
<h1 style="background-color:hsla(9, 100%, 64%, 0.5);">...</h1>
```

### Упражнение:
**Вопрос:** Какое из следующих имен является допустимым именем цвета в HTML?

- Apple
- Banana
- **Tomato**

Правильный ответ: **Tomato**

### Вывод
HTML поддерживает различные способы задания цветов: с помощью имен, значений RGB, HEX, HSL, а также их версий с прозрачностью.