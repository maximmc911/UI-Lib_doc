

[назад](../Support_and_Development.md)



# Документация по настройке Husky + lint-staged для pre-commit хуков

---

## Введение

**Husky** — это инструмент для управления Git хуками, который позволяет запускать скрипты на определённых этапах работы с Git (например, до коммита — pre-commit).

**lint-staged** — утилита, которая позволяет запускать команды (например, линтинг или форматирование) только над теми файлами, которые были изменены и подготовлены к коммиту (staged).

Используя Husky вместе с lint-staged, можно настроить автоматическую проверку и исправление кода перед каждым коммитом, что повышает качество кода и предотвращает ошибки.

---

## 1. Установка

В корне проекта выполните:

```bash
npm install --save-dev husky lint-staged
````

Или, если используете yarn:

```bash
yarn add -D husky lint-staged
```

---

## 2. Инициализация Husky

```bash
npx husky install
```

Эта команда создаст папку `.husky/` с необходимой структурой.

Чтобы Husky автоматически активировался после установки зависимостей, добавьте в `package.json`:

```json
{
  "scripts": {
    "prepare": "husky install"
  }
}
```

---

## 3. Создание pre-commit хука

Для создания pre-commit хука выполните:

```bash
npx husky add .husky/pre-commit "npx lint-staged"
```

Это создаст файл `.husky/pre-commit` с содержимым, запускающим lint-staged перед каждым коммитом.

---

## 4. Настройка lint-staged

Добавьте в `package.json` раздел `lint-staged`, где укажите команды, которые должны запускаться над изменёнными файлами.

### Пример для проекта на JavaScript/TypeScript с ESLint и Prettier:

```json
{
  "lint-staged": {
    "*.{js,jsx,ts,tsx}": [
      "eslint --fix",
      "prettier --write",
      "git add"
    ],
    "*.{json,md}": [
      "prettier --write",
      "git add"
    ]
  }
}
```

---

## 5. Как это работает

* При попытке сделать коммит Husky сработает и вызовет `lint-staged`.
* `lint-staged` найдёт все файлы, подготовленные к коммиту (`staged`), которые соответствуют паттернам (например, `*.js`, `*.ts`).
* Для каждого файла будут запущены указанные команды (например, `eslint --fix`, `prettier --write`).
* После исправления файлов они будут снова добавлены в индекс (`git add`), чтобы коммит включал исправленный код.
* Если линтинг или форматирование не пройдёт — коммит не будет выполнен, что позволит исправить ошибки до попадания кода в репозиторий.

---

## 6. Полный пример `package.json`

```json
{
  "scripts": {
    "prepare": "husky install",
    "lint": "eslint .",
    "format": "prettier --write ."
  },
  "devDependencies": {
    "eslint": "^8.0.0",
    "husky": "^8.0.0",
    "lint-staged": "^13.0.0",
    "prettier": "^2.0.0"
  },
  "lint-staged": {
    "*.{js,jsx,ts,tsx}": [
      "eslint --fix",
      "prettier --write",
      "git add"
    ],
    "*.{json,md}": [
      "prettier --write",
      "git add"
    ]
  }
}
```

---

## 7. Дополнительные советы

* Убедитесь, что ESLint и Prettier настроены в проекте.
* Можно использовать другие команды для проверки стилей, тестов и т.п.
* В хуках можно запускать скрипты на любой стадии Git: pre-push, commit-msg и другие.
* Для проверки конфигурации Husky используйте `git commit` — если есть ошибки, коммит не пройдет.

---

## 8. Полезные ссылки

* [Husky GitHub](https://github.com/typicode/husky)
* [lint-staged GitHub](https://github.com/okonet/lint-staged)
* [ESLint](https://eslint.org/)
* [Prettier](https://prettier.io/)

---

## Заключение

Настройка Husky + lint-staged помогает автоматизировать качество кода и избежать ошибок, сохраняя кодовую базу чистой и соответствующей стандартам.

---
