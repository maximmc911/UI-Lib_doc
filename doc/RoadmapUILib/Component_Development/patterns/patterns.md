

[назад](../Component_Development.md)


# Документация по паттернам разработки компонентов UI-библиотеки

---

## 1. Паттерны компонентов

Паттерны — это проверенные архитектурные решения, позволяющие создавать гибкие, переиспользуемые и удобные в поддержке UI-компоненты. Рассмотрим основные паттерны, которые используются в React и других современных фреймворках.

---

## 2. Слоты (Slots) / Render Props

### Что такое?

- **Слоты (Slots)** — это место в компоненте, куда можно "вставить" произвольный JSX-код из родительского компонента.
- **Render Props** — паттерн, когда дочерний компонент получает функцию (render prop), которая возвращает React-элемент для рендера.

### Чем полезны?

- Позволяют создавать очень гибкие компоненты с настраиваемым внутренним содержимым.
- Избавляют от жёсткой иерархии и жёсткого API, расширяют возможности кастомизации.

### Пример слотов (React)

```jsx
function Modal({ header, footer, children }) {
  return (
    <div className="modal">
      <div className="modal-header">{header}</div>
      <div className="modal-content">{children}</div>
      <div className="modal-footer">{footer}</div>
    </div>
  );
}

// Использование
<Modal
  header={<h2>Заголовок</h2>}
  footer={<button>Закрыть</button>}
>
  <p>Основное содержимое модального окна</p>
</Modal>
````

### Пример Render Props

```jsx
function MouseTracker({ render }) {
  const [pos, setPos] = React.useState({ x: 0, y: 0 });

  function handleMouseMove(event) {
    setPos({ x: event.clientX, y: event.clientY });
  }

  return (
    <div onMouseMove={handleMouseMove}>
      {render(pos)}
    </div>
  );
}

// Использование
<MouseTracker render={({ x, y }) => (
  <p>Координаты мыши: {x}, {y}</p>
)} />
```

---

## 3. Compound Components (Составные компоненты)

### Что это?

Паттерн, при котором несколько связанных компонентов работают вместе и разделяют состояние через контекст или пропсы.

### Зачем нужен?

* Упрощает использование сложных UI-составных элементов.
* Позволяет сохранять API компонента декларативным и читабельным.
* Избегает проп-дриллинга (прокидывания пропсов через многие уровни).

### Пример Compound Components

```jsx
const TabsContext = React.createContext();

function Tabs({ children, defaultIndex = 0 }) {
  const [activeIndex, setActiveIndex] = React.useState(defaultIndex);

  return (
    <TabsContext.Provider value={{ activeIndex, setActiveIndex }}>
      <div className="tabs">{children}</div>
    </TabsContext.Provider>
  );
}

function TabList({ children }) {
  return <div className="tab-list">{children}</div>;
}

function Tab({ index, children }) {
  const { activeIndex, setActiveIndex } = React.useContext(TabsContext);
  const isActive = activeIndex === index;

  return (
    <button
      className={`tab ${isActive ? 'active' : ''}`}
      onClick={() => setActiveIndex(index)}
    >
      {children}
    </button>
  );
}

function TabPanels({ children }) {
  const { activeIndex } = React.useContext(TabsContext);
  return <div className="tab-panels">{children[activeIndex]}</div>;
}

function TabPanel({ children }) {
  return <div className="tab-panel">{children}</div>;
}

// Использование
<Tabs defaultIndex={0}>
  <TabList>
    <Tab index={0}>Вкладка 1</Tab>
    <Tab index={1}>Вкладка 2</Tab>
  </TabList>
  <TabPanels>
    <TabPanel>Контент вкладки 1</TabPanel>
    <TabPanel>Контент вкладки 2</TabPanel>
  </TabPanels>
</Tabs>
```

---

## 4. Контролируемые и неконтролируемые состояния

### Определение

* **Контролируемые компоненты** — компоненты, у которых значение и состояние полностью контролируются внешним кодом через пропсы.
* **Неконтролируемые компоненты** — компоненты, которые самостоятельно управляют своим состоянием, а внешняя часть не контролирует его напрямую.

### Когда использовать?

* Контролируемые — когда нужно синхронизировать состояние компонента с внешним состоянием (например, форма, контролируемая React state).
* Неконтролируемые — когда состояние не нужно часто менять или отслеживать извне (например, простой input без сложной логики).

---

### Пример контролируемого компонента (Input)

```jsx
function ControlledInput({ value, onChange }) {
  return <input value={value} onChange={e => onChange(e.target.value)} />;
}

// Использование
function Parent() {
  const [text, setText] = React.useState('');
  return <ControlledInput value={text} onChange={setText} />;
}
```

---

### Пример неконтролируемого компонента (Input)

```jsx
function UncontrolledInput() {
  const inputRef = React.useRef();

  function handleClick() {
    alert('Текущее значение: ' + inputRef.current.value);
  }

  return (
    <>
      <input ref={inputRef} defaultValue="Привет" />
      <button onClick={handleClick}>Показать значение</button>
    </>
  );
}
```

---

## 5. Рекомендации по использованию

| Паттерн                     | Когда применять                                                    | Преимущества                                                          |
| --------------------------- | ------------------------------------------------------------------ | --------------------------------------------------------------------- |
| Слоты / Render Props        | Нужно вставлять кастомное содержимое, функциональную логику        | Максимальная гибкость и расширяемость                                 |
| Compound Components         | Несколько связанных компонентов, которые должны совместно работать | Чистый и декларативный API, простой доступ к состоянию через контекст |
| Контролируемые компоненты   | Требуется полный контроль внешнего состояния                       | Полная синхронизация с UI и данными                                   |
| Неконтролируемые компоненты | Простые формы, локальное состояние без необходимости синхронизации | Проще писать и использовать, меньше boilerplate                       |

---

## 6. Итог

Знание и умелое использование этих паттернов — ключ к построению качественной, масштабируемой и удобной UI-библиотеки. Они позволяют:

* Создавать компоненты с мощным, но простым API
* Добиваться максимальной гибкости и переиспользуемости
* Обеспечивать удобство и предсказуемость для пользователей вашей библиотеки

---

## Дополнительные материалы

* [React docs: Render Props](https://reactjs.org/docs/render-props.html)
* [Compound Components pattern — Kent C. Dodds](https://kentcdodds.com/blog/compound-components-with-react-hooks)
* [Controlled vs Uncontrolled Components](https://reactjs.org/docs/forms.html#controlled-components)

---

**Автор:** Ваша команда разработки UI-библиотеки
**Дата:** 2025

