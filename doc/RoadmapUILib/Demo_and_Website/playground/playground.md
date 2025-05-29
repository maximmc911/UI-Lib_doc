


[назад](../Demo_and_Website.md)



# ⚡ Интерактивность и визуализация компонентов

## 📦 1. `react-live`

Быстрая и лёгкая интеграция JSX playground'а прямо в документационный сайт.

### 📘 Пример: Интерактивный компонент

```tsx
import React from 'react';
import { LiveProvider, LiveEditor, LiveError, LivePreview } from 'react-live';
import { Button } from './components/Button';

const code = `<Button label="Нажми меня" onClick={() => alert('Привет!')} />`;

<LiveProvider code={code} scope={{ React, Button }}>
  <LivePreview />
  <LiveEditor />
  <LiveError />
</LiveProvider>
````

### ✅ Возможности:

* Поддерживает JSX, props, события
* Можно передавать дополнительные компоненты через `scope`
* Возможность ограничить редактор (read-only, только preview и т.п.)

---

## 🛠 2. `@codesandbox/sandpack-react`

Мощный playground от CodeSandbox. Возможна симуляция реального проекта.

### 📘 Пример: Песочница с кнопкой

```tsx
import {
  SandpackProvider,
  SandpackLayout,
  SandpackCodeEditor,
  SandpackPreview,
} from '@codesandbox/sandpack-react';

<SandpackProvider
  template="react"
  files={{
    '/App.js': {
      code: `import React from "react";
import { Button } from "./Button";

export default function App() {
  return <Button label="Click me" onClick={() => alert("Hi")} />;
}`,
      active: true,
    },
    '/Button.js': {
      code: `export function Button({ label, onClick }) {
  return <button onClick={onClick}>{label}</button>;
}`,
    },
  }}
>
  <SandpackLayout>
    <SandpackCodeEditor />
    <SandpackPreview />
  </SandpackLayout>
</SandpackProvider>
```

### ✅ Возможности:

* Поддержка файлов и структуры проекта
* Использование шаблонов (`react`, `vue`, `vanilla`)
* Изоляция окружения

---

## 🎨 3. `Playroom`

Инструмент для дизайн-систем и UI-kit документации.

> Требует настройки Webpack + темы + поддерживает JSX и props в live-режиме.

### 📘 Пример: Простой компонент с темами

```tsx
<Button primary label="Привет" />
```

> ⚠️ Компоненты и темы должны быть импортированы глобально.

### ✅ Возможности:

* Темизация
* Сборка как SPA
* Отлично подходит для корпоративных UI-библиотек

---

## 🖍 4. `react-syntax-highlighter`

Подсветка кода с поддержкой стилей и тем.

### 📘 Пример: Подсветка JSX

```tsx
import { Prism as SyntaxHighlighter } from 'react-syntax-highlighter';
import { oneDark } from 'react-syntax-highlighter/dist/esm/styles/prism';

<SyntaxHighlighter language="jsx" style={oneDark}>
  {`<Button label="Click" onClick={() => alert('Clicked')} />`}
</SyntaxHighlighter>
```

---

## 📋 5. `copy-to-clipboard`

Лёгкое копирование текста в буфер обмена.

### 📘 Пример: Кнопка копирования

```tsx
import { CopyToClipboard } from 'react-copy-to-clipboard';

const code = '<Button label="Click" />';

<CopyToClipboard text={code}>
  <button>📋 Копировать</button>
</CopyToClipboard>
```

---

## 🧠 6. Комбинированный Playground-компонент

Можно объединить все вышеописанные библиотеки для создания своего Playground-компонента:

### 📘 Пример: Свой Playground

```tsx
import { LiveProvider, LiveEditor, LiveError, LivePreview } from 'react-live';
import { Prism as SyntaxHighlighter } from 'react-syntax-highlighter';
import { CopyToClipboard } from 'react-copy-to-clipboard';

const Playground = ({ code, scope }) => {
  return (
    <LiveProvider code={code} scope={scope}>
      <div style={{ border: '1px solid #ccc', padding: '1rem', borderRadius: '8px' }}>
        <LivePreview />
        <LiveEditor />
        <LiveError />
        <CopyToClipboard text={code}>
          <button style={{ marginTop: '1rem' }}>Скопировать</button>
        </CopyToClipboard>
      </div>
    </LiveProvider>
  );
};
```

---

## 📦 Альтернативы

| Библиотека                 | Описание               | Когда использовать                                  |
| -------------------------- | ---------------------- | --------------------------------------------------- |
| `react-live`               | Быстрый JSX playground | MVP-документация, React-only                        |
| `sandpack-react`           | Песочница с файлами    | Большие UI-библиотеки, много компонентов            |
| `Playroom`                 | Темизация + playground | Дизайн-системы, крупные проекты                     |
| `Monaco Editor`            | IDE-подобный редактор  | Расширенные редакторы в SaaS, коды больших проектов |
| `react-syntax-highlighter` | Подсветка кода         | Любая документация                                  |
| `copy-to-clipboard`        | Копирование            | Везде, где есть отображение кода                    |

---

## ✅ Заключение

Для начала разработки и MVP:

* Использовать `react-live` + `copy-to-clipboard`
* Добавить `react-syntax-highlighter` для красивой подсветки

В будущем:

* Перейти на `sandpack-react` или `Playroom`, если потребуется сложная песочница или интеграция с токенами и темами
* Добавить поддержку Monaco Editor при необходимости интеграции редактора, похожего на VSCode

