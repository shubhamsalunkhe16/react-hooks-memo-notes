# 🧠 What Are React Hooks?

- special functions that let you `use state, lifecycle methods, and other React features in functional components`—without writing a class.
- introduced in `React 16.8`

---

## 🔄 Rules of Hooks

1. ✅ Only call hooks `at the top level`.
2. ✅ Only call hooks `from React functions` (components or custom hooks).
3. ✅ Cannot use inside loops or conditions.

---

## 📘 Commonly Used React Hooks

| `Hook`                | `Purpose (Short Description)`                                             | `Syntax`                                                           |
| --------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| `useState`            | Adds local state to functional components                                 | `const [state, setState] = useState(initialValue)`                 |
| `useEffect`           | Runs side effects like data fetching, subscriptions, DOM updates          | `useEffect(() => { ... }, [dependencies])`                         |
| `useContext`          | Accesses values from React context                                        | `const value = useContext(MyContext)`                              |
| `useRef`              | Creates a mutable ref that persists across renders                        | `const ref = useRef(initialValue)`                                 |
| `useMemo`             | Memoizes a computed value to avoid recalculating                          | `const memoizedValue = useMemo(() => computeFn(), [dependencies])` |
| `useCallback`         | Memoizes a function to avoid re-creating it unnecessarily                 | `const memoizedFn = useCallback(() => { ... }, [dependencies])`    |
| `useReducer`          | Alternative to `useState` for complex state logic                         | `const [state, dispatch] = useReducer(reducerFn, initialState)`    |
| `useLayoutEffect`     | Like `useEffect`, but runs `synchronously` after all DOM mutations        | `useLayoutEffect(() => { ... }, [dependencies])`                   |
| `useImperativeHandle` | Customizes what a parent gets when using `ref` with `forwardRef`          | `useImperativeHandle(ref, () => exposedValues, [dependencies])`    |
| `use`                 | Lets you use a promise directly inside components (e.g., fetch, suspense) | `const data = use(fetchData())`                                    |
| `useOptimistic`       | Show optimistic UI updates before server confirms changes                 | `const [state, add] = useOptimistic(actual, (s, v) => [...s, v])`  |
| `useFormStatus`       | Access a form’s pending state or errors (great for `form` + `action`)     | `const { pending } = useFormStatus()`                              |
| `useActionState`      | Manage form state and result from server action                           | `const [state, action] = useActionState(serverAction, initial)`    |

---

## 📌 Summary

| Feature                   | Class Components          | Functional + Hooks     |
| ------------------------- | ------------------------- | ---------------------- |
| State                     | `this.state`              | `useState()`           |
| Lifecycle methods         | `componentDidMount`, etc. | `useEffect()`          |
| Cleaner syntax            | ❌                        | ✅                     |
| Logic reuse (composition) | Difficult (HOC)           | Easy with custom hooks |

---

# 🔍 What is `useState`?

used to add `state` to functional components.

```tsx
const [state, setState] = useState(initialValue);
```

- `state`: Current value
- `setState`: Function to update the value (triggers a re-render)

---

## 🧠 Syntax & Usage

### ✅ Basic Usage

```tsx
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}
```

---

## ⚙️ `useState` Parameters and Behavior

### 1. `Initial State Value`

```tsx
const [name, setName] = useState("Shubham");
```

You can pass:

- A `primitive` (string, number, boolean)
- An `object`
- An `array`
- Or even a function for lazy initialization

---

### 2. `Lazy Initialization`

Useful if initial state is expensive to compute.

```tsx
const [value, setValue] = useState(() => expensiveComputation());

const [data, setData] = useState(() => {
  const cached = localStorage.getItem("userData");
  return cached ? JSON.parse(cached) : [];
});
```

This function only runs `on the first render`.

---

### 3. `Updating State`

#### a) Direct value

```tsx
setCount(5);
```

#### b) Functional update (recommended if based on previous state)

```tsx
setCount((prev) => prev + 1);
```

This is `safer` when updating inside closures or async logic.

---

### 4. `State is Isolated`

Each `useState` call is `independent`:

```tsx
const [count, setCount] = useState(0);
const [name, setName] = useState("Shubham");
```

---

## 🧱 Objects and Arrays in `useState`

### ❗ Replace, Not Merge

```tsx
const [user, setUser] = useState({ name: "Shubham", age: 27 });

// ❌ This removes `name`
setUser({ age: 28 });

// ✅ Use spread to merge
setUser((prev) => ({ ...prev, age: 28 }));
```

Unlike class components, `useState` `does not merge` object state automatically.

---

## 🧓🏻 `useState` vs `this.state` in Class Components

| Feature               | Functional Component (`useState`) | Class Component (`this.state`)    |
| --------------------- | --------------------------------- | --------------------------------- |
| Syntax                | `const [x, setX] = useState()`    | `this.state = {}`                 |
| Updating              | `setX(newX)`                      | `this.setState({ x: newX })`      |
| Merging State         | ❌ Manual (`...prev`)             | ✅ Auto shallow merge             |
| Functional Update     | ✅ `setX(prev => ...)`            | ✅ `this.setState((prev) => ...)` |
| State Scoping         | Each state is isolated            | All state in one object           |
| React Hook Dependency | ✅ Requires hooks                 | ❌ No hooks                       |
| Lifecycle Handling    | With hooks like `useEffect`       | With lifecycle methods            |

---

## 🧪 Class Component Example

```tsx
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  increment = () => {
    this.setState((prev) => ({ count: prev.count + 1 }));
  };

  render() {
    return <button onClick={this.increment}>Count: {this.state.count}</button>;
  }
}
```

---

## ✅ Best Practices

- Use `functional updates` when new state depends on the old one
- For `complex state`, consider `useReducer`
- Use `separate `useState` calls` for unrelated state variables
- `Avoid direct object mutation` in state

---

## 💬 Common Mistakes

| Mistake                                  | Fix                                        |
| ---------------------------------------- | ------------------------------------------ |
| Updating objects without spread          | `setObj(prev => ({ ...prev, newKey }))`    |
| Accessing state immediately after update | Remember updates are async                 |
| Using old state in closure               | Use functional update: `setX(prev => ...)` |

---

### ✅ `useState` — TypeScript Example

```tsx
const [count, setCount] = useState<number>(0);

const [name, setName] = useState<string>("Shubham");

const [items, setItems] = useState<string[]>([]);
```

---

# 🔍 What is `useEffect`?

allows you to `run side effects` in function components.

Side effects include:

- Fetching data
- Subscribing to events
- Setting timers
- Manually interacting with the DOM

---

## 🧠 Syntax

```tsx
useEffect(() => {
  // Your side-effect logic

  return () => {
    // Optional cleanup logic
  };
}, [dependencies]);
```

---

## ⚙️ How `useEffect` Works

- `Runs after render`
- Only re-runs if `dependencies change`
- The `cleanup function` runs:

  - Before the effect re-runs (on dependency change)
  - When the component `unmounts`

---

## 🧪 Basic Example

```tsx
useEffect(() => {
  console.log("Component mounted or updated");
}, []);
```

Runs once on `mount` (because `[]` means no dependencies).

---

## 🔁 Dependency Array Behavior

| Dependency Array      | When Effect Runs                    |
| --------------------- | ----------------------------------- |
| `[]`                  | On mount (like `componentDidMount`) |
| `[a, b]`              | On mount + when `a` or `b` changes  |
| _No dependency array_ | On every render (not recommended)   |

---

## ♻️ Cleanup Example (Unsubscribe, clearInterval, etc.)

```tsx
useEffect(() => {
  const interval = setInterval(() => {
    console.log("tick");
  }, 1000);

  return () => {
    clearInterval(interval); // Cleanup
  };
}, []);
```

---

## 🧓 `useEffect` vs Lifecycle Methods

| Lifecycle  | Class Component        | Functional Component                    |
| ---------- | ---------------------- | --------------------------------------- |
| On mount   | `componentDidMount`    | `useEffect(() => {...}, [])`            |
| On update  | `componentDidUpdate`   | `useEffect(() => {...}, [deps])`        |
| On unmount | `componentWillUnmount` | `return () => {...}` inside `useEffect` |

---

## ⚠️ Common Mistakes

### ❌ Missing dependencies

```tsx
useEffect(() => {
  fetchData(id); // id is not in dependency
}, []); // ❌ BUG if `id` changes
```

✅ Fix:

```tsx
useEffect(() => {
  fetchData(id);
}, [id]);
```

---

### ❌ Triggering too often (no deps)

```tsx
useEffect(() => {
  console.log("Runs on every render"); // BAD
});
```

✅ Fix: Always provide dependencies.

---

## ✅ Real-World Use Cases

### 1. `Fetching API data`

```tsx
useEffect(() => {
  async function fetchData() {
    const res = await fetch("/api/user");
    const data = await res.json();
    setUser(data);
  }

  fetchData();
}, []);
```

---

### 2. `Listening to window events`

```tsx
useEffect(() => {
  const handleResize = () => console.log(window.innerWidth);
  window.addEventListener("resize", handleResize);

  return () => window.removeEventListener("resize", handleResize);
}, []);
```

---

### 3. `Using `useEffect` with setInterval`

```tsx
useEffect(() => {
  const id = setInterval(() => {
    setTime(new Date());
  }, 1000);

  return () => clearInterval(id); // Cleanup
}, []);
```

---

## 👨‍🔬 Advanced Behavior

### 1. `Multiple `useEffect`s`

You can (and should) separate concerns:

```tsx
useEffect(() => {
  // API call
}, [userId]);

useEffect(() => {
  // Theme update
}, [theme]);
```

---

### 2. `Stale Closures`

```tsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log(count); // Might be stale!
  }, 1000);
  return () => clearInterval(timer);
}, []); // ❌ count not in deps
```

✅ Fix with dependency:

```tsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log(count);
  }, 1000);
  return () => clearInterval(timer);
}, [count]);
```

✅ Or use `useRef` to avoid re-running:

```tsx
const countRef = useRef(count);
countRef.current = count;

useEffect(() => {
  const timer = setInterval(() => {
    console.log(countRef.current);
  }, 1000);
  return () => clearInterval(timer);
}, []);
```

---

## 🧠 Why This Happens, if we pass non-primitive value?

JavaScript compares `objects, arrays, and functions by reference`, `not by value`.

```tsx
useEffect(() => {
  console.log("Ran because of options");
}, [options]);
```

Even if `options` looks the same:

```tsx
const options = { darkMode: true };
```

Every render creates a `new object in memory`, so React sees it as a `change`.

---

## ⚠️ The Resulting Issue

### ❌ Problem: Infinite or Unnecessary Effect Calls

```tsx
function App() {
  const options = { darkMode: true };

  useEffect(() => {
    console.log("Effect re-runs on every render");
  }, [options]);

  return <div>Dark mode: {String(options.darkMode)}</div>;
}
```

This runs `on every render` even though `options` never changes _logically_.

---

## ✅ Solution 1: `useMemo`

Memoize the non-primitive value:

```tsx
const options = useMemo(() => ({ darkMode: true }), []);
```

```tsx
useEffect(() => {
  console.log("Effect only runs on first mount");
}, [options]);
```

Now `options` retains the same reference unless dependencies to `useMemo` change.

---

## ✅ Solution 2: `Extract to useState or top-level constant`

```tsx
const defaultOptions = { darkMode: true };
```

```tsx
useEffect(() => {
  console.log("Safe now");
}, [defaultOptions]);
```

Or lift it to `useState` if you need to manage it.

---

## ✅ Solution 3: Use `JSON.stringify()` as a quick fix (⚠️ not ideal)

```tsx
useEffect(() => {
  console.log("Runs only when options content changes");
}, [JSON.stringify(options)]);
```

> ⚠️ This works for simple objects, but is `inefficient` and breaks with functions/cycles.

---

## ✅ Bonus: When using a `function` as a dependency

Functions also change on every render unless memoized:

```tsx
const handleClick = () => { ... };
```

Fix:

```tsx
const handleClick = useCallback(() => { ... }, []);
```

Then:

```tsx
useEffect(() => {
  // Now won't re-run every render
}, [handleClick]);
```

---

## ✅ Summary Table

| Non-Primitive Type | Problem                    | Solution                                                     |
| ------------------ | -------------------------- | ------------------------------------------------------------ |
| Object / Array     | Re-creates every render    | `useMemo`                                                    |
| Function           | Re-creates every render    | `useCallback`                                                |
| Quick workaround   | Force stringify comparison | `JSON.stringify()` (not recommended for perf-sensitive code) |

---

## 🧪 Real Example

```tsx
function App() {
  const filters = useMemo(() => ({ status: "active" }), []);

  useEffect(() => {
    console.log("Runs only when filters change");
  }, [filters]);

  return <div>Using filters safely</div>;
}
```

---

## ✅ Summary

| Topic             | Description                                                               |
| ----------------- | ------------------------------------------------------------------------- |
| `useEffect`       | Hook to run side effects in function components                           |
| `[]`              | Effect runs once on mount                                                 |
| `[deps]`          | Effect runs on mount + any `deps` change                                  |
| `return () => {}` | Cleanup function runs on unmount or before next effect                    |
| Equivalent to     | `componentDidMount`, `componentDidUpdate`, `componentWillUnmount`         |
| Best practices    | Separate concerns into multiple `useEffect`s, always include dependencies |

---

### Note :

- React tracks dependencies in useEffect `by reference`, not by value for `non-primitive` value.

So this:

```tsx
<button
  onClick={() => {
    setUser({ firstname: "Shubham", lastname: "Salunkhe" });
  }}
>
  Update User
</button>;

useEffect(() => {
  console.log("user Updated", user); // runs evevy time even if value of user is same
}, [user]);
```

- Creates a new object → new reference → [user] changed → useEffect runs ✅
- Even if the contents are the same as before.

#### Prevent unnecessary setUser:

```jsx
// option 1:
onClick={() => {
  setUser((prev) => {
    if (prev.firstname === "" && prev.lastname === "") return prev; // return same previous object if values not change
    return { firstname: "", lastname: "" };
  });
}}

// option 2:
useEffect(() => {
    console.log("user Updated", user);
}, [user.firstname,user.lastname]);

// option 3:
useEffect(() => {
  const isSame = user.firstname === "" && user.lastname === "";
  if (!isSame) {
    console.log("user Updated", user);
  }
}, [user]);

// option 4:
useDeepCompareEffect(() => {
    console.log("user Updated", user);
}, [user]);

// useDeepCompareEffect.jsx
import { useRef, useEffect } from "react";
import isEqual from "lodash.isequal";

function useDeepCompareEffect(callback, deps) {
  const prevDepsRef = useRef();

  if (!isEqual(prevDepsRef.current, deps)) {
    prevDepsRef.current = deps;
  }

  useEffect(callback, [prevDepsRef.current]);
}

```

# 🔍 What is `useContext`?

allows `function components to access data from a context`, which can be `shared across the component tree` without passing props manually at every level.

---

## ⚙️ Syntax

```tsx
const value = useContext(MyContext);
```

- `MyContext` must be created using `React.createContext()`
- `value` will be the `nearest value provided` by a `Context.Provider` up the tree

---

## 🧱 `createContext` Example

```tsx
const ThemeContext = React.createContext("light");
```

#### Optional: You can provide a default value like `"light"`.

---

## 🧪 Basic Example

### 1. `Create the Context`

```tsx
// ThemeContext.js
import { createContext } from "react";

export const ThemeContext = createContext("light");
```

---

### 2. `Provide the Context`

Wrap a part of your component tree with `ThemeContext.Provider`.

```tsx
// App.jsx
import { ThemeContext } from "./ThemeContext";
import Child from "./Child";

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Child />
    </ThemeContext.Provider>
    //  <ThemeContext value="dark"> // from React V19
    //   <Child />
    // </ThemeContext>
  );
}
```

---

### 3. `Consume with `useContext``

```tsx
// Child.jsx
import { useContext } from "react";
import { ThemeContext } from "./ThemeContext";

function Child() {
  const theme = useContext(ThemeContext);
  // const theme = use(ThemeContext); // from React V19
  return <div>Current theme: {theme}</div>;
}
```

---

## 🎯 When to Use `useContext`

- `Global themes` (dark/light mode)
- `User authentication state`
- `Language/localization (i18n)`
- `Global settings or config`
- `Sharing state across deeply nested components`

---

## 🏗️ Real World Example: Auth Context

```tsx
// AuthContext.js
import { createContext, useState } from "react";

export const AuthContext = createContext(null);

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);

  const login = (username) => setUser({ name: username });
  const logout = () => setUser(null);

  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}
```

### Usage:

```tsx
// App.jsx
<AuthProvider>
  <Profile />
</AuthProvider>
```

```tsx
// Profile.jsx
const { user, logout } = useContext(AuthContext);
```

---

## ⚠️ Caveats & Performance Tips

### 1. ❗ Entire subtree re-renders if context value changes

```tsx
<AuthContext.Provider value={{ user }}>...</AuthContext.Provider>
```

Every time `user` changes, all components consuming this context `will re-render`.

✅ `Fix`: memoize context value

```tsx
const value = useMemo(() => ({ user, login, logout }), [user]);
```

---

### 2. ❌ Don't use context for high-frequency updates (e.g., mouse position, timers)

Use `context` for `low-frequency`, app-wide data.

---

### 3. ✅ Use Multiple Contexts

Split state by concern to reduce unnecessary re-renders.

```tsx
<ThemeProvider>
  <AuthProvider>
    <App />
  </AuthProvider>
</ThemeProvider>
```

---

## 🧱 `useContext` vs Props vs Redux

| Feature              | `useContext`                       | Props Drilling             | Redux/Recoil/Zustand      |
| -------------------- | ---------------------------------- | -------------------------- | ------------------------- |
| Best for             | Shared, global low-frequency state | Parent-child communication | Complex, global state     |
| Setup effort         | Low                                | None                       | High                      |
| Performance          | Good (with memoization)            | Best                       | Excellent (selective sub) |
| Scales to large apps | Moderate                           | Poor                       | Excellent                 |

---

### ✅ `useContext` — TypeScript Example

```tsx
type UserContextType = {
  user: string;
  setUser: (user: string) => void;
};

const UserContext = React.createContext<UserContextType | undefined>(undefined);

const useUser = () => {
  const context = useContext(UserContext);
  if (!context) throw new Error("useUser must be used within UserProvider");
  return context;
};
```

---

# 📌 What is `useRef`?

1. `Persists a value across renders` `without causing a re-render` when it changes.
2. Provides a `direct reference to a DOM element`.

---

## ⚙️ Syntax

```tsx
const myRef = useRef(initialValue);
```

- Returns an object: `{ current: initialValue }`
- Changing `.current` `does NOT trigger a re-render`

---

## 🎯 Two Main Use Cases

### 1. `Accessing DOM Elements (like `document.getElementById`)`

```tsx
function App() {
  const inputRef = useRef(null);

  const focusInput = () => {
    inputRef.current.focus(); // Direct DOM access
  };

  return (
    <>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus Input</button>
    </>
  );
}
```

✅ Good for:

- Focus control
- Video playback
- Scrolling into view

---

### 2. `Storing Mutable Values That Don’t Cause Re-Renders`

```tsx
function Timer() {
  const count = useRef(0);

  useEffect(() => {
    const interval = setInterval(() => {
      count.current++;
      console.log("⏱️ Count:", count.current);
    }, 1000);
    return () => clearInterval(interval);
  }, []);
}
```

✅ Good for:

- `Tracking state` across renders without re-rendering
- `Storing timeout/interval IDs`
- `Tracking previous props/state`
- `Avoiding stale closures`

---

## 🧪 Example: Tracking Previous State

```tsx
function Counter() {
  const [count, setCount] = useState(0);
  const prevCountRef = useRef();

  useEffect(() => {
    prevCountRef.current = count;
  }, [count]);

  return (
    <>
      <p>Now: {count}</p>
      <p>Before: {prevCountRef.current}</p>
      <button onClick={() => setCount((c) => c + 1)}>Increment</button>
    </>
  );
}
```

---

## ⚠️ Important Notes

### 1. `useRef` does `not` notify React when `.current` changes

Unlike `useState`, changing `.current` does `not` cause a re-render.

### 2. `useRef` is like a `box` that holds a mutable value across renders.

```tsx
const myRef = useRef(123);
console.log(myRef.current); // 123
myRef.current = 456;
```

---

## 🔄 `useRef` vs `useState`

| Feature              | `useState` | `useRef`                    |
| -------------------- | ---------- | --------------------------- |
| Triggers re-render?  | ✅ Yes     | ❌ No                       |
| Holds mutable value? | ✅ Yes     | ✅ Yes                      |
| DOM access           | ❌ No      | ✅ Yes (`ref` prop)         |
| Good for             | UI state   | DOM nodes, mutable tracking |

---

## 🎯 Real-World Use Cases

1. `Focus / scroll to an input`
2. `Avoiding stale values inside `setTimeout``
3. `Tracking if component is mounted`
4. `Measuring DOM size (`offsetHeight`, `getBoundingClientRect`)`
5. `Preventing double submission of a form`
6. `Saving previous props/state`

---

## 📦 `useRef` in Class Components

Equivalent to:

```tsx
this.myRef = React.createRef(); // for DOM access
this.anyVar = someValue; // for mutable vars
```

But `createRef()` creates a `new ref on every render`, unlike `useRef`, which persists.

---

## ✅ Summary

| Feature            | Description                                                     |
| ------------------ | --------------------------------------------------------------- |
| `useRef`           | Returns `{ current }` object                                    |
| For DOM refs       | `<input ref={myRef} />`                                         |
| For value refs     | `useRef(initialValue)`                                          |
| Does not re-render | `current` can change without triggering render                  |
| Common uses        | DOM nodes, timers, storing previous state, avoid stale closures |

---

## ⚠️ Gotchas

- ❌ Don't use `useRef` when you need the UI to `respond to a change` → use `useState`.
- ❌ Don't assign `ref.current = ...` inside JSX directly — use the `ref` prop.
- ✅ Combine `useRef` with `useEffect` when accessing DOM after mount.

---

### ✅ `useRef` — TypeScript Example

```tsx
const inputRef = useRef<HTMLInputElement>(null);

const focusInput = () => {
  inputRef.current?.focus();
};
```

To persist mutable value:

```tsx
const renderCount = useRef<number>(0);
renderCount.current += 1;
```

---

# 🔍 What is `useMemo`?

`memoizes a calculated value` — it recomputes the value `only when its dependencies change`.

---

## 📘 Syntax

```tsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

- The function runs `only if `a`or`b` changed`.
- Otherwise, React returns the `cached result`.

---

## 💡 When to Use

- You have `expensive computations` (filtering, sorting, calculating).
- You want to `prevent unnecessary recalculations` on every render.
- You need `referential equality` (for memoized child props).

---

## ✅ Simple Example: Expensive Calculation

```tsx
function App({ num }) {
  const expensive = useMemo(() => {
    console.log("Computing...");
    let total = 0;
    for (let i = 0; i < 100000000; i++) {
      total += i;
    }
    return total + num;
  }, [num]);

  return <div>{expensive}</div>;
}
```

🔄 The computation `only re-runs` if `num` changes.

---

## 🎯 Real World: Filtering a List

```tsx
function ProductList({ products, search }) {
  const filtered = useMemo(() => {
    return products.filter((p) =>
      p.name.toLowerCase().includes(search.toLowerCase())
    );
  }, [products, search]);

  return (
    <ul>
      {filtered.map((p) => (
        <li key={p.id}>{p.name}</li>
      ))}
    </ul>
  );
}
```

Without `useMemo`, `filter()` runs `on every render`.

---

## 🧠 `useMemo` for Referential Equality (Prevent Re-Renders)

```tsx
const memoizedData = useMemo(() => ({ x: 1 }), []);
<TestComponent data={memoizedData} />;
```

- Without `useMemo`, `data={{ x: 1 }}` creates a `new object` every render
- Memoized children using `React.memo` would still `re-render unnecessarily` without `useMemo`

---

## 🔄 `useMemo` vs `useCallback`

| Hook          | Purpose              | Returns        |
| ------------- | -------------------- | -------------- |
| `useMemo`     | Memoize a `value`    | Computed value |
| `useCallback` | Memoize a `function` | Function       |

---

## ⚠️ Gotchas & Pitfalls

### 1. ❌ Don’t Overuse It

- Use it `only` for `heavy computation` or `referential equality`
- React’s render performance is good — don’t memoize everything

### 2. ❌ Memoizing Primitives Is Pointless

```tsx
const x = useMemo(() => 5, []); // ❌ not needed
```

---

## 💥 Memoizing Non-Primitive Dependencies

If your dependencies are `arrays, objects, or functions`, ensure they’re `stable`:

### ❌ This will recalculate every time:

```tsx
const result = useMemo(() => compute(x), [{ a: 1 }]); // bad
```

### ✅ Fix using `useState`, `useRef`, or top-level constant:

```tsx
const stableObj = useMemo(() => ({ a: 1 }), []);
const result = useMemo(() => compute(x), [stableObj]);
```

---

## 🧪 Example with `React.memo` Child

```tsx
const List = React.memo(({ items }) => {
  console.log("List re-rendered");
  return (
    <ul>
      {items.map((i) => (
        <li key={i}>{i}</li>
      ))}
    </ul>
  );
});

function App() {
  const [count, setCount] = useState(0);

  const items = useMemo(() => [1, 2, 3], []);

  return (
    <>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <List items={items} />
    </>
  );
}
```

Without `useMemo`, `items` would be a new array on each render, and `List` would re-render unnecessarily.

---

## 🧠 When NOT to Use `useMemo`

- For `simple values` like strings, numbers
- When the calculation is `not expensive`
- When value doesn’t depend on props/state

---

## 🧾 Summary

| Feature           | Description                                  |
| ----------------- | -------------------------------------------- |
| Purpose           | Memoize calculated values                    |
| Recalculates when | Dependencies change                          |
| Prevents          | Unnecessary recalculations or re-renders     |
| Use cases         | Expensive calculations, referential equality |
| Don’t use for     | Primitives or cheap computations             |

---

### ✅ `useMemo` — TypeScript Example

```tsx
const filteredData = useMemo<string[]>(() => {
  return data.filter((item) => item.includes(search));
}, [data, search]);
```

---

# 📌 What is `useCallback`?

`memoizes a function` reference — it `returns the same function instance` unless its dependencies change.

> _Think of it as `useMemo` for functions._

---

## 📘 Syntax

```tsx
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

- Returns a `memoized version of the function`
- `Only re-created` when dependencies (`a`, `b`) change

---

## 🎯 Why Use `useCallback`?

- To `prevent unnecessary re-creation of functions` on every render.
- To `avoid re-renders` of memoized children via `React.memo`.
- To `pass stable function references` to `useEffect`, event handlers, or external libraries

---

## 🧪 Real-World Example Without `useCallback`

```tsx
const Button = React.memo(({ onClick, children }) => {
  console.log("Button rendered");
  return <button onClick={onClick}>{children}</button>;
});

function App() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    console.log("Clicked");
  };

  return (
    <>
      <button onClick={() => setCount(count + 1)}>Count {count}</button>
      <Button onClick={handleClick}>Click Me</Button>
    </>
  );
}
```

🛑 Every time `App` re-renders, `handleClick` is recreated → ``Button` re-renders unnecessarily`, even though its props are the same.

---

## ✅ Now With `useCallback`

```tsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

- Now `handleClick` has a `stable identity` (same reference)
- `Button` will NOT re-render thanks to `React.memo`

---

## 🔬 `useCallback` vs `useMemo`

| Hook          | Purpose                    | Returns   | Use When                        |
| ------------- | -------------------------- | --------- | ------------------------------- |
| `useCallback` | Memoize a `function`       | Function  | To avoid re-creating callbacks  |
| `useMemo`     | Memoize a `computed value` | Any value | To cache expensive calculations |

---

## ⚙️ How it Works with `React.memo` and `areEqual`

```tsx
const Child = React.memo(({ onClick }) => {
  console.log("Child rendered");
  return <button onClick={onClick}>Click</button>;
});
```

```tsx
const handleClick = useCallback(() => {}, []);
```

➡️ Because `onClick` has `same reference`, `React.memo` will skip re-render.

### With `areEqual`

```tsx
const MemoChild = React.memo(Child, (prevProps, nextProps) => {
  return prevProps.onClick === nextProps.onClick;
});
```

✅ `useCallback` ensures `onClick` remains `strictly equal`, preventing re-renders.

---

## ⚠️ Common Pitfalls

### 1. `Using Without Dependency Array`

```tsx
const handleClick = useCallback(() => {
  console.log("Clicked"); // stale closure
});
```

- Missing dependencies → stale values inside callback

✅ Always pass all external dependencies.

---

### 2. `Overusing `useCallback``

```tsx
const handleInput = useCallback((e) => {
  setValue(e.target.value);
}, []);
```

- This is `cheap to re-create`, so `useCallback` adds `more complexity` than value.

✅ Only use `useCallback` when:

- The function is passed to a memoized child (`React.memo`)
- The function is expensive to re-create (closure-heavy)
- You care about `reference stability`

---

## 🔄 Example: Stable Callback in `useEffect`

```tsx
const fetchData = useCallback(() => {
  fetch(url).then(...);
}, [url]);

useEffect(() => {
  fetchData();
}, [fetchData]); // ✅ stable
```

Without `useCallback`, `fetchData` would change on each render, causing `useEffect` to run again.

---

## 🧾 Summary

| Feature          | Description                                   |
| ---------------- | --------------------------------------------- |
| Purpose          | Prevents unnecessary re-creation of functions |
| Re-runs when     | Dependencies change                           |
| Works with       | `React.memo`, `useEffect`, libraries          |
| Don’t use for    | Inline functions that don’t go to child props |
| Common use cases | Stable event handlers, API calls, effects     |

---

## 🧠 TypeScript Example

```tsx
const handleClick = useCallback((e: React.MouseEvent<HTMLButtonElement>) => {
  console.log(e.target);
}, []);
```

---

# 🧠 What is `useReducer`?

- used to manage `more complex or structured state logic` than `useState` can easily handle.
- It’s inspired by `Redux-style` state management and works by `dispatching actions` to a reducer function.

---

## 🧩 Syntax

```tsx
const [state, dispatch] = useReducer(reducer, initialState);
```

- `reducer`: Function that takes `(state, action)` and returns a new state.
- `initialState`: Initial value of your state.
- `dispatch`: Function used to send actions to update the state.

---

## 🔄 useReducer vs useState

| Feature       | `useState`                    | `useReducer`                       |
| ------------- | ----------------------------- | ---------------------------------- |
| Simplicity    | Great for small/local state   | Better for complex/multiple states |
| State updates | Direct (`setState(newState)`) | Action-based (`dispatch(action)`)  |
| State shape   | Typically one value           | Often object/structured            |
| Use case      | Form input, toggle            | Forms, modals, undo-redo, counters |
| Readability   | ✅ Simple                     | 🧠 More structured, scalable       |

---

## ✅ When to Use `useReducer`

- Complex state objects (like forms with validation)
- Multiple sub-values updated differently
- Undo/redo or toggle/step logic
- When state updates depend on the previous state
- Replacing Redux-style reducers inside components

---

## 🔄 TypeScript Example

```tsx
import { useReducer } from "react";

type TAction =
  | { type: "INCREMENT" }
  | { type: "DECREMENT" }
  | { type: "INCREMENT_BY"; payload: number };

type TState = { count: number };

const UseReducerTut = () => {
  const reducer = (state: TState, action: TAction) => {
    switch (action?.type) {
      case "INCREMENT":
        return { count: state.count + 1 };
      case "INCREMENT_BY":
        return { count: state.count + action?.payload };
      case "DECREMENT":
        return { count: state.count - 1 };
      default:
        return state;
    }
  };
  const [state, dispatch]: [TState, React.Dispatch<TAction>] = useReducer(
    reducer,
    {
      count: 0,
    }
  );
  return (
    <div>
      <p>
        {state.count}
        <button onClick={() => dispatch({ type: "INCREMENT" })}>Add</button>
        <button onClick={() => dispatch({ type: "DECREMENT" })}>Minus</button>
        <button onClick={() => dispatch({ type: "INCREMENT_BY", payload: 5 })}>
          Add 5
        </button>
      </p>
    </div>
  );
};

export default UseReducerTut;
```

---

# ✅ What is `useId` in React?

`useId` is a React Hook introduced in `React 18` that generates a unique ID that remains the same across the server and client, making it ideal for:

- Accessibility attributes (`aria-labelledby`, `aria-describedby`)
- Form control associations (`label` + `input`)
- SSR (Server-Side Rendering) consistency

---

## ✅ Why use `useId`?

1. `Stable across renders` – the generated ID doesn't change.
2. `Avoids ID collisions` – especially useful when rendering multiple components.
3. `SSR Safe` – prevents hydration mismatches due to mismatched IDs.

---

## ✅ Syntax

```tsx
const id = useId();
```

---

## ✅ Example in TypeScript (TSX)

```tsx
import React, { useId } from "react";

const InputWithLabel: React.FC = () => {
  const id = useId();

  return (
    <div>
      <label htmlFor={id}>Username:</label>
      <input id={id} type="text" />
    </div>
  );
};

export default InputWithLabel;
```

---

## ✅ Example: Accessible Component with `aria-labelledby`

```tsx
import React, { useId } from "react";

const AlertBox: React.FC = () => {
  const id = useId();

  return (
    <div role="alert" aria-labelledby={id}>
      <h2 id={id}>Error</h2>
      <p>Something went wrong. Please try again.</p>
    </div>
  );
};
```

---

## ✅ TypeScript Considerations

- `useId()` returns a `string`.
- No need to provide a type annotation unless assigning to a typed object.

```ts
const id: string = useId(); // Optional – useId already returns string
```

---

## ✅ Advanced: Prefix IDs for Multiple Components

```tsx
const id = `input-${useId()}`;
// results in something like: input-react-:r1:
```

---

## ⚠️ Notes

- Only use `useId` `for DOM ID attributes`. Not for keys in lists.
- It’s `not suitable` for generating form data or persistent user identifiers.

---

## ✅ When Not to Use `useId`

Avoid it for:

- `key` props in lists → use stable unique data (like `uuid`, `item.id`, etc.)
- Tracking or logging → prefer something persistent and domain-specific

---

# 🧠 `useEffect` vs `useLayoutEffect`

| Feature                 | `useEffect`                                | `useLayoutEffect`                           |
| ----------------------- | ------------------------------------------ | ------------------------------------------- |
| `Execution Timing`      | Runs `after` the paint                     | Runs `before` the paint                     |
| `UI Blocking`           | ✅ Non-blocking (async)                    | ❌ Blocking (sync) – delays paint           |
| `Use Case`              | Fetch data, event listeners, logging, etc. | Measure DOM, fix layout before user sees it |
| `Performance`           | More efficient for most side effects       | Slightly heavier – avoid unless necessary   |
| `Browser Paint`         | Runs `after` the screen update             | Runs `before` the screen is updated         |
| `SSR Support (Next.js)` | ✅ Runs only on client                     | ⚠️ Throws warning if used during SSR        |

---

### 📌 When to Use Each

| Use Case                        | Hook to Use       |
| ------------------------------- | ----------------- |
| Fetching data                   | `useEffect`       |
| Subscribing to events           | `useEffect`       |
| Measuring DOM size/position     | `useLayoutEffect` |
| Reading scroll/offset positions | `useLayoutEffect` |
| Animating layout changes        | `useLayoutEffect` |

---

## 🔬 Example: Visual Difference

### 1️⃣ `useEffect` (Non-blocking)

```tsx
import { useEffect, useRef } from "react";

function EffectExample() {
  const boxRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    // Runs after paint
    console.log("Box width (useEffect):", boxRef.current?.offsetWidth);
  }, []);

  return (
    <div ref={boxRef} style={{ width: "100%" }}>
      Box
    </div>
  );
}
```

### 2️⃣ `useLayoutEffect` (Blocking – before paint)

```tsx
import { useLayoutEffect, useRef } from "react";

function LayoutEffectExample() {
  const boxRef = useRef<HTMLDivElement>(null);

  useLayoutEffect(() => {
    // Runs before paint
    console.log("Box width (useLayoutEffect):", boxRef.current?.offsetWidth);
  }, []);

  return (
    <div ref={boxRef} style={{ width: "100%" }}>
      Box
    </div>
  );
}
```

📝 If the DOM is mutated in `useEffect`, the user might `see a flicker`.
Using `useLayoutEffect` avoids this, as it ensures updates happen `before` paint.

---

# 📌 useTransition Hook

React distinguishes between:

- `Urgent updates` (e.g., typing, clicking buttons)
- `Non-urgent updates` (e.g., loading a list, fetching data, filtering)

`useTransition` lets you mark updates as `non-urgent`, so React can:

- Keep the UI responsive (especially input fields)
- Avoid blocking urgent updates

---

### 🔧 Syntax

```tsx
const [isPending, startTransition] = useTransition();
```

- `startTransition(() => { ... })`: tells React this update is `low-priority`
- `isPending`: boolean to know if the transition is still running

---

### 🧠 Real-World Use Cases

## ✅ 1. `Search Filter with Large List`

```tsx
import { useTransition, useState } from "react";

export default function FilteredList({ data }) {
  const [input, setInput] = useState("");
  const [list, setList] = useState(data);
  const [isPending, startTransition] = useTransition();

  const handleChange = (e) => {
    const value = e.target.value;
    setInput(value);

    startTransition(() => {
      const filtered = data.filter((item) =>
        item.toLowerCase().includes(value.toLowerCase())
      );
      setList(filtered);
    });
  };

  return (
    <div>
      <input value={input} onChange={handleChange} placeholder="Search..." />
      {isPending && <p className="text-gray-500">Filtering...</p>}
      <ul>
        {list.map((item, i) => (
          <li key={i}>{item}</li>
        ))}
      </ul>
    </div>
  );
}
```

📍 Without `startTransition`, typing will lag if the list is huge.

---

## ✅ 2. `Navigation or Tab Switching`

Imagine switching tabs and loading a huge UI (e.g., charts, logs, etc.)

```tsx
const [tab, setTab] = useState("profile");
const [isPending, startTransition] = useTransition();

function handleTabChange(newTab) {
  startTransition(() => {
    setTab(newTab); // Heavy tab UI re-renders become non-blocking
  });
}
```

---

## ✅ 3. `Lazy Load or Filtered Pagination`

```tsx
startTransition(() => {
  setVisiblePosts(allPosts.slice(0, count + 20));
});
```

---

## ✅ 4. `Debounced Text Updates`

Let user type freely and show matching content after delay (async or sync):

```tsx
function handleChange(e) {
  const value = e.target.value;
  setInput(value);

  startTransition(() => {
    setSuggestions(getSuggestions(value));
  });
}
```

---

### ⚙️ How it Works Internally

- `useTransition` defers the `update priority` (like React batch scheduling)
- Keeps `urgent updates synchronous`
- Allows `low-priority rendering in background`
- Automatically shows the last committed screen until the new one is ready

---

### 🔍 Comparison with `useDeferredValue`

| Feature              | `useTransition`                | `useDeferredValue`                  |
| -------------------- | ------------------------------ | ----------------------------------- |
| Usage                | Wrap setter logic              | Wrap a value                        |
| Priority Control     | Yes                            | No                                  |
| When to Use          | You trigger non-urgent updates | You consume a value that may change |
| Can handle fetch/set | ✅                             | ❌ (only value deferring)           |

---

### 🔥 Advanced Tip: Use with Suspense

You can show fallback UI during transitions:

```tsx
<Suspense fallback={<Loading />}>
  {isPending ? <Skeleton /> : <HeavyComponent />}
</Suspense>
```

---

### 🧪 Dev Experience

- `isPending` helps show loading spinners or skeletons
- Works well with concurrent features (React 18+)
- Doesn’t require external state libs or debouncing utilities

---

### 🧰 When NOT to Use `useTransition`

- Avoid for `every` state change — only use for `heavy renderings`
- Don't use if update is quick (e.g., toggling visibility of small element)

---

# 💡 Custom Hooks

You can `create your own hooks` to reuse logic across components:

```tsx
function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handler = () => setWidth(window.innerWidth);
    window.addEventListener("resize", handler);
    return () => window.removeEventListener("resize", handler);
  }, []);

  return width;
}
```

---

# New hooks in React 19

1. `useActionState` (previously `useFormState`): streamlines async form submissions—handles pending, success/error, resets form automatically—reducing boilerplate.

- syntax
```tsx
const [state, formAction, isPending] = useActionState(fn, initialState, permalink?);
```

```tsx
// loginAction.ts (this is a server file)
export async function loginAction(_prevState: any, formData: FormData) {
  "use server";
  const email = formData.get("email");
  const password = formData.get("password");

  // Simulate login or validation
  if (!email || !password) {
    return {
      error: "Missing credentials",
      success: false,
    };
  }
  await new Promise<void>((resolve) => setTimeout(resolve, 2000));
  return {
    error: "",
    success: true,
  };
}

// UseActionStateTut.tsx
import { useActionState } from "react";
import { loginAction } from "../serverActions/loginAction";

type TState = {
  error: string;
  success: boolean;
};

const UseActionStateTut = () => {
  const [state, dispatch, isPending] = useActionState<TState, FormData>(
    loginAction,
    {
      error: "",
      success: false,
    }
  );

  return (
    <form action={dispatch}>
      <input type="email" name="email" placeholder="Email" />
      <input type="password" name="password" placeholder="Password" />
      <button type="submit" disabled={isPending}>
        {isPending ? "Logging in..." : "Login"}
      </button>

      {state?.error && <p style={{ color: "red" }}>{state.error}</p>}
      {state?.success && (
        <p style={{ color: "green" }}>Logged in successfully!</p>
      )}
    </form>
  );
};

export default UseActionStateTut;
```

2. `useFormStatus`: lets nested components read the state of their parent `<form>` (pending, data, method, etc.) without prop drilling.

- syntax
```tsx
const { pending, data, method, action } = useFormStatus();
```

```jsx
// UseFormStatusTut.tsx (Client Component)
'use client';
import { useFormStatus } from 'react-dom';
import { loginAction } from './loginAction';
import { useActionState } from 'react';

type TState = {
  error: string;
  success: boolean;
};

function SubmitButton() {
  const { pending } = useFormStatus();

  return (
    <button type="submit" disabled={pending}>
      {pending ? "Submitting..." : "Login"}
    </button>
  );
}

export default function UseFormStatusTut() {
  const [state, dispatch] = useActionState<TState, FormData>(loginAction, {
    error: '',
    success: false,
  });

  return (
    <form action={dispatch}>
      <input type="email" name="email" placeholder="Email" />
      <input type="password" name="password" placeholder="Password" />
      <SubmitButton />

      {state.error && <p className="text-red-600">{state.error}</p>}
      {state.success && <p className="text-green-600">Login successful!</p>}
    </form>
  );
}

```

3. `useOptimistic`: supports optimistic UI updates during async actions, automatically reverting if needed.

- syntax
```tsx
const [optimisticState, addOptimistic] = useOptimistic(state, updateFn);
```
```jsx
'use client';
import React, { useState, useTransition } from "react";
import { useOptimistic } from "react";

function TodoList() {
  const [todos, setTodos] = useState([]);
  const [isPending, startTransition] = useTransition();

  const [optimisticTodos, setOptimisticTodos] = useOptimistic(
    todos,
    (currentTodos, action) => {
      switch (action.type) {
        case "add":
          return [...currentTodos, action.todo];

        case "delete":
          return currentTodos.filter((todo) => todo !== action.todo);

        case "update":
          return currentTodos.map((todo) =>
            todo === action.oldTodo ? action.newTodo : todo
          );

        default:
          return currentTodos;
      }
    }
  );

  const handleAddTodo = async (newTodo) => {
    startTransition(() => {
      setOptimisticTodos({ type: "add", todo: newTodo });
    });

    try {
      await fakeApiCallToAddTodo(newTodo);
      setTodos((prevTodos) => [...prevTodos, newTodo]); // Persist update
    } catch (error) {
      console.error("Failed to add todo:", error);
    }
  };

  const handleDeleteTodo = async (todoToRemove) => {
    startTransition(() => {
      setOptimisticTodos({ type: "delete", todo: todoToRemove });
    });

    try {
      await fakeApiCallToDeleteTodo(todoToRemove);
      setTodos((prevTodos) => prevTodos.filter((todo) => todo !== todoToRemove)); // Persist update
    } catch (error) {
      console.error("Failed to delete todo:", error);
      startTransition(() => {
        setOptimisticTodos({ type: "add", todo: todoToRemove }); // Revert on failure
      });
    }
  };

  const handleUpdateTodo = async (oldTodo, newTodo) => {
    startTransition(() => {
      setOptimisticTodos({ type: "update", oldTodo, newTodo });
    });

    try {
      await fakeApiCallToUpdateTodo(oldTodo, newTodo);
      setTodos((prevTodos) =>
        prevTodos.map((todo) => (todo === oldTodo ? newTodo : todo))
      ); // Persist update
    } catch (error) {
      console.error("Failed to update todo:", error);
      startTransition(() => {
        setOptimisticTodos({ type: "update", oldTodo: newTodo, newTodo: oldTodo }); // Revert on failure
      });
    }
  };

  return (
    <div>
      <ul>
        {optimisticTodos.map((todo, index) => (
          <li key={index}>
            {todo}{" "}
            <button onClick={() => handleDeleteTodo(todo)} disabled={isPending}>
              Delete
            </button>
            <button
              onClick={() => handleUpdateTodo(todo, `Updated ${todo}`)}
              disabled={isPending}
            >
              Update
            </button>
          </li>
        ))}
      </ul>
      <button
        onClick={() => handleAddTodo(`New Todo ${Date.now()}`)}
        disabled={isPending}
      >
        Add Todo
      </button>
    </div>
  );
}

// Simulated API calls
const fakeApiCallToAddTodo = (todo) =>
  new Promise((resolve, reject) => {
    setTimeout(() => {
      Math.random() > 0.3 ? resolve() : reject(new Error("API Error"));
    }, 1000);
  });

const fakeApiCallToDeleteTodo = (todo) =>
  new Promise((resolve, reject) => {
    setTimeout(() => {
      Math.random() > 0.3 ? resolve() : reject(new Error("API Error"));
    }, 1000);
  });

const fakeApiCallToUpdateTodo = (oldTodo, newTodo) =>
  new Promise((resolve, reject) => {
    setTimeout(() => {
      Math.random() > 0.3 ? resolve() : reject(new Error("API Error"));
    }, 1000);
  });

export default TodoList;
```

4. `use()` : Primitive: allows components to `unwrap promises or async contexts directly in render`, compatible with Suspense.

```jsx
// Server-rendered comments fetched from API
import { use } from "react";

type TComment = {
  postId: number,
  id: number,
  name: string,
  email: string,
  body: string,
};

// This is your fetch promise (can be outside the component)
const commentPromise: Promise<TComment[]> = fetch(
  "https://jsonplaceholder.typicode.com/comments"
).then((res) => res.json());

function Comments({ commentsPromise }) {
  // `use` will suspend until the promise resolves.
  const comments: TComment[] = use(commentsPromise);
  return comments.map((comment) => <p key={comment.id}>{comment}</p>);
}

function Page() {
  // When `use` suspends in Comments,
  // this Suspense boundary will be shown.
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Comments />
    </Suspense>
  );
}

// context
const theme = use(ThemeContext);
```

---

# 🧠 What is `React.memo`?

`React.memo` is a `higher-order component (HOC)` used to optimize performance by `preventing unnecessary re-renders` of functional components when their props haven't changed.

```tsx
const MemoizedComponent = React.memo(MyComponent);
```

It performs a `shallow comparison of props by default`.

---

## 🧪 How React.memo Works

### ✅ Primitive Values (string, number, boolean)

Shallow comparison works `perfectly` with primitives:

```tsx
<MyComponent title="Hello" />
```

Re-renders `only when the value changes` (`"Hello"` → `"Hi"`).

---

### ❗ Non-Primitive Values (objects, arrays, functions)

Shallow comparison `fails` with non-primitives because each re-render creates a `new reference`:

```tsx
<MyComponent user={{ name: "Shubham" }} />
```

Even if `user.name` stays the same, a `new object reference` causes re-rendering.

To prevent this, you need either:

- `useMemo` in the parent
- A custom `areEqual` in `React.memo`

---

## 🔁 Custom Comparison Function: `areEqual`

Signature:

```ts
(prevProps, nextProps) => boolean;
```

- Return `true` to `prevent re-render`
- Return `false` to `allow re-render`

---

## 📦 Real World Example – All Cases

### 🔧 `MemoComponent.tsx`

```tsx
type Props = {
  count: number; // primitive
  user: { name: string }; // non-primitive
  onClick: () => void; // function
};

function MemoComponent({ count, user, onClick }: Props) {
  console.log("MemoComponent rendered");
  return (
    <div>
      <p>Count: {count}</p>
      <p>User: {user.name}</p>
      <button onClick={onClick}>Click me</button>
    </div>
  );
}
```

---

### 📌 Default Memo (Without `areEqual`)

```tsx
export default React.memo(MemoComponent);
```

- `count` — re-render only if value changes ✅
- `user` — re-renders `every time` ❌ (new object ref)
- `onClick` — re-renders `every time` ❌ (new function ref)

---

### 🛡️ Custom Comparison with `areEqual`

```tsx
function areEqual(prevProps: Props, nextProps: Props) {
  return (
    prevProps.count === nextProps.count &&
    prevProps.user.name === nextProps.user.name &&
    prevProps.onClick === nextProps.onClick
  );
}

export default React.memo(MemoComponent, areEqual);
```

✅ Now:

- It skips re-render if:

  - `count` is the same
  - `user.name` is unchanged
  - `onClick` reference is the same

❗ If `onClick` is defined inline like this:

```tsx
<MemoComponent onClick={() => console.log("clicked")} />
```

…it will `always re-render` (new function ref). Fix this with `useCallback`.

---

## ✅ Full Working Parent Component (`App.tsx`)

```tsx
import React, { useState, useCallback, useMemo } from "react";
import MemoComponent from "./MemoComponent";

function App() {
  const [count, setCount] = useState(0);

  // ✅ useCallback ensures function reference is stable
  const handleClick = useCallback(() => {
    console.log("Clicked");
  }, []);

  // ✅ useMemo ensures object reference is stable
  const user = useMemo(() => ({ name: "Shubham" }), []);

  return (
    <div>
      <button onClick={() => setCount((c) => c + 1)}>Increase</button>
      <MemoComponent count={count} user={user} onClick={handleClick} />
    </div>
  );
}

export default App;
```

---

## 🧠 Summary

| Prop Type    | Re-render?        | Fix                            |
| ------------ | ----------------- | ------------------------------ |
| Primitive    | No (if unchanged) | None needed                    |
| Object/Array | Yes               | Use `useMemo` + `areEqual`     |
| Function     | Yes               | Use `useCallback` + `areEqual` |

---

## 🚫 When NOT to Use React.memo

- If the component is `already very fast`.
- If props `always change`.
- If the logic inside is cheap and not worth optimizing.

---

# ✅ What is `forwardRef`?

- A React API that allows you to `pass a `ref` from parent to child`.
- Used when a parent wants to `directly access` a child’s DOM node or exposed methods.

---

### 💡 Syntax

```tsx
const MyComponent = React.forwardRef((props, ref) => {
  return <input ref={ref} {...props} />;
});
```

---

### 🧠 Key Points

1. `ref` is `received as the second argument` (after `props`) in `forwardRef`.
2. It only works on `function components` (not regular JS functions or class components).
3. Used to `access DOM elements` or `imperative methods` inside functional components.
4. Works great with `useRef()` and `useImperativeHandle()`.

---

### ✅ Example: Forwarding ref to an `<input>`

#### 👇 Child Component

```tsx
const Input = React.forwardRef<
  HTMLInputElement,
  React.InputHTMLAttributes<HTMLInputElement>
>((props, ref) => {
  return <input ref={ref} {...props} />;
});
```

#### 👇 Parent Component

```tsx
const Parent = () => {
  const inputRef = useRef<HTMLInputElement>(null);

  return (
    <>
      <Input ref={inputRef} />
      <button onClick={() => inputRef.current?.focus()}>Focus</button>
    </>
  );
};
```

---

### 🔐 With `useImperativeHandle` (optional)

To `customize what is exposed` through `ref`:

```tsx
const FancyInput = forwardRef((_, ref) => {
  const inputRef = useRef<HTMLInputElement>(null);

  useImperativeHandle(ref, () => ({
    focus: () => inputRef.current?.focus(),
    clear: () => (inputRef.current!.value = ""),
  }));

  return <input ref={inputRef} />;
});
```

---
