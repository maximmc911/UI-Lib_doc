

[назад](../Testing_and_Quality_Assurance.md)


# Качество кода: ESLint с поддержкой Tailwind CSS и правил доступности (A11y)

---

## Что такое ESLint?

ESLint — это инструмент статического анализа кода для JavaScript и TypeScript, который помогает выявлять ошибки и проблемные места в коде на ранних этапах. Он помогает поддерживать единый стиль кода, улучшать качество и предотвращать баги.

---

## Почему важно использовать ESLint?

- Автоматическое обнаружение синтаксических и логических ошибок
- Принудительное соблюдение код-стайла и соглашений команды
- Поддержка плагинов и кастомных правил
- Интеграция с редакторами (VSCode, WebStorm и др.)
- Улучшение читаемости и сопровождения кода
- Повышение качества продукта и удобства командной работы

---

## Поддержка Tailwind CSS в ESLint

Tailwind CSS использует утилиты, часто генерируемые динамически (например, `className="bg-blue-500 p-4"`). Для корректного анализа таких классов и предотвращения ложных ошибок в ESLint существует плагин:

- **eslint-plugin-tailwindcss**

### Возможности плагина:

- Проверка правильности написания классов Tailwind
- Предупреждения о неиспользуемых классах
- Поддержка автокомплита и рекомендаций

### Установка:

```bash
npm install --save-dev eslint-plugin-tailwindcss
````

### Конфигурация в `.eslintrc.js`:

```js
module.exports = {
  plugins: ['tailwindcss'],
  extends: [
    // другие расширения
    'plugin:tailwindcss/recommended',
  ],
  rules: {
    // кастомные правила
  },
};
```

---

## Правила доступности (A11y) в ESLint

Доступность — важнейший аспект при создании UI, гарантирующий, что сайт удобен и доступен для людей с ограниченными возможностями.

Для автоматической проверки доступности в коде React используют плагин:

* **eslint-plugin-jsx-a11y**

### Возможности:

* Проверка правильного использования ARIA-атрибутов
* Проверка наличия альтернативных текстов (alt) у изображений
* Предупреждения о проблемах с клавиатурной навигацией
* Поддержка большинства распространённых ошибок доступности

### Установка:

```bash
npm install --save-dev eslint-plugin-jsx-a11y
```

### Конфигурация в `.eslintrc.js`:

```js
module.exports = {
  plugins: ['jsx-a11y'],
  extends: [
    // другие расширения
    'plugin:jsx-a11y/recommended',
  ],
  rules: {
    // кастомные правила
  },
};
```

---

## Рекомендуемая конфигурация ESLint для UI-библиотеки с Tailwind и A11y

```js
module.exports = {
  env: {
    browser: true,
    es2021: true,
    node: true,
  },
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 13,
    sourceType: 'module',
  },
  plugins: ['react', '@typescript-eslint', 'jsx-a11y', 'tailwindcss'],
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:jsx-a11y/recommended',
    'plugin:tailwindcss/recommended',
    'prettier',
  ],
  rules: {
    // пример кастомных правил
    'react/react-in-jsx-scope': 'off', // для React 17+
    'tailwindcss/classnames-order': 'warn',
    'tailwindcss/no-custom-classname': 'off',
    // дополнительные правила
  },
  settings: {
    react: {
      version: 'detect',
    },
  },
};
```

---

## Интеграция ESLint в проект

1. **Установка ESLint и плагинов**

```bash
npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint-plugin-react eslint-plugin-jsx-a11y eslint-plugin-tailwindcss prettier eslint-config-prettier
```

2. **Создание/обновление конфигурации `.eslintrc.js`**

(пример см. выше)

3. **Добавление скрипта для запуска**

В `package.json`:

```json
"scripts": {
  "lint": "eslint 'src/**/*.{js,jsx,ts,tsx}' --fix"
}
```

4. **Запуск линтинга**

```bash
npm run lint
```

---

## Примеры использования ESLint с Tailwind CSS и без него

### Пример 1. Компонент React с Tailwind CSS

```tsx
import React from 'react';

const Button = () => {
  return (
    <button
      className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
      aria-label="Submit form"
    >
      Submit
    </button>
  );
};

export default Button;
```

* ESLint с плагином `eslint-plugin-tailwindcss` проверит, что классы Tailwind написаны корректно и упорядочены.
* Плагин `eslint-plugin-jsx-a11y` гарантирует, что атрибут `aria-label` присутствует для доступности.

---

### Пример 2. Компонент React с чистым CSS и CSS-модулями

```tsx
import React from 'react';
import styles from './Button.module.css';

const Button = () => {
  return (
    <button className={styles.button} aria-label="Submit form">
      Submit
    </button>
  );
};

export default Button;
```

```css
/* Button.module.css */
.button {
  background-color: #3b82f6; /* tailwind bg-blue-500 */
  color: white;
  font-weight: bold;
  padding: 0.5rem 1rem;
  border-radius: 0.375rem;
  transition: background-color 0.3s;
}

.button:hover {
  background-color: #1e40af; /* tailwind bg-blue-700 */
}
```

* В этом случае ESLint проверит правила React и доступности (jsx-a11y), но плагин tailwindcss не применяется.
* В дальнейшем, при переходе на Tailwind, можно подключить плагин без кардинальных изменений.

---

## Советы и рекомендации

* Если вы используете Tailwind — обязательно подключайте `eslint-plugin-tailwindcss` для отслеживания ошибок в классах.
* Если пишете компоненты с CSS-модулями или чистым CSS, плагин Tailwind можно временно отключить, чтобы избежать ложных предупреждений.
* В любом случае плагин доступности `eslint-plugin-jsx-a11y` обязателен для улучшения качества UI.
* Для поддержания единого стиля и минимизации ошибок используйте Prettier в связке с ESLint.

---

## Заключение

Настройка ESLint с учётом специфики Tailwind CSS и правил доступности — обязательный этап в разработке качественной UI-библиотеки. Такой подход обеспечивает чистый, поддерживаемый и доступный код, который легко масштабируется и адаптируется к будущим изменениям.

---

## Полезные ссылки

* [ESLint](https://eslint.org/)
* [eslint-plugin-tailwindcss](https://github.com/francoismassart/eslint-plugin-tailwindcss)
* [eslint-plugin-jsx-a11y](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y)
* [Prettier](https://prettier.io/)

