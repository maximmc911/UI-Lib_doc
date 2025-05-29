

[назад](../documentation.md)


Вот подробная **документация в формате Markdown** на тему:

# 🧪 Интерактивные примеры с использованием Sandpack

---

## 📌 Что такое Sandpack?

**Sandpack** — это мощный инструмент от команды [CodeSandbox](https://codesandbox.io/), позволяющий встраивать **интерактивные окружения разработки прямо на сайт или в документацию**. Он рендерит работающий проект React, Vue или Vanilla JS прямо в браузере, без необходимости серверной сборки.

---

## 🎯 Возможности

* ✅ Live preview компонентов в реальном времени
* ✅ Редактирование кода на лету (подсветка, автосохранение)
* ✅ Поддержка TypeScript, React, Vue, Vanilla JS
* ✅ Темизация интерфейса
* ✅ Поддержка файловой структуры

---

## 🧰 Установка

Установите необходимые пакеты в React-проект:

```bash
npm install @codesandbox/sandpack-react
```

или

```bash
yarn add @codesandbox/sandpack-react
```

---

## 🛠️ Базовое использование

```tsx
import { Sandpack } from "@codesandbox/sandpack-react";

export default function Example() {
  return (
    <Sandpack
      template="react"
      files={{
        "/App.js": {
          code: `import React from "react";

export default function App() {
  return <button>Hello Sandpack!</button>;
}`,
        },
      }}
    />
  );
}
```

---

## 🧩 Пример с TypeScript и несколькими файлами

```tsx
import { Sandpack } from "@codesandbox/sandpack-react";

export default function ExampleTS() {
  return (
    <Sandpack
      template="react-ts"
      files={{
        "/App.tsx": {
          code: `import React from "react";
import { Button } from "./Button";

export default function App() {
  return <Button label="Нажми меня" />;
}`,
        },
        "/Button.tsx": {
          code: `import React from "react";

export function Button({ label }: { label: string }) {
  return <button>{label}</button>;
}`,
        },
      }}
    />
  );
}
```

---

## 🎨 Настройка внешнего вида

```tsx
import {
  SandpackLayout,
  SandpackCodeEditor,
  SandpackPreview,
  SandpackProvider,
} from "@codesandbox/sandpack-react";

export default function ThemedSandpack() {
  return (
    <SandpackProvider template="react">
      <SandpackLayout>
        <SandpackCodeEditor showTabs style={{ height: 300 }} />
        <SandpackPreview />
      </SandpackLayout>
    </SandpackProvider>
  );
}
```

---

## 🧠 Полезные параметры

| Параметр      | Описание                                                       |
| ------------- | -------------------------------------------------------------- |
| `template`    | Предустановленный шаблон (например, `react`, `vue`, `vanilla`) |
| `theme`       | Тема оформления (`dark`, `light`, или кастомная)               |
| `files`       | Объект с файлами и содержимым                                  |
| `customSetup` | Кастомизация зависимостей, языка и т.д.                        |

---

## 🎛 Пример с кастомными зависимостями

```tsx
<Sandpack
  template="react"
  customSetup={{
    dependencies: {
      "lodash": "4.17.21"
    }
  }}
  files={{
    "/App.js": {
      code: `import _ from "lodash";

export default function App() {
  return <div>{_.capitalize("hello world")}</div>;
}`
    }
  }}
/>
```

---

## 🧩 Альтернативы Sandpack

| Инструмент            | Особенности                         | Поддержка SSR | Стиль       |
| --------------------- | ----------------------------------- | ------------- | ----------- |
| **Sandpack**          | Лучший для интеграции с React       | ❌             | React-first |
| **Liveblocks**        | Встроенный редактор + синхронизация | ❌             | React-first |
| **MDX Live**          | Простая интерактивность внутри MDX  | ❌             | Next/Gatsby |
| **Playroom**          | Интерфейс Playground с live preview | ✅ (частично)  | Standalone  |
| **CodeSandbox Embed** | Вставка iFrame с песочницей         | ✅             | Независимый |

---

## ✅ Когда использовать Sandpack?

* 🟢 Для создания документации компонентов
* 🟢 Для showcase-страниц
* 🟢 Для уроков/курсов
* 🟡 Не подходит для SSR (например, Next.js с SSR) без костылей

---

## 🧩 Советы по использованию

* Не загружайте слишком большие проекты
* Используйте только нужные зависимости
* Поддерживайте чистоту примеров

---

## 📚 Дополнительные ресурсы

* [Официальная документация Sandpack](https://sandpack.codesandbox.io/)
* [Playroom](https://seek-oss.github.io/playroom/)
* [MDX Live](https://mdxjs.com/guides/live-code/)

