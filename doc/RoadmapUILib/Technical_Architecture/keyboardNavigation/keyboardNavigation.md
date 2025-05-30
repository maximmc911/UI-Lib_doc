

[назад](../Technical_Architecture.md)



# ♿ Доступность (A11y) в UI-библиотеке  
## WAI-ARIA, фокус-ловушки и клавиатурная навигация

---

## Что такое доступность (Accessibility)?

Доступность — это практика создания интерфейсов, которые могут использовать все люди, включая людей с ограничениями по зрению, слуху, моторике и когнитивным способностям.

---

## 1. WAI-ARIA — что это и зачем?

**WAI-ARIA (Web Accessibility Initiative – Accessible Rich Internet Applications)** — это набор атрибутов, которые помогают описывать смысл и поведение интерактивных элементов для вспомогательных технологий (экранных читалок, зуммеров и др.).

- Улучшает восприятие сложных UI-компонентов.
- Обозначает роли (role), состояния (aria-*) и свойства.

---

### Основные ARIA-атрибуты

| Атрибут          | Описание                                          | Пример                                 |
|------------------|--------------------------------------------------|---------------------------------------|
| `role`           | Роль элемента (button, dialog, navigation и др.) | `<div role="button" tabindex="0">`    |
| `aria-label`     | Альтернативный текст для элемента                 | `<button aria-label="Закрыть">X</button>` |
| `aria-labelledby`| Связь с элементом, который описывает текущее     | `<div aria-labelledby="title1"></div>` |
| `aria-hidden`    | Скрыть элемент от вспомогательных технологий      | `<div aria-hidden="true"></div>`       |
| `aria-expanded`  | Состояние раскрытия (например, меню)               | `<button aria-expanded="false"></button>` |
| `aria-controls`  | Указывает, какой элемент контролируется            | `<button aria-controls="menu1"></button>` |

---

### Пример кнопки с ARIA

```tsx
<button
  role="button"
  aria-pressed={isPressed}
  aria-label="Включить режим"
  onClick={toggleMode}
>
  Режим
</button>
````

---

## 2. Фокус-ловушки (Focus Traps)

Фокус-ловушка — это техника, которая ограничивает фокус клавиатуры внутри определённого контейнера (например, модального окна), не позволяя пользователю случайно уйти из него при навигации с клавиатуры.

### Когда нужна?

* В модальных окнах
* В выпадающих меню
* В сайдбарах и диалогах

---

### Как реализовать?

1. Отслеживать нажатия Tab и Shift+Tab.
2. Перемещать фокус между первыми и последними фокусируемыми элементами внутри контейнера.
3. Блокировать выход за пределы контейнера.

---

### Пример простейшей focus trap на React

```tsx
import { useEffect, useRef } from "react";

export function FocusTrap({ children }: { children: React.ReactNode }) {
  const trapRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const focusableElementsString = `
      a[href], area[href], input:not([disabled]),
      select:not([disabled]), textarea:not([disabled]),
      button:not([disabled]), iframe, object, embed,
      *[tabindex]:not([tabindex="-1"]), *[contenteditable]
    `;

    const trap = trapRef.current;
    if (!trap) return;

    const focusableElements = Array.from(
      trap.querySelectorAll<HTMLElement>(focusableElementsString)
    ).filter(el => el.offsetWidth > 0 || el.offsetHeight > 0 || el === document.activeElement);

    const firstElement = focusableElements[0];
    const lastElement = focusableElements[focusableElements.length - 1];

    function handleKeyDown(e: KeyboardEvent) {
      if (e.key !== "Tab") return;

      if (e.shiftKey) {
        // Shift + Tab
        if (document.activeElement === firstElement) {
          e.preventDefault();
          lastElement.focus();
        }
      } else {
        // Tab
        if (document.activeElement === lastElement) {
          e.preventDefault();
          firstElement.focus();
        }
      }
    }

    trap.addEventListener("keydown", handleKeyDown);

    // Фокусируем первый элемент при монтировании
    firstElement?.focus();

    return () => {
      trap.removeEventListener("keydown", handleKeyDown);
    };
  }, []);

  return <div ref={trapRef}>{children}</div>;
}
```

---

## 3. Клавиатурная навигация

Правильная клавиатурная навигация позволяет пользователям, которые не используют мышь, полноценно взаимодействовать с сайтом.

---

### Основные правила

* Все интерактивные элементы должны быть доступны через Tab.
* Последовательность фокуса должна быть логичной.
* Используйте **`tabindex`**:

  * `tabindex="0"` — элемент доступен через Tab.
  * `tabindex="-1"` — элемент убирается из последовательности, но доступен программно.
* Используйте клавиши Enter и Space для активации кнопок и ссылок.
* Обеспечьте видимый фокус (outline) — не убирайте его без замены.

---

### Пример доступной кнопки и ссылка

```tsx
<button onClick={handleClick} tabIndex={0}>
  Нажми меня
</button>

<a href="/home" tabIndex={0}>
  Главная
</a>
```

---

### Управление фокусом в компонентах

* При открытии модального окна фокус должен переходить внутрь модалки.
* При закрытии — возвращаться на элемент, с которого открыли.
* При переключении вкладок или аккордеонов фокус должен обновляться соответственно.

---

## 4. Полезные библиотеки для A11y

* [React Aria](https://react-spectrum.adobe.com/react-aria/) — готовые доступные UI-хуки.
* [Focus Trap React](https://github.com/focus-trap/focus-trap-react) — реализация фокус-ловушек.
* [Reach UI](https://reach.tech/) — доступные UI-компоненты.

---

## 5. Итоговые рекомендации

* Всегда используйте семантические HTML-элементы.
* Добавляйте ARIA-атрибуты, если элемент нестандартный.
* Следите за последовательностью фокуса.
* Тестируйте клавиатурную навигацию.
* Используйте фокус-ловушки для модальных окон и сложных компонентов.
* Позаботьтесь о видимости фокуса.
* Пользуйтесь вспомогательными библиотеками для ускорения разработки.

---

## 6. Полезные ссылки

* [WAI-ARIA Overview](https://www.w3.org/WAI/standards-guidelines/aria/)
* [WebAIM Keyboard Accessibility](https://webaim.org/techniques/keyboard/)
* [MDN Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility)
* [A11y Project](https://www.a11yproject.com/)
* [Testing Keyboard Navigation](https://dequeuniversity.com/rules/axe/4.8/keyboard-focus-order?application=AxeChrome)

---

Если хочешь, могу помочь написать React-компоненты с поддержкой всех этих подходов или сделать примеры интеграции A11y в твою библиотеку.

