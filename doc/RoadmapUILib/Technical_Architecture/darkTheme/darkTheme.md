


[назад](../Technical_Architecture.md)



# 🌙 Поддержка тёмной темы

Наша UI-библиотека включает встроенную поддержку светлой и тёмной темы. Темизация реализована с использованием CSS-переменных и позволяет динамически переключать стили без перезагрузки страницы.

---

## 🧩 Основы реализации

Темы задаются с помощью `data-theme` или `class` на корневом HTML-элементе. Компоненты используют CSS-переменные, определённые для каждой темы.

```html
<html data-theme="light"> <!-- или "dark" -->
````

Или:

```html
<html class="theme-light"> <!-- или "theme-dark" -->
```

---

## 🎨 CSS-переменные тем

```css
:root[data-theme='light'] {
  --color-bg: #ffffff;
  --color-text: #000000;
  --color-primary: #2563eb;
}

:root[data-theme='dark'] {
  --color-bg: #0f172a;
  --color-text: #f1f5f9;
  --color-primary: #3b82f6;
}
```

Использование:

```tsx
<div className="bg-[var(--color-bg)] text-[var(--color-text)]">
  Контент
</div>
```

---

## 🔄 Способы смены темы

### 1. ✅ Через `data-theme` вручную

```tsx
function switchTheme(theme: 'light' | 'dark') {
  document.documentElement.setAttribute('data-theme', theme);
}
```

### 2. ✅ Через класс (`classList.toggle`)

Если используется Tailwind с кастомными классами (`.theme-light`, `.theme-dark`):

```tsx
function toggleTheme() {
  const root = document.documentElement;
  root.classList.toggle('theme-dark');
  root.classList.toggle('theme-light');
}
```

Tailwind-конфигурация:

```js
module.exports = {
  darkMode: 'class', // или 'media' или 'attribute'
};
```

---

## 🌐 3. Системная тема (`prefers-color-scheme`)

Вы можете автоматически применять тему в зависимости от настроек ОС пользователя:

```js
const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
document.documentElement.setAttribute('data-theme', prefersDark ? 'dark' : 'light');
```

### Отслеживание изменений в реальном времени:

```js
window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', (e) => {
  const theme = e.matches ? 'dark' : 'light';
  document.documentElement.setAttribute('data-theme', theme);
});
```

---

## 💾 4. Хранение темы в `localStorage`

```tsx
const savedTheme = localStorage.getItem('theme');
if (savedTheme) {
  document.documentElement.setAttribute('data-theme', savedTheme);
} else {
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  document.documentElement.setAttribute('data-theme', prefersDark ? 'dark' : 'light');
}

function setTheme(theme: 'light' | 'dark') {
  document.documentElement.setAttribute('data-theme', theme);
  localStorage.setItem('theme', theme);
}
```

---

## 🧪 5. Переключатель темы (Toggle-компонент)

```tsx
import { useEffect, useState } from 'react';

export function ThemeToggle() {
  const [theme, setTheme] = useState(() => {
    return localStorage.getItem('theme') || 'light';
  });

  useEffect(() => {
    document.documentElement.setAttribute('data-theme', theme);
    localStorage.setItem('theme', theme);
  }, [theme]);

  const toggle = () => {
    setTheme(prev => (prev === 'light' ? 'dark' : 'light'));
  };

  return (
    <button onClick={toggle}>
      Переключить на {theme === 'light' ? 'тёмную' : 'светлую'} тему
    </button>
  );
}
```

---

## ⚙️ Конфигурация Tailwind (для `darkMode`)

```js
// tailwind.config.js
module.exports = {
  darkMode: ['class', '[data-theme="dark"]'], // гибкая поддержка
  theme: {
    extend: {
      colors: {
        background: 'var(--color-bg)',
        text: 'var(--color-text)',
        primary: 'var(--color-primary)',
      },
    },
  },
};
```

---

## 🔍 Примеры компонентов с тёмной темой

```tsx
<Card className="bg-[var(--color-bg)] text-[var(--color-text)] shadow-md p-4 rounded-xl">
  Этот компонент адаптируется под текущую тему
</Card>
```

---

## ✅ Резюме: Варианты переключения темы

| Метод                  | Описание                                        |
| ---------------------- | ----------------------------------------------- |
| `data-theme`           | Универсальный способ, работает с CSS и Tailwind |
| `class`                | Совместим с Tailwind `darkMode: 'class'`        |
| `prefers-color-scheme` | Автоматически использует системную тему         |
| `localStorage`         | Позволяет сохранять выбор пользователя          |
| Toggle-компонент       | Удобный UI-элемент для переключения             |

---

## 📚 Дополнительно

* [Tailwind Dark Mode](https://tailwindcss.com/docs/dark-mode)
* [CSS prefers-color-scheme](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme)
* [CSS Variables](https://developer.mozilla.org/en-US/docs/Web/CSS/--*)

