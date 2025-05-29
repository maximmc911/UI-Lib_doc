

[назад](../Technical_Architecture.md)



# Документация по Storybook для разработки и документирования UI-библиотеки

---

## Что такое Storybook?

**Storybook** — это инструмент для разработки, тестирования и документирования компонентов пользовательского интерфейса (UI) в изоляции от основного приложения. Он позволяет создавать «истории» (stories) — независимые примеры использования компонентов с разными состояниями и пропсами.

Storybook помогает разработчикам, дизайнерам и тестировщикам работать с компонентами независимо, что улучшает качество кода и ускоряет процесс разработки.

---

## Основные возможности Storybook

- **Изоляция компонентов** — можно видеть и тестировать компоненты отдельно от приложения.
- **Интерактивная документация** — автоматическое создание документации на основе историй и комментариев.
- **Поддержка множества фреймворков** — React, Vue, Angular, Svelte и другие.
- **Аддоны (плагины)** — расширяют функциональность (документация, тесты, accessibility, дизайн-системы и др.).
- **Демонстрация всех состояний компонента** — разные варианты пропсов, темы, размеры и т.п.
- **Инструменты для тестирования и отладки** — визуальные тесты, проверка доступности (a11y).

---

## Основные понятия Storybook

| Термин       | Описание                                                                                  |
|--------------|-------------------------------------------------------------------------------------------|
| **Story**    | Конкретный пример компонента с определённым набором пропсов и состояния.                  |
| **Stories**  | Набор историй одного компонента.                                                         |
| **Story file** | Файл с расширением `.stories.js/.tsx/.vue` где описываются истории для компонента.       |
| **Addon**    | Плагины, расширяющие функциональность Storybook (например, документация, controls, a11y).  |
| **Canvas**   | Рабочая область, где отображается выбранная история (компонент с заданными пропсами).      |
| **Docs**     | Документационный режим, где компоненты описаны и задокументированы в удобном формате.     |
| **Controls** | Панель управления для интерактивного изменения пропсов компонента прямо в UI Storybook.   |

---

## Установка и базовая настройка Storybook

### Установка

Для React-проекта установка Storybook выполняется командой:

```bash
npx sb init
````

Это автоматически добавит зависимости и создаст структуру для Storybook.

---

## Пример простой истории (React)

```tsx
import React from 'react';
import { Button } from './Button';

export default {
  title: 'Example/Button',
  component: Button,
  argTypes: {
    backgroundColor: { control: 'color' },
    onClick: { action: 'clicked' },
  },
};

const Template = (args) => <Button {...args} />;

export const Primary = Template.bind({});
Primary.args = {
  primary: true,
  label: 'Primary Button',
};

export const Secondary = Template.bind({});
Secondary.args = {
  label: 'Secondary Button',
};
```

---

## Основные конфигурационные файлы

### 1. `main.js`

Определяет, где искать истории и какие аддоны использовать:

```js
module.exports = {
  stories: ['../src/**/*.stories.@(js|jsx|ts|tsx|mdx)'],
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    '@storybook/addon-a11y',
  ],
};
```

### 2. `preview.js`

Глобальные настройки для всех историй — декораторы, параметры:

```js
export const parameters = {
  actions: { argTypesRegex: '^on[A-Z].*' },
  controls: {
    matchers: {
      color: /(background|color)$/i,
      date: /Date$/,
    },
  },
};
```

---

## Аналоги Storybook

| Инструмент       | Описание                                                                    | Плюсы                               | Минусы                             |
| ---------------- | --------------------------------------------------------------------------- | ----------------------------------- | ---------------------------------- |
| **Styleguidist** | Простой инструмент для создания styleguide и документации компонентов React | Легко настроить, быстрый старт      | Меньше возможностей, чем Storybook |
| **Docz**         | Автоматическая документация для React-компонентов на основе MDX             | Простой MDX, поддержка MD           | Меньшая экосистема                 |
| **Bit**          | Платформа для шаринга и разработки компонентов в команде                    | Управление компонентами, шаринг     | Требует интеграции                 |
| **Pattern Lab**  | Инструмент для построения дизайн-систем на основе Atomic Design             | Хорош для комплексных дизайн-систем | Более сложная настройка            |

---

## Аргументы против использования Storybook на начальном этапе разработки

Хотя Storybook — мощный инструмент, на ранних стадиях разработки UI-библиотеки или отдельных компонентов иногда удобнее обходиться без него, используя более простые методы:

* **Быстрый старт**: Создавать и тестировать компоненты в простом пустом проекте React проще и быстрее — не нужно настраивать и запускать отдельный Storybook-сервер.
* **Меньше накладных расходов**: Storybook требует дополнительной конфигурации, запуска отдельного процесса, что может замедлить разработку в самом начале.
* **Простая визуализация**: Для первых прототипов и тестов достаточно отрисовывать компоненты в корневом приложении или на отдельной тестовой странице.
* **Гибкость и контроль**: Можно полностью контролировать окружение и зависимости без ограничений и специфики Storybook.
* **Избежание переусложнения**: На ранних этапах проект может меняться быстро, а Storybook потребует постоянных обновлений конфигураций и историй, что добавляет лишнюю работу.

---

## Когда переходить к Storybook?

* Когда количество компонентов увеличится и понадобится централизованное хранилище для всех UI-элементов.
* Для создания единой документации, понятной не только разработчикам, но и дизайнерам, менеджерам.
* Для облегчения командной работы и ревью компонентов.
* При необходимости добавления автоматизированных визуальных тестов и проверок доступности.

---

## Примеры конфигурации Storybook для разных технологий

---

### React + TypeScript

1. Установка Storybook с поддержкой TypeScript:

```bash
npx sb init --type react_scripts
```

2. Пример `main.js` с TypeScript и React:

```js
module.exports = {
  stories: ['../src/**/*.stories.@(ts|tsx|js|jsx|mdx)'],
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    '@storybook/addon-a11y',
  ],
  framework: '@storybook/react',
  typescript: {
    check: true, // Включить проверку типов
    reactDocgen: 'react-docgen-typescript',
  },
};
```

3. Пример файла истории с TypeScript (`Button.stories.tsx`):

```tsx
import React from 'react';
import { Button, ButtonProps } from './Button';

export default {
  title: 'Example/Button',
  component: Button,
};

const Template = (args: ButtonProps) => <Button {...args} />;

export const Primary = Template.bind({});
Primary.args = {
  label: 'Primary Button',
  primary: true,
};
```

---

### Vue 3 + Composition API

1. Установка Storybook для Vue 3:

```bash
npx sb init --type vue3
```

2. Пример `main.js` для Vue 3:

```js
module.exports = {
  stories: ['../src/**/*.stories.@(js|jsx|ts|tsx|vue|mdx)'],
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    '@storybook/addon-a11y',
  ],
  framework: '@storybook/vue3',
};
```

3. Пример истории Vue 3 (`Button.stories.js`):

```js
import Button from './Button.vue';

export default {
  title: 'Example/Button',
  component: Button,
};

const Template = (args) => ({
  components: { Button },
  setup() {
    return { args };
  },
  template: '<Button v-bind="args" />',
});

export const Primary = Template.bind({});
Primary.args = {
  label: 'Primary Button',
  primary: true,
};
```

---

### Next.js (React + SSR)

Storybook можно использовать в проектах Next.js почти так же, как в обычных React-приложениях, но стоит учитывать специфичные настройки.

1. Установка Storybook:

```bash
npx sb init
```

2. Пример `main.js`:

```js
module.exports = {
  stories: ['../components/**/*.stories.@(js|jsx|ts|tsx|mdx)'],
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    '@storybook/addon-a11y',
  ],
  framework: '@storybook/react',
  webpackFinal: async (config) => {
    // Добавьте здесь кастомизации webpack, если нужно
    return config;
  },
};
```

3. Важно: Для правильной работы с CSS/SCSS и alias в Next.js возможно потребуется дополнительно настроить webpack через `webpackFinal`.

---

### Nuxt.js (Vue 3 + SSR)

Для Nuxt 3 есть отдельный Storybook плагин.

1. Установка в Nuxt 3:

```bash
npx nuxi init my-nuxt-app
cd my-nuxt-app
npm install
npm install -D @nuxt/storybook
```

2. Добавьте модуль в `nuxt.config.ts`:

```ts
export default defineNuxtConfig({
  modules: ['@nuxt/storybook'],
  storybook: {
    stories: ['~/components/**/*.stories.@(js|ts|vue)'],
  },
});
```

3. Запуск Storybook:

```bash
npx nuxt storybook dev
```

4. Пример файла истории (`Button.stories.js`):

```js
import Button from '~/components/Button.vue';

export default {
  title: 'Example/Button',
  component: Button,
};

const Template = (args) => ({
  components: { Button },
  setup() {
    return { args };
  },
  template: '<Button v-bind="args" />',
});

export const Primary = Template.bind({});
Primary.args = {
  label: 'Primary Button',
  primary: true,
};
```

---

## Итоговые рекомендации

| Технология | Особенности использования Storybook            | Рекомендации по настройке                         |
| ---------- | ---------------------------------------------- | ------------------------------------------------- |
| React + TS | Поддержка типизации, настройка react-docgen-ts | Включить `typescript.check` и `reactDocgen`       |
| Vue 3      | Composition API, Vue 3 фреймворк               | Использовать `@storybook/vue3`                    |
| Next.js    | SSR, специфичные webpack-настройки             | Настроить webpack в `webpackFinal`                |
| Nuxt.js    | SSR, интеграция через nuxt/storybook           | Использовать официальный модуль `@nuxt/storybook` |

---

## Заключение

Storybook — мощный инструмент, который помогает структурировать и документировать UI-компоненты для множества фреймворков. Правильная настройка позволяет получить удобное средство для разработки и презентации компонентов в изоляции. На начальном этапе разработки стоит использовать простой тестовый стенд, а с ростом библиотеки — переходить на Storybook с его богатым набором возможностей.

---

