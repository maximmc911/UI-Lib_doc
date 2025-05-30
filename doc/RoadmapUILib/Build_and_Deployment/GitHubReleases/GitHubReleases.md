

[назад](../Build_and_Deployment.md)



# 📦 GitHub Releases + CHANGELOG.md

Эта документация описывает лучшие практики управления релизами с помощью **GitHub Releases** и ведения **CHANGELOG.md** в проектах с открытым и закрытым исходным кодом.

---

## 🧭 Что такое GitHub Releases?

**GitHub Releases** — это способ объявлять стабильные версии проекта с тегом и описанием изменений. Позволяет:

- легко следить за историей изменений;
- удобно загружать артефакты (бандлы, документацию и т.д.);
- автоматически уведомлять пользователей о новых версиях.

---

## 📝 Что такое CHANGELOG.md?

**CHANGELOG.md** — это файл в корне репозитория, где ведется список всех значимых изменений по версиям, включая:

- добавленные функции,
- изменения API,
- исправления багов,
- изменения в инфраструктуре.

> Формат changelog'а: [Keep a Changelog](https://keepachangelog.com/ru/1.0.0/)

---

## 📁 Структура CHANGELOG.md

```md
# 📓 Changelog

Все заметные изменения в этом проекте будут задокументированы в этом файле.

Форматирование согласно [Keep a Changelog](https://keepachangelog.com/ru/1.0.0/).

## [1.2.0] — 2025-05-30
### Добавлено
- Компонент `<Accordion />`
- Хук `useToggle()`
- Поддержка SSR (Next.js)

### Изменено
- Обновлён компонент `<Button />` для поддержки иконок

### Исправлено
- Ошибка с фокусом в `<Input />`

## [1.1.0] — 2025-05-15
### Добавлено
- Компонент `<Input />`
- Стилизация по умолчанию

## [1.0.0] — 2025-05-01
### Добавлено
- Первый релиз: `<Button />`
````

---

## 🔧 Как вести CHANGELOG.md вручную

1. При каждом Pull Request, относящемся к релизу — **обновляйте CHANGELOG.md**
2. Категории для изменений:

   * `### Добавлено`
   * `### Изменено`
   * `### Удалено`
   * `### Исправлено`
   * `### Безопасность`
3. Последний раздел может быть `[Unreleased]`, чтобы фиксировать грядущие изменения.

---

## 🚀 Использование GitHub Releases

### 1. Создание релиза через GitHub UI

1. Перейдите во вкладку **Releases** в репозитории: `https://github.com/username/repo/releases`
2. Нажмите **"Draft a new release"**
3. Укажите **tag** (`v1.2.0`)
4. Назовите версию (`Release 1.2.0`)
5. Вставьте соответствующий раздел из `CHANGELOG.md`
6. При необходимости загрузите артефакты (архив, сборку и т.д.)
7. Опубликуйте релиз

### 2. Использование Git CLI

```bash
git tag -a v1.2.0 -m "Release v1.2.0"
git push origin v1.2.0
```

После этого GitHub автоматически создаст страницу релиза (можно отредактировать вручную).

---

## 🔄 Автоматизация с `standard-version`

Можно автоматизировать создание CHANGELOG и релизов с помощью [`standard-version`](https://github.com/conventional-changelog/standard-version):

### Установка:

```bash
npm install --save-dev standard-version
```

### Добавьте в `package.json`:

```json
{
  "scripts": {
    "release": "standard-version"
  }
}
```

### Создание релиза:

```bash
npm run release
git push --follow-tags origin main
```

### Автоматический CHANGELOG

Файл `CHANGELOG.md` будет обновлён на основе commit-месседжей в формате Conventional Commits (`feat:`, `fix:`, и т.д.)

---

## 🛡 Пример Conventional Commits

```bash
git commit -m "feat(button): добавлен prop size"
git commit -m "fix(input): устранён баг с placeholder"
git commit -m "chore: обновлён README"
```

---

## 🧪 Использование с GitHub Actions

Создание релиза при пуше тега:

```yaml
# .github/workflows/release.yml
name: Create GitHub Release

on:
  push:
    tags:
      - 'v*.*.*'

jobs:
  release:
    name: Release
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Create Release
        uses: softprops/action-gh-release@v1
        with:
          body_path: CHANGELOG.md
```

---

## 🟢 Рекомендации

| Практика                                | Описание                                        |
| --------------------------------------- | ----------------------------------------------- |
| ✏️ Вести `CHANGELOG.md` вручную         | Лучше всего отражает реальный смысл изменений   |
| 🧪 Использовать `standard-version`      | Хорошо для автоматизации с Conventional Commits |
| 🎯 Писать meaningful commit messages    | Это влияет на читаемость changelog и релизов    |
| 🧩 Использовать GitHub Releases         | Удобно для пользователей и CI/CD систем         |
| 🧷 Теги должны соответствовать `semver` | Пример: `v1.2.3`                                |

---

## 📚 Полезные ссылки

* [Keep a Changelog](https://keepachangelog.com/ru/1.0.0/)
* [GitHub Releases Docs](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases)
* [Standard Version](https://github.com/conventional-changelog/standard-version)
* [Conventional Commits](https://www.conventionalcommits.org/ru/v1.0.0/)
* [Softprops GH Action](https://github.com/softprops/action-gh-release)

```

---

Если хочешь — могу дополнительно подготовить шаблоны GitHub Actions, pre-release конфигурации или скрипты для автообновления версий.
```
