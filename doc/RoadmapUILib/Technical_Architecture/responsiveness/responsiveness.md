

[назад](../Technical_Architecture.md)



# 📱 Адаптивность — все методы и технологии

В этой статье собраны все основные методы и технологии, которые используются для создания адаптивных интерфейсов в современных UI-библиотеках.

---

## 1. 🧱 CSS Media Queries

Media Queries — классический и основной инструмент для адаптации интерфейса под разные размеры экранов.

```css
@media (max-width: 768px) {
  .container {
    padding: 1rem;
  }
}
````

Tailwind эквивалент:

```html
<div class="p-4 md:p-8 lg:p-12">Контент</div>
```

---

## 2. ⚡ Tailwind Breakpoints (Mobile-first)

Tailwind CSS использует мобильный подход, стили сначала применяются для мобильных устройств, а потом для больших экранов.

| Breakpoint | min-width |
| ---------- | --------- |
| `sm`       | 640px     |
| `md`       | 768px     |
| `lg`       | 1024px    |
| `xl`       | 1280px    |
| `2xl`      | 1536px    |

---

## 3. 🧩 Flexbox & Grid Layouts

Использование гибких layout-систем для адаптивного расположения элементов.

* Flexbox:

```html
<div class="flex flex-wrap justify-center items-center gap-4">
  <Card />
  <Card />
</div>
```

* Grid:

```html
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
  <Card />
  <Card />
  <Card />
</div>
```

---

## 4. 🌐 Относительные размеры (Relative units)

### Основные варианты:

| Единица                     | Описание                                       | Пример использования                            |
| --------------------------- | ---------------------------------------------- | ----------------------------------------------- |
| `%`                         | Процент от размера родительского элемента      | `width: 50%;` — половина ширины контейнера      |
| `vw`                        | 1% от ширины окна браузера (viewport width)    | `width: 80vw;` — 80% от ширины экрана           |
| `vh`                        | 1% от высоты окна браузера (viewport height)   | `height: 100vh;` — 100% высоты экрана           |
| `rem`                       | Размер шрифта корневого элемента (обычно 16px) | `font-size: 1.25rem;` — 20px, если базовый 16px |
| `em`                        | Размер шрифта родительского элемента           | `padding: 1em;` — равен текущему размеру шрифта |
| `ch`                        | Ширина символа "0" в текущем шрифте            | Используется для задания ширины поля ввода      |
| `min()`, `max()`, `clamp()` | Функции CSS для гибкой адаптации размеров      | `width: clamp(200px, 50%, 600px);`              |

### Примеры:

```css
.container {
  width: 90%;       /* Занимает 90% от родителя */
  max-width: 1200px; /* Максимальная ширина */
  padding: 1rem;     /* Отступы относительно корневого шрифта */
}

.text {
  font-size: clamp(1rem, 2vw, 2rem); /* Гибкий размер шрифта */
}
```

В Tailwind можно использовать:

```html
<div class="w-full max-w-4xl p-4 text-base sm:text-lg lg:text-xl">
  Текст с адаптивным размером
</div>
```

---

## 5. 🎛 Container Queries (контейнерные запросы)

Позволяют применять стили в зависимости от размера **родительского контейнера**, а не окна браузера.

```css
@container (min-width: 500px) {
  .card {
    flex-direction: row;
  }
}
```

Поддержка постепенно растёт, для Tailwind доступны плагины.

---

## 6. 🖼 Адаптивные изображения

* Используйте атрибуты `srcset` и `sizes` для подгрузки нужного размера изображения.
* Форматы WebP, AVIF уменьшают вес.
* Tailwind классы `object-cover`, `aspect-[ratio]` помогают сохранять пропорции.

```html
<img
  srcset="small.jpg 480w, medium.jpg 800w, large.jpg 1200w"
  sizes="(max-width: 600px) 480px, (max-width: 1024px) 800px, 1200px"
  src="large.jpg"
  alt="Пример изображения"
/>
```

---

## 7. 🧠 Адаптация через JavaScript

Можно динамически менять UI по размерам экрана или другим параметрам.

```tsx
const isMobile = window.innerWidth < 768;
```

Или использовать React Hook:

```tsx
function useMediaQuery(query: string) {
  const [matches, setMatches] = React.useState(false);
  React.useEffect(() => {
    const media = window.matchMedia(query);
    setMatches(media.matches);
    const listener = () => setMatches(media.matches);
    media.addListener(listener);
    return () => media.removeListener(listener);
  }, [query]);
  return matches;
}

// Использование
const isMobile = useMediaQuery('(max-width: 768px)');
```

---

## 8. 📦 Tailwind CSS Plugins для адаптивности

* `@tailwindcss/aspect-ratio` — управление соотношением сторон
* `@tailwindcss/container-queries` — контейнерные запросы
* `tailwindcss-fluid-type` — плавный адаптивный размер шрифтов

---

## 9. 🎚 Комбинированные классы Tailwind для адаптивности

| Класс             | Описание                                 |
| ----------------- | ---------------------------------------- |
| `w-full`          | Ширина 100%                              |
| `max-w-xs`        | Максимальная ширина                      |
| `aspect-square`   | Квадратное соотношение сторон            |
| `truncate`        | Сокращение текста с многоточием          |
| `hidden sm:block` | Скрыть на маленьких, показать на больших |
| `overflow-auto`   | Прокрутка при нехватке места             |

---

## 10. 🎭 Контентная адаптация

* Сокращение/показ текста
* Показывать/скрывать элементы в зависимости от размера
* Упрощение UI на маленьких экранах

```html
<p class="truncate sm:overflow-visible">
  Очень длинный текст, который на мобильных устройствах сокращается...
</p>
```

---

## 11. 🧭 Ориентация экрана

Можно применять стили под `portrait` или `landscape`:

```css
@media (orientation: landscape) {
  .sidebar {
    display: none;
  }
}
```

---

## 12. 🧪 User Agent Detection (не рекомендуется)

Можно определить устройство по user agent, но лучше полагаться на media queries.

---

## ✅ Итоговые рекомендации

* Используйте **mobile-first подход** с Tailwind
* Применяйте **flexbox и grid** для гибких макетов
* Отдавайте предпочтение **относительным единицам** (%, vw, rem, em)
* Поддерживайте **адаптивные изображения** с `srcset`
* Используйте **контейнерные запросы** по мере роста поддержки
* В случае сложных сценариев используйте **JavaScript для адаптации**
* Тестируйте интерфейс на разных устройствах и разрешениях

---

## 📚 Полезные ссылки

* [Tailwind CSS Responsive Design](https://tailwindcss.com/docs/responsive-design)
* [MDN CSS Media Queries](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries)
* [CSS Tricks — Container Queries](https://css-tricks.com/container-queries/)
* [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
* [Responsive Images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images)

---

Если хочешь, могу сделать ещё файл с примерами компонентов твоей библиотеки, реализующих эти подходы, или подготовить аналогичную документацию по доступности (a11y).

