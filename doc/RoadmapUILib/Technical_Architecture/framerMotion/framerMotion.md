

[назад](../Technical_Architecture.md)




# 🎞 Поддержка анимаций с Framer Motion

Framer Motion — современная и мощная библиотека для создания анимаций в React. Она предоставляет простой API для анимирования компонентов, поддерживает жесты, сложные переходы и позволяет создавать интерактивные UI.

---

## 📦 Установка

```bash
npm install framer-motion
# или
yarn add framer-motion
````

---

## 🚀 Основные концепции

* **motion components** — обёртка над HTML-элементами, которые можно анимировать.
* **animate** — цель анимации, задаёт конечные стили.
* **initial** — начальные стили для анимации.
* **exit** — стили при выходе компонента (для анимаций при удалении).
* **variants** — наборы состояний анимации для упрощённого управления.
* **transition** — настройка параметров анимации (длительность, задержка, easing).

---

## 🔥 Пример базовой анимации

```tsx
import { motion } from "framer-motion";

export function FadeInBox() {
  return (
    <motion.div
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      transition={{ duration: 0.8 }}
      className="w-32 h-32 bg-blue-500"
    >
      Анимированный блок
    </motion.div>
  );
}
```

---

## 🎭 Использование Variants для управления анимацией

Variants позволяют описать несколько состояний и переключаться между ними.

```tsx
const boxVariants = {
  hidden: { opacity: 0, y: 20 },
  visible: { opacity: 1, y: 0 },
};

export function VariantBox() {
  return (
    <motion.div
      variants={boxVariants}
      initial="hidden"
      animate="visible"
      transition={{ duration: 0.5 }}
      className="w-40 h-40 bg-red-400"
    />
  );
}
```

---

## ↔️ Анимация при монтировании и размонтировании

Используйте `AnimatePresence` для анимации выхода компонента.

```tsx
import { AnimatePresence, motion } from "framer-motion";
import { useState } from "react";

export function ToggleBox() {
  const [isVisible, setVisible] = useState(true);

  return (
    <>
      <button onClick={() => setVisible(!isVisible)}>
        Переключить
      </button>
      <AnimatePresence>
        {isVisible && (
          <motion.div
            key="box"
            initial={{ opacity: 0, scale: 0.8 }}
            animate={{ opacity: 1, scale: 1 }}
            exit={{ opacity: 0, scale: 0.8 }}
            transition={{ duration: 0.4 }}
            className="w-32 h-32 bg-green-500 mt-4"
          />
        )}
      </AnimatePresence>
    </>
  );
}
```

---

## 🖱 Жесты и интерактивность

Framer Motion поддерживает события drag, hover и tap с возможностью анимации.

```tsx
<motion.div
  whileHover={{ scale: 1.1 }}
  whileTap={{ scale: 0.9 }}
  drag
  dragConstraints={{ left: 0, right: 100, top: 0, bottom: 100 }}
  className="w-24 h-24 bg-purple-600 rounded"
>
  Перетаскиваемый блок
</motion.div>
```

---

## ⚙️ Настройка Transition

```tsx
<motion.div
  animate={{ x: 100 }}
  transition={{
    type: "spring",
    stiffness: 100,
    damping: 10,
  }}
  className="w-20 h-20 bg-yellow-400"
/>
```

Основные параметры:

| Параметр               | Описание                         |
| ---------------------- | -------------------------------- |
| `duration`             | Длительность анимации (секунды)  |
| `delay`                | Задержка перед стартом анимации  |
| `ease`                 | Кривая плавности (ease-in/out)   |
| `type`                 | Тип анимации (`tween`, `spring`) |
| `stiffness`, `damping` | Параметры пружинной анимации     |

---

## 📁 Пример анимации списка элементов

```tsx
const listVariants = {
  hidden: { opacity: 0, y: 20 },
  visible: {
    opacity: 1,
    y: 0,
    transition: {
      staggerChildren: 0.15,
    },
  },
};

const itemVariants = {
  hidden: { opacity: 0, y: 10 },
  visible: { opacity: 1, y: 0 },
};

export function AnimatedList({ items }: { items: string[] }) {
  return (
    <motion.ul
      variants={listVariants}
      initial="hidden"
      animate="visible"
      className="space-y-2"
    >
      {items.map((item, i) => (
        <motion.li
          key={i}
          variants={itemVariants}
          className="p-2 bg-gray-200 rounded"
        >
          {item}
        </motion.li>
      ))}
    </motion.ul>
  );
}
```

---

## 🔗 Полезные ссылки

* [Официальная документация Framer Motion](https://www.framer.com/motion/)
* [API Reference](https://www.framer.com/api/motion/)
* [Примеры на CodeSandbox](https://codesandbox.io/examples/package/framer-motion)

---

## ✅ Резюме

* Framer Motion — мощная библиотека для создания плавных и интерактивных анимаций в React.
* Используйте `motion` компоненты для базовой анимации.
* `variants` упрощают управление несколькими состояниями.
* `AnimatePresence` позволяет анимировать удаление компонентов.
* Поддержка drag, hover, tap и кастомных переходов.
* Настраивайте анимации через `transition` для гибкости.

---

Если хочешь, могу помочь интегрировать Framer Motion в твои компоненты UI-библиотеки или сделать примеры для конкретных кейсов.

