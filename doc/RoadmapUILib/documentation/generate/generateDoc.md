

[назад](../documentation.md)


# 📚 Генерация документации

Документация — ключевой элемент UI-библиотеки. Она помогает другим разработчикам (и вам в будущем) понять структуру, поведение и способ использования компонентов. В этом документе описаны два основных подхода к генерации документации:

---

## 🧩 1. Storybook + MDX

[Storybook](https://storybook.js.org/) — это инструмент для изолированной разработки и демонстрации компонентов. С помощью **MDX** (Markdown + JSX) можно добавлять богатые описания и интерактивные примеры прямо в документацию компонентов.

### 📦 Установка

```bash
npm install @storybook/react @storybook/addon-docs --save-dev
````

> **Примечание:** Для TypeScript-проектов стоит добавить `@storybook/preset-typescript`.

### 📁 Пример структуры файлов

```
src/
├── components/
│   └── Button/
│       ├── Button.tsx
│       ├── Button.stories.mdx
│       └── Button.stories.tsx
```

### ✍ Пример `Button.stories.mdx`

```mdx
import { Meta, Story, Canvas, ArgsTable } from '@storybook/addon-docs';
import { Button } from './Button';

<Meta title="Components/Button" component={Button} />

# Кнопка

Компонент кнопки используется для триггеров действий. Поддерживает темы, размеры и кастомные состояния.

<Canvas>
  <Story name="Пример использования">
    <Button variant="primary">Нажми меня</Button>
  </Story>
</Canvas>

<ArgsTable story="Пример использования" />
```

### 📘 Пример `Button.stories.tsx`

```tsx
import React from 'react';
import { Button } from './Button';

export default {
  title: 'Components/Button',
  component: Button,
};

export const Primary = () => <Button variant="primary">Primary</Button>;
```

### ✅ Преимущества MDX:

* Позволяет писать документацию рядом с компонентами.
* Поддерживает JSX и интерактивные примеры.
* Интеграция с Storybook Canvas и ArgsTable.

---

## 📒 2. JSDoc → Markdown

Если вы предпочитаете автоматическую генерацию документации, вы можете использовать **JSDoc**-комментарии и преобразовывать их в **Markdown**.

### 📦 Установка

```bash
npm install --save-dev jsdoc jsdoc-to-markdown
```

### ✍ Пример JSDoc-комментариев

```ts
/**
 * Компонент кнопки
 *
 * @param {Object} props
 * @param {'primary' | 'secondary'} props.variant - Вариант кнопки
 * @param {React.ReactNode} props.children - Содержимое кнопки
 * @returns {JSX.Element}
 */
export const Button = ({ variant, children }) => {
  return <button className={`btn btn-${variant}`}>{children}</button>;
};
```

### 📁 Создание Markdown-документа

Создайте файл `generate-docs.js`:

```js
const fs = require('fs');
const jsdoc2md = require('jsdoc-to-markdown');

jsdoc2md.render({ files: 'src/components/**/*.tsx' }).then(output => {
  fs.writeFileSync('DOCUMENTATION.md', output);
});
```

Затем выполните:

```bash
node generate-docs.js
```

В результате вы получите файл `DOCUMENTATION.md` с описанием всех компонентов.

---

## 🔄 Сравнительная таблица

| Метод            | Формат       | Поддержка интерактивности | Подходит для Storybook | Автоматизация |
| ---------------- | ------------ | ------------------------- | ---------------------- | ------------- |
| Storybook + MDX  | Markdown+JSX | ✅ Да                      | ✅ Да                   | ❌ Нет         |
| JSDoc → Markdown | JSDoc        | ❌ Нет                     | ❌ Нет                  | ✅ Да          |

---

## 🚀 Рекомендации

* На начальных этапах используйте **Storybook + MDX** — это идеальный способ визуально демонстрировать компоненты.
* Для технической документации API или утилит используйте **JSDoc + jsdoc-to-markdown**.
* Со временем возможно объединение двух подходов: визуальные примеры через Storybook, технические детали через JSDoc.

---

## 📁 Пример итоговой структуры проекта с документацией

```
my-ui-library/
├── src/
│   ├── components/
│   │   └── Button/
│   │       ├── Button.tsx
│   │       ├── Button.stories.mdx
│   │       └── Button.test.tsx
├── .storybook/
├── DOCUMENTATION.md
├── package.json
└── README.md
```

---

## 📌 Заключение

Для полноценной, удобной и масштабируемой документации UI-библиотеки стоит использовать комбинированный подход. Он охватывает как визуальное восприятие, так и формальное API-описание компонентов.


