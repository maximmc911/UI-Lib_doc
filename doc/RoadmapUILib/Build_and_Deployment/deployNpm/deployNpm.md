


[назад](../Build_and_Deployment.md)


Вот подробная документация в формате Markdown на тему **Публикация библиотеки в npm**, включая настройку `package.json`, авторизацию, сборку, версионирование и рекомендации по публикации:

---

# 📦 Публикация UI-библиотеки в npm

## 🚀 Цель

На этом этапе мы публикуем библиотеку компонентов в публичный реестр **npm**, чтобы её могли использовать другие разработчики.

---

## 🔧 Шаг 1: Подготовка `package.json`

Минимальный пример:

```json
{
  "name": "@your-scope/ui-library",
  "version": "1.0.0",
  "description": "Современная UI-библиотека для React",
  "main": "dist/index.js",
  "module": "dist/index.js",
  "types": "dist/index.d.ts",
  "files": [
    "dist",
    "README.md",
    "LICENSE"
  ],
  "scripts": {
    "build": "tsc && rollup -c",
    "prepare": "npm run build"
  },
  "keywords": ["ui", "react", "components", "design-system"],
  "author": "Твое Имя <you@example.com>",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/your-name/ui-library"
  },
  "publishConfig": {
    "access": "public"
  },
  "peerDependencies": {
    "react": "^18.0.0",
    "react-dom": "^18.0.0"
  }
}
````

> 🛠 **peerDependencies** — используйте их для указания зависимостей, которые не должны быть включены в сборку.

---

## 🔐 Шаг 2: Авторизация в npm

1. Зарегистрируйтесь на [https://www.npmjs.com/](https://www.npmjs.com/)
2. Выполните вход в терминале:

```bash
npm login
```

Укажите:

* Имя пользователя
* Пароль
* Email

После этого в вашем локальном окружении будет создан файл `~/.npmrc`.

---

## 🏗 Шаг 3: Сборка проекта

Убедитесь, что у вас есть готовый `build`-процесс:

```bash
npm run build
```

Проверьте, что в папке `dist/` находятся:

* Скомпилированные `.js/.jsx` или `.ts/.tsx` файлы
* `.d.ts` декларации
* `styles.css` (если есть)
* index-файл, экспортирующий все компоненты

---

## 🗃 Структура проекта перед публикацией

```
my-ui-lib/
├── dist/
│   ├── index.js
│   ├── index.d.ts
│   └── Button/
├── src/
├── package.json
├── README.md
└── LICENSE
```

---

## 📤 Шаг 4: Публикация

Если вы публикуете **впервые**, добавьте:

```bash
npm publish --access public
```

В следующий раз просто:

```bash
npm publish
```

---

## 📌 Шаг 5: Версионирование

Следуйте [SemVer](https://semver.org/lang/ru/):

* `1.0.1` — патч (исправления)
* `1.1.0` — минор (новые возможности без поломок)
* `2.0.0` — мажор (с ломающими изменениями)

Обновить версию можно так:

```bash
npm version patch   # или minor / major
```

> После этого автоматически создается коммит и Git-тег.

---

## ⛔️ Исключения: `.npmignore` или `"files"`

Если хотите исключить из публикации ненужные файлы, используйте один из вариантов:

1. `.npmignore` — работает аналогично `.gitignore`
2. `"files"` в `package.json` — указывает, что именно нужно включить

---

## 📚 Пример использования после публикации

```bash
npm install @your-scope/ui-library
```

```tsx
import { Button } from '@your-scope/ui-library';

function App() {
  return <Button label="Click me" />;
}
```

---

## ✅ Чек-лист перед публикацией

| Шаг                                  | Статус |
| ------------------------------------ | ------ |
| `package.json` настроен              | ✅      |
| Типы `.d.ts` сгенерированы           | ✅      |
| GitHub-репозиторий указан            | ✅      |
| `README.md` и `LICENSE` присутствуют | ✅      |
| `build` успешно проходит             | ✅      |
| Вошли в аккаунт npm                  | ✅      |
| Версия обновлена                     | ✅      |

---

## 🛠 Советы

* Используйте **scoped packages** (`@your-org/package`) для группировки
* Настройте **CI/CD** для автоматической публикации
* Добавьте **GitHub Actions** для проверки сборки перед публикацией

---

## 🔄 Альтернатива: автоматизация с GitHub Actions

Пример `.github/workflows/publish.yml`:

```yaml
name: Publish to npm

on:
  push:
    tags:
      - 'v*.*.*'

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Publish
        run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

---

## 📚 Дополнительные материалы

* [Документация npm](https://docs.npmjs.com/)
* [npm version](https://docs.npmjs.com/cli/v9/commands/npm-version)
* [GitHub Actions: npm publish](https://docs.github.com/en/actions)

---

Готово! Вы успешно опубликовали свою UI-библиотеку и открыли её для всего сообщества.

