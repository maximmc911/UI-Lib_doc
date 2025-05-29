


[назад](../Testing_and_Quality_Assurance.md)


# Подробная документация Prettier с примерами и сравнением с Tailwind CSS

---

## Что такое Prettier?

Prettier — это инструмент автоматического форматирования кода, который поддерживает единый стиль в проекте и избавляет разработчиков от рутинной задачи исправления отступов, пробелов и переносов строк.

---

## Зачем нужен Prettier?

- Обеспечивает единый стиль кода для всей команды
- Уменьшает количество споров о форматировании
- Работает с множеством языков и файлов (.js, .ts, .json, .css, .md и др.)
- Интегрируется с редакторами и CI/CD

---

## Установка Prettier

```bash
npm install --save-dev prettier
````

---

## Основная конфигурация Prettier

Создайте `.prettierrc` в корне проекта:

```json
{
  "semi": true,
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "trailingComma": "es5",
  "arrowParens": "always"
}
```

| Опция           | Описание                                                 | Пример     |
| --------------- | -------------------------------------------------------- | ---------- |
| `semi`          | Добавлять точку с запятой в конце выражений              | `true`     |
| `singleQuote`   | Использовать одинарные кавычки вместо двойных            | `true`     |
| `printWidth`    | Максимальная длина строки                                | `80`       |
| `tabWidth`      | Количество пробелов на один уровень отступа              | `2`        |
| `trailingComma` | Добавлять запятую в конце объектов и массивов            | `"es5"`    |
| `arrowParens`   | Использовать скобки вокруг параметров стрелочных функций | `"always"` |

---

## Интеграция Prettier с ESLint

Для устранения конфликтов правил между ESLint и Prettier используйте:

```bash
npm install --save-dev eslint-config-prettier
```

Пример `.eslintrc.js`:

```js
module.exports = {
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended',
    'prettier'  // Должен быть последним
  ],
  rules: {}
};
```

---

## Интеграция с редакторами (на примере VSCode)

Добавьте в настройки:

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

---

## Использование Prettier

* Форматирование всех файлов:

```bash
npx prettier --write "src/**/*.{js,ts,jsx,tsx,json,css,md}"
```

* Проверка форматирования без изменений:

```bash
npx prettier --check "src/**/*.{js,ts,jsx,tsx,json,css,md}"
```

* Добавьте скрипты в `package.json`:

```json
{
  "scripts": {
    "format": "prettier --write 'src/**/*.{js,ts,jsx,tsx,json,css,md}'",
    "format:check": "prettier --check 'src/**/*.{js,ts,jsx,tsx,json,css,md}'"
  }
}
```

---

## Поддержка Tailwind CSS с Prettier

Tailwind CSS использует utility-классы, которые могут быть сложно читать из-за порядка. Для их сортировки используется плагин:

### prettier-plugin-tailwindcss

* Автоматически сортирует классы Tailwind по рекомендуемому порядку
* Простая установка и автоматическое подключение

```bash
npm install --save-dev prettier-plugin-tailwindcss
```

### Конфигурация `.prettierrc` с плагином:

```json
{
  "semi": true,
  "singleQuote": true,
  "printWidth": 100,
  "tabWidth": 2,
  "trailingComma": "es5",
  "arrowParens": "always",
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

---

## Сравнение Prettier с и без Tailwind CSS

| Особенность           | Prettier без Tailwind            | Prettier + prettier-plugin-tailwindcss   |
| --------------------- | -------------------------------- | ---------------------------------------- |
| Форматирование JS/TS  | Полное и стандартное             | Полное, плюс сортировка классов Tailwind |
| Обработка CSS классов | Без сортировки классов           | Автоматическая сортировка классов        |
| Конфликты с ESLint    | Требует `eslint-config-prettier` | То же, плагин не влияет                  |
| Конфигурация          | Простой `.prettierrc`            | Дополнительное подключение плагина       |

---

## Пример полного рабочего проекта React + Tailwind + Prettier

```bash
npm install --save-dev prettier prettier-plugin-tailwindcss eslint eslint-config-prettier eslint-plugin-react
```

`.prettierrc`:

```json
{
  "semi": true,
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "trailingComma": "es5",
  "arrowParens": "always",
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

`.eslintrc.js`:

```js
module.exports = {
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'prettier'
  ],
  settings: {
    react: { version: 'detect' }
  }
};
```

---

## Важные рекомендации

* **Используйте Prettier с ESLint** — так достигается лучший контроль качества и стиля.
* **Автоматизируйте форматирование в CI/CD** — например, запускайте `prettier --check` перед мержем.
* **Подключайте prettier-plugin-tailwindcss, если используете Tailwind CSS** — для удобочитаемости и поддержки единого стиля классов.
* **Используйте автоформатирование в редакторе** — экономит время и исключает ошибки.

---

## Заключение

Prettier — универсальный и незаменимый инструмент для поддержания чистого и единообразного кода. В сочетании с плагином для Tailwind CSS он значительно облегчает работу с utility-классами и повышает качество фронтенд-проектов.

---

## Полезные ссылки

* [Официальный сайт Prettier](https://prettier.io/)
* [prettier-plugin-tailwindcss](https://github.com/tailwindlabs/prettier-plugin-tailwindcss)
* [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)
* [Prettier VSCode extension](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)


