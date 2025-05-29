

[назад](../Build_and_Deployment.md)


# 📦 Генерация `.d.ts` файлов для TypeScript

## 📘 Что такое `.d.ts`?

Файлы с расширением `.d.ts` — это **TypeScript декларации типов**. Они описывают структуру кода: какие типы, интерфейсы, функции, классы и модули доступны в пакете. Эти файлы позволяют другим проектам (или пакетам) корректно использовать ваш код с поддержкой автодополнения и типизации — даже если они не написаны на TypeScript.

---

## 🧩 Когда нужна генерация `.d.ts`?

- Вы публикуете **UI-библиотеку** или любой **npm-пакет**.
- Вы хотите, чтобы другие могли использовать **автодополнение** и **типовую безопасность**.
- Ваш проект написан на TypeScript или вы хотите предоставить типы для JavaScript пользователей.

---

## 🛠 Настройка TypeScript для генерации `.d.ts`

### Шаг 1: Установка TypeScript

```bash
npm install --save-dev typescript
````

---

### Шаг 2: Создание `tsconfig.json`

```json
{
  "compilerOptions": {
    "declaration": true,            // 💡 Генерация .d.ts файлов
    "emitDeclarationOnly": true,    // Только декларации (без .js файлов, опционально)
    "outDir": "dist",               // Папка вывода
    "module": "ESNext",
    "target": "ESNext",
    "moduleResolution": "Node",
    "jsx": "react-jsx",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "baseUrl": "./src",
    "paths": {
      "@/*": ["./*"]
    }
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist", "tests"]
}
```

> 💡 `emitDeclarationOnly` полезен, если вы используете `Vite`, `Rollup` или `SWC` для сборки JS/TS и хотите отдельно генерировать `.d.ts`.

---

## 🔄 Генерация типов

Выполните:

```bash
tsc --project tsconfig.json
```

> ✅ В результате в папке `dist/` появятся файлы `.d.ts`.

---

## 🧪 Пример структуры

```
my-ui-lib/
├── src/
│   └── Button.tsx
├── dist/
│   └── Button.d.ts
├── package.json
└── tsconfig.json
```

---

## 📦 Пример настройки в `package.json`

```json
{
  "name": "my-ui-lib",
  "version": "1.0.0",
  "main": "dist/index.js",
  "module": "dist/index.js",
  "types": "dist/index.d.ts",
  "files": ["dist"]
}
```

> Обязательно укажите поле `"types"` — путь к главному `.d.ts` файлу.

---

## 📂 Использование в библиотеке компонентов

Пример компонента:

```tsx
// src/components/Button.tsx
import React from "react";

export interface ButtonProps {
  label: string;
  onClick?: () => void;
}

export const Button = ({ label, onClick }: ButtonProps) => (
  <button onClick={onClick}>{label}</button>
);
```

Сгенерированный `.d.ts` будет:

```ts
export interface ButtonProps {
  label: string;
  onClick?: () => void;
}

export declare const Button: ({ label, onClick }: ButtonProps) => JSX.Element;
```

---

## 🧰 Библиотеки для генерации и оптимизации `.d.ts`

| Библиотека             | Назначение                                   |
| ---------------------- | -------------------------------------------- |
| `tsc`                  | Стандартный генератор от TypeScript          |
| `rollup-plugin-dts`    | Генерация `.d.ts` из `Rollup` бандла         |
| `dts-bundle-generator` | Создание единого `.d.ts` файла из всех       |
| `api-extractor`        | Генерация публичных API-типов и документации |

---

## 🧩 Пример: генерация единого `.d.ts` с `rollup-plugin-dts`

```bash
npm install --save-dev rollup-plugin-dts
```

В `rollup.config.js`:

```js
import dts from "rollup-plugin-dts";

export default {
  input: "./dist/index.d.ts",
  output: [{ file: "dist/index.d.ts", format: "es" }],
  plugins: [dts()],
};
```

---

## 🚧 Потенциальные проблемы

| Проблема                                | Решение                                             |
| --------------------------------------- | --------------------------------------------------- |
| Не создаются `.d.ts` файлы              | Проверь `tsconfig.json` — опция `declaration: true` |
| Типы не отображаются при импорте из npm | Убедитесь, что `types` указан в `package.json`      |
| Типы ссылаются на `src/` вместо `dist/` | Настройте `baseUrl` и `paths` в `tsconfig.json`     |

---

## 📚 Дополнительные ресурсы

* [TypeScript Handbook – Declaration Files](https://www.typescriptlang.org/docs/handbook/declaration-files/introduction.html)
* [dts-bundle-generator](https://github.com/timocov/dts-bundle-generator)
* [Rollup Plugin DTS](https://github.com/Swatinem/rollup-plugin-dts)
* [API Extractor](https://api-extractor.com/)

---
