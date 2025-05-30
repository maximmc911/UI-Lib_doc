

[назад](../Demo_and_Website.md)



# 🔍 Реализация фильтров и поисковиков на демонстрационном сайте

## 🧭 Цель

Добавление полнотекстового поиска и системы фильтрации компонентов/страниц документации позволит:

- Повысить **удобство навигации**
- Ускорить **доступ к нужной информации**
- Повысить **качество пользовательского опыта**

---

## 🛠 Основные функции

### 🔎 Поиск по документации
- Поиск по заголовкам, описаниям, props компонентов
- Подсветка совпадений
- Поддержка латиницы и кириллицы
- Возможность кастомизации результата (например, иконки, категория)

### 🧰 Фильтры
- Фильтрация по категориям: `Компоненты`, `Hooks`, `Утилиты`, `Гайды`
- Фильтрация по доступности (`доступен в React`, `доступен в Vue`)
- Фильтрация по дате добавления или популярности

---

## 📚 Рекомендуемые библиотеки

| Библиотека                | Назначение                         | Преимущества                                     |
|---------------------------|------------------------------------|--------------------------------------------------|
| **Algolia / DocSearch**   | Полнотекстовый поиск               | Мощный поиск, уже используется во многих UI-библиотеках |
| **FlexSearch**            | Кастомный локальный поиск          | Легковесный, работает без сервера                |
| **Fuse.js**               | Fuzzy-поиск                        | Простая настройка, работает локально            |
| **lunr.js**               | Поиск с индексом                   | Поддержка токенизации, похож на Elasticsearch   |
| **React InstantSearch**   | React-обёртка для Algolia          | Поддержка фильтров и кастомной визуализации     |

---

## 🔧 Пример реализации с `Fuse.js`

### 📦 Установка

```bash
npm install fuse.js
````

### 🧩 Пример кода

```tsx
import Fuse from 'fuse.js';
import { useState } from 'react';

const componentsList = [
  { name: 'Button', description: 'Универсальная кнопка', tags: ['ui', 'input'] },
  { name: 'Modal', description: 'Модальное окно', tags: ['overlay', 'dialog'] },
];

const fuse = new Fuse(componentsList, {
  keys: ['name', 'description', 'tags'],
  threshold: 0.3, // насколько нечеткий поиск
});

export default function Search() {
  const [query, setQuery] = useState('');
  const results = query ? fuse.search(query).map(result => result.item) : componentsList;

  return (
    <>
      <input
        type="text"
        placeholder="Поиск компонентов..."
        onChange={(e) => setQuery(e.target.value)}
        className="search-input"
      />
      <ul>
        {results.map((component) => (
          <li key={component.name}>{component.name}</li>
        ))}
      </ul>
    </>
  );
}
```

---

## 🧪 Фильтрация компонентов по категории

```tsx
const filters = ['Все', 'Компоненты', 'Hooks', 'Утилиты'];
const [activeFilter, setActiveFilter] = useState('Все');

const filtered = componentsList.filter((item) =>
  activeFilter === 'Все' ? true : item.category === activeFilter
);
```

---

## ⚙ Интеграция с документационными системами

| Система              | Поиск                   | Фильтры             | Комментарии                 |
| -------------------- | ----------------------- | ------------------- | --------------------------- |
| **Docusaurus**       | Algolia (встроен)       | Есть                | Очень гибкая система        |
| **Storybook**        | Плагин DocSearch        | Можно кастомно      | Нужна настройка             |
| **Next.js Site**     | Любая                   | Любые               | Полный контроль через React |
| **Custom React SPA** | Любые локальные решения | Полная кастомизация | Отлично подходит для MVP    |

---

## ✅ Плюсы и минусы различных подходов

| Подход                   | Плюсы                                 | Минусы                                  |
| ------------------------ | ------------------------------------- | --------------------------------------- |
| `Algolia` + DocSearch    | Мгновенный поиск, облачная индексация | Требуется регистрация и API-ключи       |
| `Fuse.js` / `FlexSearch` | Работает локально, просто настроить   | Может быть медленным на большом объёме  |
| Фильтрация вручную       | Полный контроль, просто реализовать   | Нет ранжирования, не работает как поиск |

---

## 💡 Рекомендации

* Для MVP и SPA-документации → `Fuse.js` или `FlexSearch`
* Для масштабируемого проекта с большим числом компонентов → `Algolia` + `DocSearch`
* Использовать отдельную секцию фильтров в левой панели документации
* Реализовать адаптивный поиск для мобильных

---

## 📌 Дополнительные идеи

* **История поиска**
* **Группировка результатов**
* **Скоринговая система по частоте использования**
* **Интеграция с `localStorage` для сохранения состояния поиска/фильтра**

---
Вот дополнение к документации: подробное описание **дополнительных идей** для улучшения поисковой и фильтрационной системы демонстрационного сайта. Оформлено в формате Markdown.

---

````md
## 💡 Дополнительные идеи для улучшения поиска и фильтрации

Эти функции позволяют значительно расширить возможности поиска и навигации, повышая удобство для пользователей, особенно в случае роста библиотеки компонентов.

---

### 🔁 История поиска

**Описание:**
Сохранение последних (или часто используемых) поисковых запросов пользователя. При повторном заходе можно быстро выбрать нужный запрос.

**Как реализовать:**
- Хранение последних N запросов в `localStorage`
- Автозаполнение при фокусе на поле поиска
- Возможность очистить историю

**Пример:**

```tsx
const [history, setHistory] = useState(
  JSON.parse(localStorage.getItem('searchHistory')) || []
);

const updateHistory = (query) => {
  const updated = [query, ...history.filter((q) => q !== query)].slice(0, 5);
  setHistory(updated);
  localStorage.setItem('searchHistory', JSON.stringify(updated));
};
````

---

### 🧩 Группировка результатов

**Описание:**
Позволяет структурировать результаты по категориям: например, «Компоненты», «Hooks», «Утилиты», «Гайды».

**Преимущества:**

* Упрощает визуальный скан списка
* Быстрое определение типа искомого элемента

**Пример:**

```tsx
const groupedResults = results.reduce((acc, item) => {
  const group = item.category || 'Другое';
  acc[group] = acc[group] || [];
  acc[group].push(item);
  return acc;
}, {});
```

---

### 📈 Скоринговая система по частоте использования

**Описание:**
Ранжирование результатов поиска по частоте взаимодействия (количество кликов по компоненту).

**Преимущества:**

* Наиболее популярные компоненты отображаются первыми
* Возможность анализа интереса пользователей к API

**Как реализовать:**

* Хранение счётчиков в `localStorage`, `IndexedDB`, или бэкенде
* Инкремент при клике на компонент

**Пример:**

```tsx
const incrementUsage = (id) => {
  const usage = JSON.parse(localStorage.getItem('usageStats')) || {};
  usage[id] = (usage[id] || 0) + 1;
  localStorage.setItem('usageStats', JSON.stringify(usage));
};

const sortByUsage = (results) => {
  const usage = JSON.parse(localStorage.getItem('usageStats')) || {};
  return results.sort((a, b) => (usage[b.id] || 0) - (usage[a.id] || 0));
};
```

---

### 🗂 Интеграция с `localStorage` для сохранения состояния

**Описание:**
Сохранение состояния фильтров, языка, темы или сортировки между сессиями.

**Преимущества:**

* Улучшенный UX
* Не нужно повторно выбирать фильтры при возврате

**Пример:**

```tsx
const [filters, setFilters] = useState(
  JSON.parse(localStorage.getItem('filters')) || defaultFilters
);

useEffect(() => {
  localStorage.setItem('filters', JSON.stringify(filters));
}, [filters]);
```

---

### 🎯 Потенциальные улучшения

* Поддержка **"похожих результатов"** при ошибках ввода (e.g. "btn" → "Button")
* Использование **веса полей** для поиска (название важнее описания)
* Интеграция с **командной строкой** для Power User’ов (через Cmd+K / Ctrl+K)

---

## ✅ Вывод

Интеграция этих улучшений позволяет создать максимально **интерактивный**, **умный** и **удобный** интерфейс поиска и фильтрации, подходящий как для MVP, так и для крупной документации, развивающейся вместе с UI-библиотекой.

```
```
