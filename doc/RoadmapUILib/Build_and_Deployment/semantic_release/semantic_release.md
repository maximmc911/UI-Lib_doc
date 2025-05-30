


[назад](../Build_and_Deployment.md)


# 📦 Управление версиями: Semantic Release vs Ручное версионирование

---

## 🔍 Описание

Поддержание версий — важная часть выпуска библиотеки. Это позволяет пользователям понимать, когда происходят:

- 🛠 Исправления (`patch`)
- ✨ Новые функции (`minor`)
- 💥 Ломающие изменения (`major`)

Существует два подхода:

- **🧑‍💻 Ручное версионирование** — вы вручную меняете версию в `package.json`, создаёте теги и публикуете.
- **🤖 Semantic Release** — автоматизирует весь процесс: анализирует коммиты, определяет версию, создает Git-тег и публикует.

---

## 📌 Сравнительная таблица

| Характеристика                | Semantic Release            | Ручное версионирование      |
|-----------------------------|-----------------------------|-----------------------------|
| Автоматизация                | ✅ Полная                   | ❌ Нет                      |
| Требует стандартных коммитов | ✅ Да                       | ❌ Нет                      |
| Ошибки из-за человеческого фактора | ❌ Минимальны            | ✅ Часто                   |
| Настройка                    | 🟡 Средняя сложность        | 🟢 Простая                 |
| Гибкость                     | ✅ Высокая                  | ✅ Высокая                 |
| Контроль вручную             | ❌ Нет                      | ✅ Полный                  |

---

## 🧑‍💻 Ручное версионирование

### 🔧 Шаги

1. Обновите версию вручную:

```bash
npm version patch     # или minor, major
````

2. Это:

   * Обновит `package.json`
   * Создаст Git-коммит
   * Добавит Git-тег

3. Запушьте:

```bash
git push origin main --tags
```

4. Опубликуйте:

```bash
npm publish
```

### ✅ Плюсы

* Простота и контроль
* Не требует настроек

### ❌ Минусы

* Легко забыть шаг
* Высокий шанс ошибки

---

## 🤖 Semantic Release

### 📦 Что делает

* Анализирует коммиты (по Conventional Commits)
* Вычисляет версию
* Обновляет changelog
* Создаёт тег и публикует в npm
* Пушит изменения обратно в репозиторий

---

## 🚀 Установка

```bash
npm install -D semantic-release @semantic-release/changelog @semantic-release/git @semantic-release/npm
```

---

## 📄 Пример `.releaserc.json`

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    [
      "@semantic-release/changelog",
      {
        "changelogFile": "CHANGELOG.md"
      }
    ],
    "@semantic-release/npm",
    [
      "@semantic-release/git",
      {
        "assets": ["package.json", "CHANGELOG.md"],
        "message": "chore(release): ${nextRelease.version}"
      }
    ]
  ]
}
```

---

## 🤝 Conventional Commits

Semantic Release использует соглашение:

* `fix:` — для `patch`
* `feat:` — для `minor`
* `BREAKING CHANGE:` — для `major`

**Примеры:**

```bash
git commit -m "fix(button): исправлен отступ"
git commit -m "feat(button): добавлена иконка"
git commit -m "refactor!: удалён устаревший компонент\n\nBREAKING CHANGE: компонент удалён"
```

---

## 🧪 Настройка GitHub Actions

```yaml
# .github/workflows/release.yml
name: Release

on:
  push:
    branches:
      - main

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Установка Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          registry-url: 'https://registry.npmjs.org/'

      - name: Установка зависимостей
        run: npm ci

      - name: Semantic Release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
        run: npx semantic-release
```

---

## 🔐 Переменные окружения

* `GITHUB_TOKEN` — автоматически доступен в GitHub Actions
* `NPM_TOKEN` — создаётся в [npm → Access Tokens](https://www.npmjs.com/settings/<ваш-логин>/tokens)

---

## 📝 Автоматическое обновление CHANGELOG

Semantic Release автоматически обновляет `CHANGELOG.md`, используя `@semantic-release/changelog` и `@semantic-release/git`.

---

## 🧠 Рекомендации

* Используйте Semantic Release, если:

  * У вас есть CI/CD
  * Вы хотите автоматизировать публикацию и changelog
* Оставайтесь на ручной версии, если:

  * Проект небольшой
  * Вы не хотите поддерживать сложную инфраструктуру

---

## 📚 Полезные ссылки

* [Semantic Release Docs](https://semantic-release.gitbook.io)
* [Conventional Commits](https://www.conventionalcommits.org/ru/)
* [npm version](https://docs.npmjs.com/cli/v9/commands/npm-version)

---

