

[назад](../Build_and_Deployment.md)

Вот подробная документация в формате Markdown по теме **Сборка UI-библиотеки** с использованием **Rollup** и **Vite**, включая Tree-shaking, ESM/CJS, CSS extraction и примеры конфигураций.

---


# ⚙️ Сборка UI-библиотеки: Rollup и Vite

Эффективная сборка — это фундамент UI-библиотеки, который обеспечивает:

- поддержку различных форматов (ESM, CJS, UMD),
- минимальный размер сборки,
- поддержку Tree-shaking,
- извлечение стилей,
- совместимость с современными экосистемами.

---

## 🧰 Почему важна кастомная сборка?

| Цель                          | Значение для библиотеки                   |
|------------------------------|-------------------------------------------|
| 📦 Поддержка форматов        | Подключение в любых проектах              |
| 🌲 Tree-shaking              | Удаление неиспользуемого кода             |
| 🎨 Изоляция CSS              | Минимизация конфликтов и лишнего веса     |
| ⚙️ ESM/CJS                   | Универсальность для node и браузера       |
| 🚀 Скорость                  | Быстрая сборка и публикация               |

---

## 🛠 Выбор сборщика

| Сборщик | Подходит для UI-библиотек | Tree-shaking | Поддержка ESM/CJS | Встроенный CSS extraction | Простота настройки |
|--------|----------------------------|--------------|-------------------|----------------------------|--------------------|
| **Rollup** | ✅ Да                    | ✅ Да        | ✅ Да             | ✅ Да (`rollup-plugin-postcss`) | ⭐⭐                 |
| **Vite**   | ✅ Частично             | ✅ Да        | ✅ Да             | ⚠️ Частично (больше для приложений) | ⭐⭐⭐⭐             |

> 💡 Рекомендуется использовать **Rollup** как основной бандлер для библиотек, а **Vite** — на этапе разработки или для демо/Playground-проекта.

---

## 🚀 Rollup: Конфигурация для UI-библиотеки

### 📦 Установка

```bash
npm install --save-dev rollup rollup-plugin-peer-deps-external \
rollup-plugin-postcss rollup-plugin-terser rollup-plugin-node-resolve \
rollup-plugin-commonjs rollup-plugin-typescript2 typescript
````

### 📄 `rollup.config.js`

```js
import resolve from 'rollup-plugin-node-resolve';
import commonjs from 'rollup-plugin-commonjs';
import external from 'rollup-plugin-peer-deps-external';
import postcss from 'rollup-plugin-postcss';
import typescript from 'rollup-plugin-typescript2';
import { terser } from 'rollup-plugin-terser';

import pkg from './package.json';

export default {
  input: 'src/index.ts',
  output: [
    {
      file: pkg.main,
      format: 'cjs',
      sourcemap: true
    },
    {
      file: pkg.module,
      format: 'esm',
      sourcemap: true
    }
  ],
  plugins: [
    external(),
    resolve(),
    commonjs(),
    typescript({
      useTsconfigDeclarationDir: true
    }),
    postcss({
      extract: true,
      minimize: true,
      modules: true
    }),
    terser()
  ]
};
```

### 📄 `package.json` (часть)

```json
{
  "main": "dist/index.cjs.js",
  "module": "dist/index.esm.js",
  "types": "dist/index.d.ts",
  "scripts": {
    "build": "rollup -c"
  }
}
```

---

## ⚡ Vite: Сборка библиотеки

> Vite идеально подходит для Playground и разработки компонентов.

### 📦 Установка

```bash
npm install --save-dev vite
```

### 📄 `vite.config.ts`

```ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  build: {
    lib: {
      entry: path.resolve(__dirname, 'src/index.ts'),
      name: 'MyUILibrary',
      fileName: (format) => `my-ui-library.${format}.js`,
    },
    rollupOptions: {
      external: ['react', 'react-dom'],
      output: {
        globals: {
          react: 'React',
          'react-dom': 'ReactDOM'
        }
      }
    }
  }
});
```

> ⚠️ Vite не поддерживает `postcss`-extract по умолчанию в режиме сборки библиотеки. Можно использовать `vite-plugin-css-injected-by-js`, либо собрать стили отдельно через PostCSS.

---

## 📂 CSS extraction

Если вы используете CSS-модули или PostCSS:

```bash
npm install --save-dev postcss postcss-cli autoprefixer
```

### 📄 `postcss.config.js`

```js
module.exports = {
  plugins: {
    autoprefixer: {}
  }
};
```

Запуск:

```bash
npx postcss src/styles.css -o dist/styles.css
```

---

## 🌲 Tree-shaking

Tree-shaking работает при выполнении трёх условий:

1. Использование **ESM** (`import`/`export`).
2. Правильная настройка `sideEffects: false` в `package.json`.
3. Отсутствие нежелательных глобальных эффектов при импорте.

### 📄 `package.json`

```json
{
  "sideEffects": false
}
```

---

## 🔍 Примеры: `src/index.ts`

```ts
export { Button } from './components/Button';
export { Input } from './components/Input';
```

---

## 🧪 Проверка результатов сборки

* **dist/index.esm.js** — для современных проектов
* **dist/index.cjs.js** — для Node.js и старых бандлеров
* **dist/index.d.ts** — типы для TypeScript
* **dist/style.css** — стили, подключаемые вручную или через CDN

---

## 📌 Рекомендации

| Подход          | Когда использовать                   |
| --------------- | ------------------------------------ |
| **Rollup**      | Производственная сборка библиотеки   |
| **Vite**        | Разработка и playground-документация |
| **PostCSS CLI** | Отдельная сборка стилей              |
| **TypeScript**  | Для типизированного API              |

---

## ✅ Итоговая структура проекта

```
my-ui-library/
├── src/
│   ├── components/
│   ├── index.ts
│   └── styles.css
├── dist/
│   ├── index.cjs.js
│   ├── index.esm.js
│   ├── index.d.ts
│   └── styles.css
├── vite.config.ts
├── rollup.config.js
├── postcss.config.js
└── package.json
```

---

## 📚 См. также

* [Rollup Docs](https://rollupjs.org/)
* [Vite Build Libraries](https://vitejs.dev/guide/build.html#library-mode)
* [PostCSS](https://postcss.org/)

