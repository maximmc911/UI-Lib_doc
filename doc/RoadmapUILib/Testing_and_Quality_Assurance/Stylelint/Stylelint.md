


[назад](../Testing_and_Quality_Assurance.md)


# Документация по Stylelint

---

## Что такое Stylelint?

**Stylelint** — это мощный современный линтер для CSS и его препроцессоров (SCSS, Less и других), который помогает находить ошибки в стилях, следить за соблюдением код-стайла и предотвращать потенциальные баги.

---

## Зачем использовать Stylelint?

- Автоматический контроль качества CSS/SCSS кода
- Поддержка современных стандартов CSS и препроцессоров
- Возможность создания и использования кастомных правил и конфигураций
- Интеграция с редакторами, CI/CD, сборщиками
- Поддержка плагинов для Tailwind CSS и других CSS-фреймворков

---

## Установка

```bash
npm install --save-dev stylelint stylelint-config-standard
````

> Опционально для SCSS:

```bash
npm install --save-dev stylelint-scss
```

---

## Основная конфигурация

Создайте файл `.stylelintrc.json` в корне проекта:

```json
{
  "extends": ["stylelint-config-standard"],
  "rules": {
    "indentation": 2,
    "string-quotes": "double",
    "color-hex-case": "lower",
    "color-hex-length": "short",
    "max-empty-lines": 2,
    "no-descending-specificity": true,
    "selector-class-pattern": "^[a-z0-9\\-]+$"
  }
}
```

| Правило                     | Описание                                       | Пример             |
| --------------------------- | ---------------------------------------------- | ------------------ |
| `indentation`               | Количество пробелов для отступа                | `2`                |
| `string-quotes`             | Используемые кавычки в строках                 | `"double"`         |
| `color-hex-case`            | Регистр букв в hex-цветах                      | `"lower"`          |
| `color-hex-length`          | Короткая или полная форма hex-цветов           | `"short"`          |
| `max-empty-lines`           | Максимальное количество пустых строк           | `2`                |
| `no-descending-specificity` | Запрет на селекторы с убывающей специфичностью | `true`             |
| `selector-class-pattern`    | Регулярное выражение для названия CSS классов  | `"^[a-z0-9\\-]+$"` |

---

## Интеграция с Tailwind CSS

Для корректной работы с Tailwind CSS и его utility-классами используйте специальный плагин:

```bash
npm install --save-dev stylelint-config-tailwindcss
```

Пример расширенной конфигурации `.stylelintrc.json`:

```json
{
  "extends": [
    "stylelint-config-standard",
    "stylelint-config-tailwindcss"
  ],
  "rules": {
    "at-rule-no-unknown": [
      true,
      {
        "ignoreAtRules": ["tailwind", "apply", "variants", "responsive", "screen"]
      }
    ]
  }
}
```

---

## Использование Stylelint

### Запуск проверки вручную

```bash
npx stylelint "**/*.{css,scss}"
```

### Автоматическое исправление ошибок

```bash
npx stylelint "**/*.{css,scss}" --fix
```

---

## Интеграция с редакторами

* **VSCode**: установка расширения [Stylelint](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint) для мгновенной подсветки ошибок и автозаполнения.

---

## Пример использования с Tailwind CSS

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

.button {
  @apply px-4 py-2 bg-blue-500 text-white rounded;
}
```

С Stylelint с плагином Tailwind CSS вы можете проверять такие файлы без ложных срабатываний.

---

## Интеграция с CI/CD

Добавьте в скрипты `package.json`:

```json
{
  "scripts": {
    "lint:css": "stylelint '**/*.{css,scss}'",
    "lint:css:fix": "stylelint '**/*.{css,scss}' --fix"
  }
}
```

Запускайте `npm run lint:css` на этапе проверки кода перед публикацией.

---

## Частые полезные плагины

| Плагин                         | Описание                        | Установка                               |
| ------------------------------ | ------------------------------- | --------------------------------------- |
| `stylelint-scss`               | Правила для SCSS                | `npm i -D stylelint-scss`               |
| `stylelint-order`              | Проверка порядка свойств CSS    | `npm i -D stylelint-order`              |
| `stylelint-config-recommended` | Базовые рекомендуемые настройки | Входит в `stylelint-config-standard`    |
| `stylelint-config-tailwindcss` | Поддержка Tailwind CSS          | `npm i -D stylelint-config-tailwindcss` |

---

## Советы и рекомендации

* Используйте конфигурации сообществ, чтобы не создавать правила с нуля.
* Интегрируйте Stylelint в редактор для моментального выявления проблем.
* Автоматизируйте исправление в CI/CD.
* Комбинируйте с Prettier для полного контроля стиля и форматирования.

---

## Полезные ссылки

* [Официальный сайт Stylelint](https://stylelint.io/)
* [Stylelint Config Tailwind CSS](https://github.com/tailwindlabs/stylelint-config-tailwindcss)
* [Плагины Stylelint](https://stylelint.io/user-guide/plugins/)
* [Расширения для VSCode](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint)

---

## Заключение

Stylelint — незаменимый инструмент для поддержки качества CSS и препроцессорных стилей. Он помогает сохранять код чистым, удобочитаемым и соответствующим стандартам, что критично для масштабируемых проектов и командной разработки.
