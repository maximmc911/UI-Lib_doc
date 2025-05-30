

[назад](../Technical_Architecture.md)



# 🛠 Возможности кастомизации компонентов

Наша UI-библиотека разработана с прицелом на **гибкость и переиспользуемость**. Любой компонент можно адаптировать под нужды конкретного проекта без необходимости его переписывать. Поддерживаются следующие уровни кастомизации:

- `className` — для переопределения стилей
- `slots` — для замены/вставки внутренних элементов
- `override props` — для передачи глубокой кастомизации

---

## 🎨 1. Кастомизация через `className`

Все компоненты принимают проп `className`, который добавляется к корневому элементу или к ключевым внутренним обёрткам.

### Пример:

```tsx
<Button className="bg-green-600 hover:bg-green-700 text-white font-bold">
  Кастомная кнопка
</Button>
````

Если компонент использует `clsx`, то внешние классы не перетирают внутренние, а дополняют их:

```tsx
export function Button({ className, ...props }) {
  return (
    <button className={clsx("px-4 py-2 rounded-md", className)} {...props} />
  );
}
```

---

## 🧩 2. Кастомизация через `slots`

**Слоты** позволяют изменять внутренние части компонента без полной перезаписи. Это удобно для кастомизации и расширения UI-элементов.

### Пример: Кастомный слот иконки

```tsx
<Card
  slots={{
    header: () => <div className="text-xl font-bold">Мой заголовок</div>,
    footer: () => <div className="text-right text-sm text-muted">Футер</div>,
  }}
>
  Основной контент
</Card>
```

### Типичный API:

```tsx
interface CardProps {
  slots?: {
    header?: React.ReactNode | (() => React.ReactNode);
    footer?: React.ReactNode | (() => React.ReactNode);
  };
}
```

### Внутри компонента:

```tsx
{slots?.header ? (
  typeof slots.header === 'function' ? slots.header() : slots.header
) : (
  <DefaultHeader />
)}
```

---

## ⚙️ 3. Кастомизация через override props

Библиотека позволяет передавать пропсы на внутренние элементы — это называется **override props**. Обычно реализуется через объект `components` или `slotsProps`.

### Пример:

```tsx
<Dialog
  slotsProps={{
    overlay: {
      className: 'bg-black/70 backdrop-blur',
    },
    content: {
      className: 'max-w-lg p-6',
    },
  }}
/>
```

### Интерфейс пропов:

```tsx
interface DialogProps {
  slotsProps?: {
    overlay?: React.HTMLAttributes<HTMLDivElement>;
    content?: React.HTMLAttributes<HTMLDivElement>;
  };
}
```

### Использование внутри:

```tsx
<div className={clsx("dialog-overlay", slotsProps?.overlay?.className)} />
<div className={clsx("dialog-content", slotsProps?.content?.className)} />
```

---

## 🧠 Комбинирование кастомизаций

```tsx
<Tooltip
  className="bg-blue-600 text-white"
  slots={{
    arrow: () => <span className="text-white">▲</span>,
  }}
  slotsProps={{
    content: { className: "p-3 shadow-xl" },
  }}
>
  Наведи на меня
</Tooltip>
```

---

## 📁 Структура типового компонента

```tsx
export function Component({
  className,
  slots,
  slotsProps,
  ...props
}: ComponentProps) {
  return (
    <div className={clsx("base-class", className)} {...props}>
      {slots?.icon ? slots.icon() : <DefaultIcon />}
      <div {...slotsProps?.content}>{props.children}</div>
    </div>
  );
}
```

---

## 🧪 Практический пример: Input

```tsx
<Input
  className="border-2 border-blue-500"
  slots={{
    leftIcon: () => <SearchIcon className="text-blue-500" />,
  }}
  slotsProps={{
    input: {
      className: 'px-4 py-2 focus:ring-2 focus:ring-blue-500',
    },
  }}
/>
```

---

## ✅ Резюме

| Подход           | Назначение                              | Когда использовать                          |
| ---------------- | --------------------------------------- | ------------------------------------------- |
| `className`      | Поверхностная стилизация                | Когда нужно быстро изменить внешний вид     |
| `slots`          | Замена или расширение внутренних блоков | Когда нужно внедрить свои компоненты        |
| `override props` | Передача props в конкретные элементы    | Когда нужно изменить поведение/стили внутри |

---

## 📚 Полезные паттерны

* Используй `clsx` для объединения классов
* Всегда делай `className` переопределяемым
* Делай `slots` опциональными, с `default`-контентом
* Поддерживай `slotsProps` для сложных компонентов (Modal, Dialog, Select)

---
