

[назад](../Component_Development.md)


# 📦 3.3 Хуки и Утилиты в UI-библиотеке

В современных UI-библиотеках большое значение имеют **повторно используемые хуки (hooks)** и **утилиты (utilities)**. Они помогают минимизировать дублирование кода, улучшить читаемость и ускорить разработку компонентов.

Ниже представлена подробная документация по ключевым хукам и утилитам, которые будут включены в базовое ядро библиотеки.

---

## 🔁 Хуки (React Hooks)

### 1. `useToggle`

**Назначение:** Управление булевыми состояниями — например, открытие и закрытие модальных окон, переключатели, чекбоксы.

**Пример:**

```tsx
const [isOpen, toggle] = useToggle();

return (
  <>
    <button onClick={toggle}>Открыть модалку</button>
    {isOpen && <Modal />}
  </>
);
````

**Реализация:**

```ts
function useToggle(initial = false): [boolean, () => void] {
  const [state, setState] = useState(initial);
  const toggle = () => setState(prev => !prev);
  return [state, toggle];
}
```

---

### 2. `useOutsideClick`

**Назначение:** Отслеживание кликов вне определённого DOM-элемента (например, закрытие попапов и дропдаунов).

**Пример:**

```tsx
const ref = useRef(null);
useOutsideClick(ref, () => setOpen(false));

return <div ref={ref}>Контент</div>;
```

**Реализация:**

```ts
function useOutsideClick(ref: RefObject<HTMLElement>, callback: () => void) {
  useEffect(() => {
    function handleClick(event: MouseEvent) {
      if (!ref.current || ref.current.contains(event.target as Node)) return;
      callback();
    }

    document.addEventListener('mousedown', handleClick);
    return () => document.removeEventListener('mousedown', handleClick);
  }, [ref, callback]);
}
```

---

### 3. `useClipboard`

**Назначение:** Копирование текста в буфер обмена и отслеживание состояния скопировано/не скопировано.

**Пример:**

```tsx
const [copied, copyToClipboard] = useClipboard();

<button onClick={() => copyToClipboard("Hello")}>Copy</button>
{copied && <p>Скопировано!</p>}
```

**Реализация:**

```ts
function useClipboard(timeout = 2000): [boolean, (text: string) => void] {
  const [copied, setCopied] = useState(false);

  const copy = (text: string) => {
    navigator.clipboard.writeText(text).then(() => {
      setCopied(true);
      setTimeout(() => setCopied(false), timeout);
    });
  };

  return [copied, copy];
}
```

---

## 🧰 Утилиты (Utilities)

### 1. `cn` — Класс-комбайнер (classNames)

**Назначение:** Удобное объединение строк классов, особенно с условиями. Является аналогом `clsx` или `classnames`.

**Пример:**

```tsx
<div className={cn('btn', isActive && 'btn--active', isDisabled && 'btn--disabled')} />
```

**Реализация:**

```ts
function cn(...args: (string | boolean | null | undefined)[]): string {
  return args.filter(Boolean).join(' ');
}
```

---

### 2. `debounce` — Задержка вызова

**Назначение:** Предотвращает слишком частое выполнение функции. Полезен при вводе текста, поиске, фильтрации и т.п.

**Пример:**

```ts
const debouncedSearch = debounce((query) => {
  fetchData(query);
}, 300);

<input onChange={(e) => debouncedSearch(e.target.value)} />
```

**Реализация:**

```ts
function debounce<T extends (...args: any[]) => void>(fn: T, delay: number): T {
  let timer: ReturnType<typeof setTimeout>;
  return function (...args: any[]) {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  } as T;
}
```

---

### 3. `throttle` — Ограничение частоты вызова

**Назначение:** Позволяет вызывать функцию не чаще указанного интервала. Часто используется при scroll/resize.

**Пример:**

```ts
const throttledScroll = throttle(() => {
  console.log('Scroll!');
}, 1000);

window.addEventListener('scroll', throttledScroll);
```

**Реализация:**

```ts
function throttle<T extends (...args: any[]) => void>(fn: T, limit: number): T {
  let inThrottle = false;
  return function (...args: any[]) {
    if (!inThrottle) {
      fn(...args);
      inThrottle = true;
      setTimeout(() => (inThrottle = false), limit);
    }
  } as T;
}
```

---

## 🧾 Сравнительная таблица

| Название          | Тип     | Назначение                               | Где использовать                    |
| ----------------- | ------- | ---------------------------------------- | ----------------------------------- |
| `useToggle`       | Хук     | Управление булевым состоянием            | Модалки, чекбоксы, попапы           |
| `useOutsideClick` | Хук     | Закрытие при клике вне элемента          | Dropdown, Popover, Modal            |
| `useClipboard`    | Хук     | Копирование текста и отображение статуса | Кнопки "Copy", уведомления          |
| `cn`              | Утилита | Комбинирование CSS-классов               | Все компоненты                      |
| `debounce`        | Утилита | Задержка выполнения функции              | Поиск, инпут, фильтры               |
| `throttle`        | Утилита | Ограничение частоты вызова функции       | Scroll, resize, обработчики событий |

---

## ✅ Заключение

Эти хуки и утилиты будут доступны из коробки в библиотеке и применимы во всех компонентах. Они значительно упростят взаимодействие с интерфейсами и обеспечат хорошую практику повторного использования логики.

> ✨ В будущем возможно добавление дополнительных хуков и утилит (например, `useMediaQuery`, `usePrefersColorScheme`, `usePortal`, `mergeRefs` и др.).

