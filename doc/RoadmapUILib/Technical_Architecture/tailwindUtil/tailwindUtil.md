


[назад](../Technical_Architecture.md)



# 📦 Стилизация в UI-библиотеке

Эта UI-библиотека использует современные подходы к стилизации с фокусом на гибкость, масштабируемость и поддержку тем. Основные технологии:

- Tailwind CSS для утилитарной стилизации
- Темы (themes) через CSS-переменные
- Токены дизайна для стандартизации стилей
- Поддержка кастомизации через классы и переменные

---

## 🎨 Стилизация с помощью Tailwind CSS

Библиотека активно использует [Tailwind CSS](https://tailwindcss.com) как главный способ стилизации компонентов. Это позволяет использовать декларативный, наглядный и кастомизируемый подход.

Пример:

```tsx
<button className="px-4 py-2 bg-primary text-white rounded-lg hover:bg-primary/80">
  Нажми меня
</button>
````

Tailwind-классы легко переопределяются:

```tsx
<Button className="bg-red-500 hover:bg-red-600" />
```

---

## 🧩 Темизация (Themes)

Темы реализованы через **CSS-переменные** и **атрибуты** (`data-theme`). Это позволяет задавать разные визуальные стили (например, светлая/тёмная тема) без изменения компонентов.

### Подключение тем

```html
<html data-theme="light">
```

или

```html
<html data-theme="dark">
```

### Пример переменных темы:

```css
:root[data-theme='light'] {
  --color-bg: #ffffff;
  --color-text: #000000;
  --color-primary: #1d4ed8;
}

:root[data-theme='dark'] {
  --color-bg: #0f172a;
  --color-text: #ffffff;
  --color-primary: #3b82f6;
}
```

Компоненты используют эти переменные:

```tsx
<div className="bg-[var(--color-bg)] text-[var(--color-text)]">
  Контент
</div>
```

---

## 🧱 Design Tokens (Токены дизайна)

Токены — это стандартизированные переменные для повторного использования цветов, отступов, размеров, шрифтов и т.д.

```css
:root {
  --token-radius-sm: 4px;
  --token-radius-md: 8px;
  --token-radius-lg: 16px;

  --token-font-body: 'Inter', sans-serif;
  --token-spacing-xs: 4px;
  --token-spacing-sm: 8px;
  --token-spacing-md: 16px;
  --token-spacing-lg: 24px;

  --token-color-primary: #3b82f6;
  --token-color-secondary: #64748b;
}
```

Использование токенов в компонентах:

```tsx
<div
  style={{
    padding: 'var(--token-spacing-md)',
    borderRadius: 'var(--token-radius-md)',
    backgroundColor: 'var(--token-color-primary)',
  }}
>
  Токенизированный компонент
</div>
```

---

## 💡 CSS-переменные

CSS-переменные используются везде, где возможна темизация или кастомизация. Это позволяет:

* Изменять темы без пересборки
* Переопределять стили на уровне пользователя
* Поддерживать accessibility

### Пример кастомизации темы пользователем

```html
<html style="--color-primary: #10b981;">
```

И компонент автоматически адаптируется:

```tsx
<button className="bg-[var(--color-primary)]">
  Кнопка с кастомной темой
</button>
```

---

## 📁 Пример структуры токенов

```txt
/tokens
├── colors.css
├── spacing.css
├── typography.css
└── radius.css
```

Каждый файл содержит соответствующие `:root` переменные.

---

## 🛠 Резюме

| Технология    | Применение                         |
| ------------- | ---------------------------------- |
| Tailwind      | Быстрая утилитарная стилизация     |
| CSS Variables | Темизация, гибкость                |
| Design Tokens | Единообразие, масштабируемость     |
| Themes        | Поддержка нескольких цветовых схем |

---

## 📚 Примеры компонентов

```tsx
<Card className="bg-[var(--color-bg)] text-[var(--color-text)] p-[var(--token-spacing-md)] rounded-[var(--token-radius-lg)]">
  Пример карточки с токенами
</Card>
```

---

## 🔗 Полезные ссылки

* [Tailwind CSS Documentation](https://tailwindcss.com/docs)
* [Design Tokens W3C](https://design-tokens.github.io/community-group/format/)
* [CSS Variables](https://developer.mozilla.org/en-US/docs/Web/CSS/--*)

---

