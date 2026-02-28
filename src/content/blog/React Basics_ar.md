---
title: React بالعربي
description: React 101
pubDate: Jul 15 2022
heroImage: ../../assets/react-basics.jpg
---

## حاجات هتلاقي إنك محتاجها قبل ما أبدأ

قبل ما نبدأ (React)، لازم نتأكد من الأساسات.

### الـSPA vs MPA: إيه الفرق وليه العالم راح للـ SPA

زمان، كانت المواقع شغالة بنظام **MPA (Multi-Page Applications)**.

- **يعني إيه؟** يعني كل ما تدوس على لينك، المتصفح يروح للـ Server يقوله "هات الصفحة دي"، والـ Server يرجعله صفحة HTML كاملة جديدة.
- **النتيجة:** الشاشة تبيض لحظة، والصفحة تعمل Reload كامل. حاجة كلاسيكية زي المواقع القديمة.

دلوقتي، الدنيا راحت للـ **SPA (Single-Page Applications)**، هنا يظهر React.

- **يعني إيه؟** هي صفحة HTML واحدة بس "فاضية" بتحمل أول مرة. بعدين JavaScript (React) هو اللي بيملاها محتوى.
- **النتيجة:** لما تتنقل بين الصفحات، مفيش Reload. الدنيا ناعمة وسريعة زي التطبيقات اللي على موبايلك (Facebook, Gmail).

| المقارنة           | الـMPA (التقليدي)           | الـSPA (الحديث - React)      |
| :----------------- | :-------------------------- | :--------------------------- |
| **طريقة الشغل**    | كل صفحة ملف HTML منفصل      | صفحة HTML واحدة بتتغير بـ JS |
| **التحميل**        | أسرع في أول مرة (HTML جاهز) | أبطأ سنة في الأول (بيحمل JS) |
| **التنقل**         | بطيء (Reload كل مرة)        | طيارة (بدون Reload)          |
| **تجربة المستخدم** | "محسوسة" ومتقطعة            | ناعمة ومتصلة                 |

---

### تجهيز بيئة العمل (Node.js & npm)

عشان أدوات React الحديثة (زي Vite اللي هنستخدمه) تشتغل، لازم يكون عندك **Node.js** على جهازك. هو اللي بيخلينا نشغل JavaScript بره المتصفح، وبييجى معاه **npm** (مدير الحزم) اللي بننزل بيه المكتبات.

1.  افتح الـ Terminal (أو cmd) واكتب:
    ```bash
    node -v
    npm -v
    ```
2.  لو طلعلك أرقام (زي `v20.x.x`) يبقى أنت تمام.
3.  لو طلعلك Error، روح نزل الـ LTS version من [الموقع الرسمي](https://nodejs.org).

---

### ليه اخترت أبدأ بـ Vite

عشان أكتب React، عرفت إننا محتاجين "Bundler". كان قدامي اختيارين كبار: Webpack و Vite.
أنا قررت أشتغل بـ **Vite** عشان أسرع وأسهل بكتير في البداية.

عشان أبدأ المشروع، فتحت الـ Terminal وكتبت الأوامر دي:

```bash
npm create vite@latest my-app -- --template react
cd my-app
npm install
npm run dev
```

الـ Files اللي هتشوفها:

- الـ `src/main.jsx`: ده المدخل الرئيسي (Entry Point) اللي بيربط React بالـ DOM.
- الـ `src/App.jsx`: ده الـ Component الرئيسي بتاعك، وغالباً هتمسح اللي فيه وتبدأ تكتب كودك.

---

## ملخصي لأهم حاجات في الـ JavaScript

لأن React كله JavaScript، دي الحاجات اللي راجعتها ولقيتها مهمة جداً.

### الـ`var` vs `let` vs `const`

- الـ **`var`:** (انساها) مشاكلها كتير في الـ Scope.
- الـ **`let`:** للمتغيرات اللي _قيمتها هتتغير_ (زي عداد loop).
- الـ **`const`:** (المفضلة في React) للمتغيرات اللي *مش هتتغير*. حتى الـ Arrays والـ Objects بنعرفهم `const` (لأنك مش بتغير المتغير نفسه، بتغير محتواه).

### الـ Arrow Functions

مش بس شكلها مختلف، دي كمان بتحل مشاكل الـ `this` اللي كانت بتجنن المطورين زمان.

```javascript
// دالة عادية
function sayHello(name) {
  return "Hello " + name;
}

// Arrow Function
const sayHello = (name) => `Hello ${name}`;
```

### الـ Destructuring

إزاي تطلع البيانات من جوه Objects أو Arrays .

```javascript
// مع Objects
const user = { id: 1, name: "Amr", role: "Admin" };
const { name, role } = user; // بدل ما تكتب user.name و user.role

// مع Arrays (مهمة جداً في useState)
const colors = ["red", "blue"];
const [primary, secondary] = colors;
```

### الـ Spread Operator `...`

ده "الجوكر" بتاعنا. بنستخدمه عشان ننسخ الـ Data أو ندمجها من غير ما نعدل في الأصل (Immutability).

```javascript
const oldUser = { name: "Amr", age: 25 };
// عايز أعمل يوزر جديد بنفس البيانات بس أغير السن
const newUser = { ...oldUser, age: 26 };
// newUser بقا { name: 'Amr', age: 26 } والقديم زي ما هو
```

### الـ Array Methods (Map, Filter, Reduce)

دول عيشك وملحك في React.

- الـ**`map`:** بتحول "Data" لـ "UI". (معايا array أسماء، عايز أخليهم `<li>`).
- الـ**`filter`:** بتصفي البيانات. (عايز المنتجات اللي سعرها أقل من 100 جنيه).

### الـModules (Import / Export)

إزاي تقسم كودك لملفات صغيرة نظيفة.

- الـ`export default`: بتصدر حاجة واحدة رئيسية من الملف.
- الـ`export`: بتصدر كذا حاجة (Named Export).

### الـ Promises & Async/Await (عقدة واتحلت)

دي كانت بتلخبطني جداً، بس فهمتها كده:
**الـ Promise** عامل زي "وصل أمانة" أو الإيصال اللي بتاخده في الكافيه.

- أنت طلبت قهوة (طلبت داتا من Server).
- الكشير اداك إيصال (Promise) وقال لك "استنى شوية".
- الإيصال ده مش القهوة، ده وعد إن القهوة جاية. يا إما تيجي (Resolve)، يا إما المكنة تبوظ (Reject).

**الـ Async/Await** بقى هما اللي بيخلو الكود يستنى بأدب.

- **Async**: بتقول للـ JavaScript "الدالة دي فيها حاجات هتاخد وقت، فماتعطلش الدنيا".
- **Await**: بتقول للكود "اقف هنا دقيقة (زي ما بتقف تستنى رقمك) لحد ما الـ Promise يتنفذ والنتيجة تيجي"، وبعدين كمل.

```javascript
async function getCoffee() {
  try {
    console.log("طلبت القهوة...");
    const response = await orderCoffee(); // Await: واقف مستني رقمي يننده
    console.log("استلمت القهوة:", response);
  } catch (error) {
    console.log("حصلت مشكلة في المكنة:", error);
  }
}
```

---

## أول خطوة ليا جوه الـ React

عشان نفهم أحسن، قررت أطبق عملي واحنا ماشيين. **هنعمل سوا أبلكيشن لـ "إدارة المهام" (Task Manager).**
كل ما نتعلم حاجة، هنزودها في المشروع بتاعنا.

### يعني إيه React أصلاً؟

هو مكتبة (Library) مش Framework (زي Angular)، بتركز على حاجة واحدة بس: **بناء واجهة المستخدم (UI) من خلال Components**.
فكرته العبقرية هي **Virtual DOM**: نسخة خفيفة من الصفحة في الذاكرة، React بيعدل فيها الأول، ويشوف إيه اللي اتغير، ويروح يعدله في الصفحة الحقيقية. ده اللي بيخليه سريع جداً.

### الـ Components & JSX

أي صفحة في React عبارة عن مكعبات صغيرة اسمها Components.
الـ Component هو دالة JavaScript بترجع HTML (بس بنسميه **JSX**).

```javascript
// ده Component بسيط لكرت المهمة
function TaskCard() {
  return (
    <div className="task-card">
      <h3>ذاكر React</h3>
    </div>
  );
}
```

#### قوانين JSX

- لازم ترجع **Element واحد بس** كبير (أب) شايل جواه كل حاجة. (أو استخدم `<>...</>` Fragments)
- لازم تقفل كل التاجات `<img />` مش `<img>`.
- الـ `class` بقت `className` (عشان class كلمة محجوزة في JS).

---

### إزاي الـ Components تكلم بعضها (Props)

الـ **Props** (اختصار Properties) هي الطريقة اللي الـ "أب" بيبعت بيها داتا للـ "ابن".

- هي للقراءة فقط (**Read-only**). الابن ما ينفعش يغير الـ Props اللي جاياه، لكن يقدر يستخدمها.

```javascript
// الأب (التطبيق كله)
function App() {
  return (
    <div>
      <TaskCard title="ذاكر React" />
      <TaskCard title="نام بدري" />
    </div>
  );
}

// الابن (الكرت)
function TaskCard(props) {
  return (
    <div className="task-card">
      <h3>{props.title}</h3>
    </div>
  );
}
```

---

### أساس الـ Component (State)

الـ **State** هي الذاكرة بتاعت الـ Component. أي حاجة بتتغير وتأثر في شكل الصفحة (زرار بنعد عليه، فورم بنكتب فيه، قائمة بتزيد) لازم تكون في State.
لما الـ State تتغير، React بيعرف لوحده ويعيد رسم الـ Component (Re-render).

```javascript
import { useState } from "react";

function TaskManager() {
  // القائمة بتاعتنا بقت في State عشان لما تزيد، الصفحة تتحدث
  const [tasks, setTasks] = useState(["ذاكر React", "اشرب قهوة"]);

  return (
    <div>
      <button onClick={() => setTasks([...tasks, "مهمة جديدة"])}>
        زود مهمة
      </button>

      {tasks.map((task, index) => (
        <TaskCard key={index} title={task} />
      ))}
    </div>
  );
}
```

**⚠️ تحذير هام جداً:**
أبداً، أبداً، أبداً ما تعدلش الـ State مباشرة! `tasks.push("New")` دي جريمة.
استخدم الدالة اللي جاية معاه `setTasks` عشان React يحس بالتغيير.

---

## تجربتي مع الـ Events والـ Lists

### التعامل مع الـ Events

شبه الـ HTML بس camelCase.

- `onclick` -> `onClick`
- `onsubmit` -> `onSubmit`

```javascript
function Form() {
  function handleSubmit(e) {
    e.preventDefault(); // عشان الصفحة ما تعملش reload
    console.log("Form Submitted!");
  }

  return (
    <form onSubmit={handleSubmit}>
      <button>Send</button>
    </form>
  );
}
```

### الـ Conditional Rendering (أظهر إيه امتى؟)

بلاش `if` و `else` جوه الـ JSX عشان مش هتنفع. استخدم دول:

- **Ternary Operator `? :`**: (لو آه اعمل كذا، لو لأ اعمل كذا).

  ```javascript
  {
    isLoggedIn ? <UserDashboard /> : <LoginButton />;
  }
  ```

- **Logical AND `&&`**: (لو الشرط موجود، اظهر ده).
  ```javascript
  {
    hasError && <ErrorMessage />;
  }
  ```

### القوائم والـ Keys 🔑

لما تيجي تعرض List، لازم تدي لكل عنصر `key` مميز (زي ID).
الـ Key ده بيساعد React يعرف مين العنصر اللي اتضاف أو اتمسح بالظبط بدل ما يهد القائمة كلها ويبنيها تاني.

```javascript
const todos = [
  { id: 1, text: "Study" },
  { id: 2, text: "Sleep" },
];

function TodoList() {
  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>{todo.text}</li> // الـ key هنا مهم جداً
      ))}
    </ul>
  );
}
```

### الـ Forms & Controlled Components

دلوقتي عايزين نكتب المهمة بنفسنا مش بس ندوس زرار.

في الـ HTML العادي، الـ Input هو اللي ماسك قيمته. في React، احنا بنحب نتحكم في كل حاجة (Controlled Components).
هنعمل متغير للـ Input ونربطه بيه.

```javascript
function AddTaskForm() {
  const [newTask, setNewTask] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log("هضيف مهمة:", newTask);
    setNewTask(""); // نفضي الخانة تاتي
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={newTask}
        onChange={(e) => setNewTask(e.target.value)}
      />
      <button>إضافة</button>
    </form>
  );
}
```

### الـ Styling (التنسيق)

عندك كذا طريقة لتنسيق الـ Components:

1.  الـ **Inline Styles:** (مش مستحبة أوي) `style={{ color: 'red' }}`.
2.  الـ **CSS Files:** اعمل import للملف عادي `import './App.css'`.
3.  الـ **CSS Modules:** (للشغل النضيف) بتعمل ملف `Button.module.css` وتعمل import classes from it.
4.  الـ **Frameworks:** زي TailwindCSS (الأشهر حالياً).

---

## اللي فهمته عن الـ Hooks ✨

من React 16.8، الـ Hooks غيرت الدنيا. هي دوال بتبدأ بـ `use` بتخليك تستخدم مميزات React جوه الـ Functional Components.

### الـ `useEffect`:

عايز تحفظ المهام دي عشان لما تعمل Refresh ما تضيعش؟ هنستخدم `useEffect`.
ده المكان اللي بنتعامل فيه مع حاجات بره الصفحة (زي الـ LocalStorage او API).

```javascript
// هتشتغل كل ما الـ tasks تتغير
useEffect(() => {
  localStorage.setItem("my-tasks", JSON.stringify(tasks));
}, [tasks]);
```

### الـ `useRef`:

ليها استخدامين:

1.  **تمسك عنصر DOM:** مثلاً عايز أول ما تفتح البرنامج، الكتابة تكون جوه الـ Input علطول (Focus).
2.  **تخزن قيمة:** بس القيمة دي لما تتغير **مش بتعمل Re-render**.

```javascript
const inputRef = useRef(null);

useEffect(() => {
  inputRef.current.focus(); // أول ما تفتح، اقف جوه الخانة
}, []);

return <input ref={inputRef} ... />;
```

---

## الـ Hooks لأداء افضل (Performance Optimization)

مش لازم تستخدم دول عمال على بطال، بس لما التطبيق يتقل، دول الحل.

### -الـ `React.memo`

بتغلف بيها الـ Component عشان تقوله: "لو الـ Props اللي جيالك ماتغيرتش، ما ترسمش نفسك تاني".

```javascript
const MyComponent = React.memo(function MyComponent(props) {
  /* render logic */
});
```

### -الـ `useMemo`

لو عندك عملية حسابية تقيلة جداً، استخدم `useMemo` عشان يحفظ النتيجة وما يحسبهاش تاني إلا لو المدخلات اتغيرت.

### -الـ `useCallback`

شبه `useMemo` بس للـ Functions. بتضمن إن الـ Function متتغيرش (عنوانها في الذاكرة مايتغيرش) مع كل Render، وده مهم لو بتبعتها لـ Child Component محمي بـ `React.memo`.

---

## الـCustom Hooks:

لو لقيت نفسك بتكتب نفس الكود (Logic) في كذا Component، افصله في Hook خاص بيك.
مثال: Hook بيقولنا احنا Online ولا Offline.

```javascript
// useOnlineStatus.js
import { useState, useEffect } from "react";

export function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(true);

  useEffect(() => {
    // listeners للشبكة
    window.addEventListener("online", () => setIsOnline(true));
    window.addEventListener("offline", () => setIsOnline(false));
    return () => {
      /* cleanup code */
    };
  }, []);

  return isOnline;
}
```

دلوقتي أي Component يقدر يستخدمه بسطر واحد: `const isOnline = useOnlineStatus();`. عبقرية!

---

## إدارة الـ State المتقدمة (Context & Reducers)

### الحل لـ Prop Drilling (Context API)

بدل ما تقعد تباصي الـ Props من الجد للأب للابن للحفيد (Prop Drilling)، الـ Context بيعمل "سحابة" داتا فوق الـ Components. أي حد محتاج الداتا (زي الـ Theme أو User)، يقدر ياخدها.

- **Create:** `const ThemeContext = createContext('light');`
- **Provider:** `<ThemeContext.Provider value="dark"> ... </ThemeContext.Provider>`
- **Consume:** `const theme = useContext(ThemeContext);`

### الـ`useReducer`:

لو عندك State معقدة (زي عربية تسوق فيها منتجات كتير، إضافة، حذف، تعديل كميات)، `useState` هتكون فوضوية.
الـ`useReducer` بيخليك تجمع كل طرق التعديل (Actions) في دالة واحدة اسمها **Reducer**. (نفس فكرة Redux).

---

## أول مشروع عملته: To-Do List ✅

تعالوا نركب كل المكعبات دي مع بعض في `App.jsx`.

**الخطوات:**

1.  `npm create vite@latest todo-app`
2.  هنكتب الكود ده اللي بيجمع: State (عشان القائمة)، Inputs (عشان الإضافة)، و Effects (عشان الحفظ).

```javascript
import { useState, useEffect, useRef } from "react";
import "./App.css";

function App() {
  // 1. الـ State: القائمة بتاعتنا
  const [tasks, setTasks] = useState(() => {
    // بنحاول نجيب اللي متخزن الاول
    const saved = localStorage.getItem("my-tasks");
    return saved ? JSON.parse(saved) : [];
  });

  const [newTask, setNewTask] = useState("");
  const inputRef = useRef(null);

  // 2. الـ Effect: نحفظ أي تغيير
  useEffect(() => {
    localStorage.setItem("my-tasks", JSON.stringify(tasks));
  }, [tasks]);

  // focus أول ما نفتح
  useEffect(() => {
    inputRef.current.focus();
  }, []);

  const handleAdd = () => {
    if (!newTask) return;
    setTasks([...tasks, { id: Date.now(), title: newTask, done: false }]);
    setNewTask("");
  };

  const toggleTask = (id) => {
    setTasks(tasks.map((t) => (t.id === id ? { ...t, done: !t.done } : t)));
  };

  return (
    <div className="container">
      <h1>مدير المهام الخاص بي</h1>

      <div className="add-task">
        <input
          ref={inputRef}
          value={newTask}
          onChange={(e) => setNewTask(e.target.value)}
          placeholder="وراك إيه؟"
        />
        <button onClick={handleAdd}>إضافة</button>
      </div>

      <div className="list">
        {tasks.map((task) => (
          <div
            key={task.id}
            className={`task-card ${task.done ? "done" : ""}`}
            onClick={() => toggleTask(task.id)}
          >
            <h3>{task.title}</h3>
            <span>{task.done ? "✅" : "⏳"}</span>
          </div>
        ))}
      </div>
    </div>
  );
}

export default App;
```

---
