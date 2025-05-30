

[назад](../Support_and_Development.md)


# 💬 Обратная связь через GitHub Issues и Discussions

Организация качественной обратной связи — ключевой аспект поддержки и развития open-source UI-библиотеки. GitHub предоставляет два основных механизма для взаимодействия с сообществом:

- `Issues` — для трекинга багов, фич, улучшений.
- `Discussions` — для идей, вопросов, архитектурных решений и предложений.

---

## 🐛 GitHub Issues — для задач

### Что такое Issues?

`Issues` — это задачи или проблемы, требующие внимания. Они могут быть:

- Ошибками (`bug`)
- Новыми функциями (`feature`)
- Улучшениями (`enhancement`)
- Техническими вопросами (`question`)
- Документационными проблемами (`docs`)

---

### Пример шаблона `bug_report.yml`

Файл: `.github/ISSUE_TEMPLATE/bug_report.yml`

```yaml
name: 🐞 Баг
description: Сообщите об ошибке, которую вы обнаружили
title: "[Bug] - "
labels: ["bug"]
body:
  - type: input
    attributes:
      label: Версия библиотеки
      placeholder: например, 0.5.2
  - type: textarea
    attributes:
      label: Описание ошибки
      description: Что пошло не так? Что ожидалось?
  - type: textarea
    attributes:
      label: Как воспроизвести
      placeholder: 1. Импортировать компонент...\n2. Использовать его в...\n3. Ошибка появляется в консоли
  - type: input
    attributes:
      label: Ссылка на код или песочницу
      placeholder: https://stackblitz.com/edit/example
  - type: dropdown
    attributes:
      label: Браузер
      options:
        - Chrome
        - Firefox
        - Safari
        - Edge
````

---

### Пример шаблона `feature_request.yml`

Файл: `.github/ISSUE_TEMPLATE/feature_request.yml`

```yaml
name: 💡 Новая фича
description: Предложите полезную функцию
title: "[Feature] - "
labels: ["enhancement"]
body:
  - type: textarea
    attributes:
      label: Описание фичи
      placeholder: Расскажите, что вы хотите добавить и зачем
  - type: textarea
    attributes:
      label: Проблема, которую решает
  - type: textarea
    attributes:
      label: Альтернативы
  - type: textarea
    attributes:
      label: Дополнительные материалы
```

---

### Метки (labels)

Используйте метки (`labels`) для быстрой навигации:

| Метка              | Назначение               |
| ------------------ | ------------------------ |
| `bug`              | Сообщения об ошибках     |
| `enhancement`      | Улучшения                |
| `docs`             | Проблемы с документацией |
| `question`         | Вопросы                  |
| `good first issue` | Задачи для новичков      |

---

### Пример Issue

```md
**Заголовок**: [Bug] Компонент Button не работает в Safari

**Описание**
Кнопка не реагирует на клик в Safari 15.3

**Шаги**
1. Импортировать `Button`
2. Вставить в проект
3. Открыть в Safari

**Ожидаемое поведение**
Кнопка должна вызывать событие onClick

**Среда**
- Safari 15.3
- macOS Ventura
- UI-библиотека v0.4.1
```

---

## 💬 GitHub Discussions — для общения

### Что это?

`Discussions` — это площадка для общения сообщества: вопросы, идеи, предложения, отзывы, сбор фидбека.

Они **не попадают** в список задач и не засоряют backlog.

---

### Категории для раздела Discussions

| Категория         | Назначение                            |
| ----------------- | ------------------------------------- |
| ❓ Вопросы         | Помощь по использованию библиотеки    |
| 💡 Идеи           | Предложения новых компонентов, фич    |
| 💬 Обсуждения     | Архитектура, подходы к API            |
| 📝 Обратная связь | Мнения о качестве, UX, DX             |
| 🔗 Ресурсы        | Проекты, шаблоны, связанные материалы |

---

### Пример обсуждения

```md
**Заголовок:** 💡 Поддержка Tailwind токенов

**Описание**
Хотелось бы, чтобы все компоненты поддерживали переменные из Tailwind config.

**Почему это важно**
Это упростит кастомизацию под существующие дизайн-системы.

**Обсуждение**
- Как можно это реализовать?
- Есть ли у кого-то опыт?
- Какие компоненты нужно адаптировать в первую очередь?
```

---

## 🛠 Настройка репозитория

### Структура `.github`

```bash
.github/
├── ISSUE_TEMPLATE/
│   ├── bug_report.yml
│   ├── feature_request.yml
├── DISCUSSION_TEMPLATE/  # если используется
├── workflows/
│   └── stale.yml          # автозакрытие старых задач
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
└── SECURITY.md
```

---

### Советы по управлению

* Используйте **GitHub Projects** для управления задачами
* Настройте **Stale Bot**:

  * Закрытие неактивных задач через 30–60 дней
  * Напоминания о действиях от автора
* Используйте **Templates** для единого стиля

---

## 🔁 Когда использовать что?

| Задача                       | Где обсуждать              |
| ---------------------------- | -------------------------- |
| Сообщение об ошибке          | `Issues`                   |
| Предложение новой функции    | `Issues` или `Discussions` |
| Вопрос по использованию      | `Discussions`              |
| Архитектурные решения        | `Discussions`              |
| Дискуссии по дизайну API     | `Discussions`              |
| Обратная связь пользователей | `Discussions`              |

---

## 🎯 Заключение

Грамотно организованная обратная связь:

* Повышает доверие к проекту
* Ускоряет развитие
* Привлекает вкладчиков
* Делает разработку более предсказуемой и устойчивой

Если вы только начинаете — достаточно одной категории Issues и включённых Discussions. Со временем можно развивать систему, добавлять автоматизацию и ботов.

---

## 🔗 Полезные ссылки

* [📘 GitHub Discussions Docs](https://docs.github.com/en/discussions)
* [📘 GitHub Issue Templates](https://docs.github.com/en/issues/using-labels-and-milestones-to-track-work/about-issue-and-pull-request-templates)
* [🤖 Stale GitHub Action](https://github.com/actions/stale)

---
