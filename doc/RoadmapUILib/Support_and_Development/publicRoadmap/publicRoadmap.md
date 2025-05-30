

[назад](../Support_and_Development.md)




# 🗺 Дорожная карта (Public Roadmap)

Публичная дорожная карта (roadmap) — это инструмент, который позволяет прозрачно демонстрировать планы развития проекта, этапы разработки, текущие задачи и долгосрочные цели. Это важный элемент для построения доверия в open-source сообществе и среди пользователей продукта.

---

## 📌 Зачем нужен публичный roadmap?

### Преимущества:

- 📣 **Прозрачность** — пользователи знают, что в разработке.
- 🧭 **Ориентир** — помогает команде придерживаться курса.
- 🤝 **Сообщество** — люди могут предлагать идеи и голосовать за фичи.
- 🔧 **Планирование релизов** — удобно отслеживать, что входит в будущие версии.
- 📊 **Аналитика** — видны приоритеты и частота запросов на фичи.

---

## 🛠 Способы реализации

| Способ                      | Подходит для               | Плюсы                                             | Минусы                                   |
|----------------------------|----------------------------|---------------------------------------------------|------------------------------------------|
| 📌 GitHub Projects (v2)     | Open-source проекты        | Прямо в GitHub, интерактивно, колонки Kanban      | Требует GitHub-аккаунта                   |
| 🗳 Canny / Upvoty           | SaaS-продукты              | Голосование, публичные предложения, лейблы        | Не всегда бесплатно, внешний сервис       |
| 📃 Static Roadmap Page     | Документационные сайты     | Полная кастомизация, можно в Markdown             | Нет интерактивности, нужно поддерживать   |
| 📁 Notion + Public link    | Внутренние команды         | Быстрый старт, markdown + визуально               | Меньше гибкости                          |
| 📦 Trello (Public Board)   | Простые команды             | Канбан, открытый доступ                           | Не интегрируется с кодовой базой напрямую |
| 🧩 Linear (Public View)    | Продвинутые команды         | Гибкость, линейная разработка                     | Платный, не GitHub-native                 |

---

## ✅ Рекомендуемый подход

На старте библиотеки можно использовать **GitHub Projects v2 + pinned issue**. В будущем — опционально дополнить:

- Статической страницей в документации: `docs/roadmap.md`
- Голосованием через Canny / Upvoty (если будет большая аудитория)

---

## 🚀 Пример: GitHub Projects v2

Создание roadmap-доски:

1. Перейдите в ваш GitHub репозиторий → **Projects** → **New Project (beta)**
2. Выберите шаблон `Roadmap`
3. Создайте колонки:
   - 🧪 Backlog
   - 🚧 In Progress
   - ✅ Done
4. Добавьте задачи/issue и назначьте их на колонки

Пример задачи:

```md
### ✨ Добавить поддержку кастомной темизации

- Тип: Фича
- Приоритет: Средний
- Версия: 0.3.0
- Issue: #12
````

---

## 📝 Пример статической страницы `docs/roadmap.md`

```md
# 📌 Дорожная карта UI-библиотеки

## 🧪 В процессе (v0.2.0)

- ✅ Поддержка TypeScript
- ✅ Базовые компоненты (Button, Input)
- 🚧 Темизация через CSS-переменные
- 🚧 Конфигурация Storybook

## 🔜 Планируется (v0.3.0)

- 🧩 Поддержка анимаций (Framer Motion)
- 🌓 Темная тема
- 📦 Публикация в npm

## 💡 Идеи

- Поддержка Vue 3
- Интеграция с Tailwind
- Playground с интерактивным кодом

## 🗳 Предложить идею

Вы можете предложить улучшение, создав issue с меткой `enhancement`.

👉 [Создать предложение](https://github.com/ваш-репозиторий/issues/new?labels=enhancement)
```

---

## 🧠 Полезные советы

* Указывайте приоритет и ожидаемую версию релиза
* Упрощайте язык, чтобы roadmap был понятен всем
* Ссылайтесь на задачи и pull requests
* Используйте эмодзи и визуальные маркеры 🎯

---

## 📚 Примеры хороших roadmap

* [Radix UI Roadmap](https://github.com/radix-ui/primitives/projects/1)
* [Next.js Roadmap](https://nextjs.org/roadmap)
* [Tailwind CSS Roadmap](https://github.com/tailwindlabs/tailwindcss/discussions/3420)

---

## 🏁 Заключение

Публичная дорожная карта — важный инструмент взаимодействия с пользователями, особенно в open-source. Начните с простого (`GitHub Projects` + `roadmap.md`) и масштабируйте по мере роста аудитории.

---




1. 📘 Пошаговую инструкцию по созданию **доски Roadmap** в GitHub Projects.
2. 🧾 Сгенерированную структуру `roadmap.md` (документация).
3. 🏷 Шаблон issue с меткой `roadmap` для предложений от сообщества.

---


# 🛠 Как создать доску Roadmap в GitHub Projects

## 📍 Шаги:

1. Перейди в свой репозиторий на GitHub.
2. Нажми вкладку **Projects** (если не видишь — включи Projects в настройках репозитория).
3. Нажми **"New project"** → выбери **"Table"** или **"Board"** (для Kanban-доски).
4. Назови проект `Roadmap` или `📌 Roadmap`.
5. Добавь колонки:
   - `Backlog` – идеи и неактивные задачи
   - `In Progress` – текущие задачи
   - `Planned` – задачи, запланированные на будущее
   - `Released` или `Done` – завершённые задачи
6. Добавляй задачи через `Issues` или вручную.
7. Привязывай milestone/релизы и метки (labels), например: `enhancement`, `bug`, `roadmap`, `v0.1.0`, и т.д.

📎 GitHub Projects поддерживает **фильтры, виджеты и автоматизации**, которые можно активировать позже.

---

# 📄 Пример начальной структуры `roadmap.md`

```md
# 📌 Дорожная карта UI-библиотеки

> Актуальная Roadmap проекта: [GitHub Projects](https://github.com/your-org/your-repo/projects/1)

## ✅ Версия v0.1.0 (MVP)

- [x] Настроить сборку (Vite + TS)
- [x] Добавить базовые компоненты: `Button`, `Input`
- [x] Базовая документация и Storybook
- [x] Публикация первого релиза в npm

## 🏗 Версия v0.2.0 (дизайн-система)

- [ ] Темизация (CSS-переменные + кастомизация)
- [ ] Адаптивные компоненты (Container, Grid)
- [ ] Поддержка анимаций (Framer Motion)
- [ ] Тестирование (Jest + RTL)

## 🚀 Будущее (v0.3+)

- [ ] Поддержка Vue 3
- [ ] Playground с Sandpack
- [ ] Интеграция Tailwind + Tokens
- [ ] Поддержка Web Components
- [ ] Формы и валидация

## 💡 Предложения

Вы можете предложить идею, создав `issue` с меткой `enhancement` или `roadmap`.

👉 [Создать предложение](https://github.com/your-org/your-repo/issues/new?labels=roadmap,enhancement&template=roadmap-suggestion.yml)
````

---

# 🧩 Шаблон для задач с roadmap-метками (issue template)

Создай файл:
`.github/ISSUE_TEMPLATE/roadmap-suggestion.yml`

```yaml
name: 💡 Предложение по Roadmap
description: Поделитесь идеей или предложением по улучшению библиотеки
title: "[Roadmap]: "
labels: ["roadmap", "enhancement"]
body:
  - type: input
    id: summary
    attributes:
      label: Краткое описание
      placeholder: Например, поддержка Vue 3 или анимации
    validations:
      required: true

  - type: textarea
    id: motivation
    attributes:
      label: Зачем это нужно?
      description: Объясните, какую проблему решает это улучшение
    validations:
      required: true

  - type: textarea
    id: proposal
    attributes:
      label: Как вы предлагаете это реализовать?
      description: Поделитесь идеей, ссылками, примерами

  - type: checkboxes
    id: willing
    attributes:
      label: Хотите помочь с реализацией?
      options:
        - label: Да, могу предложить Pull Request
        - label: Нет, просто идея
```

---

📌 **Важно**: не забудь активировать шаблоны issue в GitHub → Settings → Features → Issues → Templates.

---
