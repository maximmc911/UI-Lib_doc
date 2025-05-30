

[назад](../Support_and_Development.md)



# Документация по GitHub Actions для линтинга, тестов и релизов

---

## Введение

GitHub Actions — это встроенный в GitHub сервис CI/CD, который позволяет автоматизировать процессы разработки: запускать линтинг, тесты, сборку и релизы при разных событиях (push, pull request и т.д.).

В этой документации показано, как настроить GitHub Actions для:

- Автоматического запуска линтинга кода
- Запуска тестов
- Автоматизации релизов (сборка и публикация релизов)

---

## 1. Линтинг

### 1.1. Зачем нужен линтинг?

Линтинг помогает обнаруживать ошибки в коде, следить за стилем и качеством кода ещё до сборки и деплоя.

### 1.2. Пример workflow для ESLint (Node.js / JavaScript)

Создайте файл `.github/workflows/lint.yml`:

```yaml
name: Lint

on:
  push:
    branches:
      - main
      - develop
  pull_request:

jobs:
  lint:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'

      - name: Install dependencies
        run: npm install

      - name: Run ESLint
        run: npm run lint
````

### 1.3. Что должен содержать `package.json`

```json
{
  "scripts": {
    "lint": "eslint ."
  },
  "devDependencies": {
    "eslint": "^8.0.0"
  }
}
```

---

## 2. Запуск тестов

### 2.1. Зачем нужны тесты?

Тесты помогают проверять корректность работы кода и предотвращать регрессии.

### 2.2. Пример workflow для Jest

Создайте файл `.github/workflows/test.yml`:

```yaml
name: Test

on:
  push:
    branches:
      - main
      - develop
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

### 2.3. Скрипт для тестов в `package.json`

```json
{
  "scripts": {
    "test": "jest"
  },
  "devDependencies": {
    "jest": "^29.0.0"
  }
}
```

---

## 3. Автоматизация релизов

### 3.1. Что такое релизы?

Релизы — зафиксированные версии кода, которые можно распространять, скачивать, использовать в production.

### 3.2. Пример workflow для автоматического создания релизов с помощью `actions/create-release`

Создайте файл `.github/workflows/release.yml`:

```yaml
name: Release

on:
  push:
    tags:
      - 'v*.*.*'  # Триггер на теги, например, v1.2.3

jobs:
  release:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'

      - name: Install dependencies
        run: npm install

      - name: Build project
        run: npm run build

      - name: Create GitHub Release
        id: create_release
        uses: actions/create-release@v1
        with:
          tag_name: ${{ github.ref_name }}
          release_name: Release ${{ github.ref_name }}
          draft: false
          prerelease: false
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - name: Upload release asset
        uses: actions/upload-release-asset@v1
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: ./dist/app.zip
          asset_name: app.zip
          asset_content_type: application/zip
```

### 3.3. Что должен содержать `package.json`

```json
{
  "scripts": {
    "build": "webpack --mode production"
  },
  "devDependencies": {
    "webpack": "^5.0.0",
    "webpack-cli": "^4.0.0"
  }
}
```

---

## 4. Общие рекомендации

* Используйте разные workflows для разных целей (линтинг, тесты, сборка).
* Настраивайте триггеры (`on`) под свои процессы разработки (push, pull\_request, schedule).
* Используйте секреты GitHub (`GITHUB_TOKEN`) для безопасного взаимодействия с API.
* Добавляйте кэширование зависимостей (например, `actions/cache`) для ускорения выполнения.
* Ведите версионирование тегов в формате `vX.Y.Z` для релизов.

---

## 5. Дополнительные примеры и ссылки

* [GitHub Actions official docs](https://docs.github.com/en/actions)
* [actions/setup-node](https://github.com/actions/setup-node)
* [actions/create-release](https://github.com/actions/create-release)
* [actions/upload-release-asset](https://github.com/actions/upload-release-asset)

---

## Заключение

Автоматизация линтинга, тестирования и релизов с помощью GitHub Actions значительно улучшает качество кода и ускоряет процесс разработки. Правильное разделение процессов в разные workflows упрощает поддержку CI/CD.

---
