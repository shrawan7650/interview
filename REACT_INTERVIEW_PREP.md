# React.js Interview Preparation Guide
**From Beginner to Advanced - Complete Mastery**

---

## Table of Contents
1. [Beginner Level - Fundamentals](#beginner-level)
2. [Intermediate Level - Core Concepts](#intermediate-level)
3. [Advanced Level - Expert Topics](#advanced-level)
4. [Coding Interview Questions](#coding-questions)
5. [Tricky Interview Questions](#tricky-questions)
6. [Real-World Scenarios](#real-world-scenarios)

---

## BEGINNER LEVEL - Fundamentals

### Q1: What is React?
**Answer:** React is a JavaScript library for building user interfaces, developed by Facebook.

**Deep Explanation:**
- It's a **library, not a framework** (doesn't enforce structure like Angular)
- Focuses on the **view layer** (the V in MVC)
- Uses a **component-based architecture** (UI broken into reusable pieces)
- Employs a **declarative approach** (you describe what UI should look like, React handles updates)
- **Why interviewers ask:** They want to see if you understand React's philosophy vs other tools

**Example:**
```javascript
// Declarative approach in React
function Welcome() {
  return <h1>Hello, World!</h1>;
}

// vs Imperative (vanilla JS)
const h1 = document.createElement('h1');
h1.textContent = 'Hello, World!';
document.body.appendChild(h1);
```

---

### Q2: What is JSX?
**Answer:** JSX is a syntax extension for JavaScript that looks like HTML but gets transformed into JavaScript function calls.

**Deep Explanation:**
- Stands for **JavaScript XML**
- Gets compiled to `React.createElement()` calls by Babel
- Makes code more readable than pure JavaScript
- Can embed JavaScript expressions using `{}`
- Must return a **single parent element**
- **Why interviewers ask:** To check if you understand JSX is syntactic sugar, not HTML

**Example:**
```javascript
// JSX
const element = <h1 className="greeting">Hello, {name}!</h1>;

// Compiles to:
const element = React.createElement(
  'h1',
  { className: 'greeting' },
  'Hello, ',
  name,
  '!'
);
```

**Key Points:**
- Use `className` not `class`
- Use `htmlFor` not `for`
- All tags must be closed: `<img />`, `<input />`
- JavaScript expressions in `{}`, not statements

---

### Q3: What is the Virtual DOM?
**Answer:** The Virtual DOM is a lightweight JavaScript representation of the actual DOM that React uses to optimize updates.

**Deep Explanation:**
- **Problem it solves:** Direct DOM manipulation is slow
- React keeps a virtual copy in memory
- When state changes, React creates a new virtual DOM
- **Diffing algorithm** compares old vs new virtual DOM
- Only **actual changes** are applied to real DOM (reconciliation)
- **Why interviewers ask:** To assess understanding of React's performance optimization

**How it works:**
1. State changes trigger re-render
2. New Virtual DOM tree is created
3. React "diffs" the two trees
4. Calculates minimal set of changes
5. Updates real DOM in batches

**Example Scenario:**
```javascript
// If you update one item in a list of 1000 items
// React only updates that ONE DOM node, not all 1000
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map(todo => <li key={todo.id}>{todo.text}</li>)}
    </ul>
  );
}
```

---

### Q4: What are Components?
**Answer:** Components are independent, reusable pieces of UI that can accept inputs (props) and return React elements.

**Deep Explanation:**
- **Two types:** Function components and Class components (modern React favors functions)
- Components let you split UI into independent pieces
- Each component has its own logic and rendering
- Can be composed together to build complex UIs
- **Why interviewers ask:** Components are React's building blocks

**Example:**
```javascript
// Function Component (Modern)
function Button({ onClick, children }) {
  return <button onClick={onClick}>{children}</button>;
}

// Class Component (Legacy)
class Button extends React.Component {
  render() {
    return <button onClick={this.props.onClick}>{this.props.children}</button>;
  }
}
```

**Best Practices:**
- Component names start with capital letter
- One component per file (usually)
- Keep components small and focused
- Extract reusable logic

---

### Q5: What are Props?
**Answer:** Props (properties) are read-only data passed from parent to child components.

**Deep Explanation:**
- **Unidirectional data flow** (parent → child only)
- Props are **immutable** in the receiving component
- Can pass any data type: strings, numbers, objects, functions
- Used to make components reusable
- **Why interviewers ask:** Props are fundamental to React's data flow

**Example:**
```javascript
// Parent passing props
function App() {
  const user = { name: 'John', age: 25 };

  return <UserProfile user={user} isActive={true} onUpdate={handleUpdate} />;
}

// Child receiving props
function UserProfile({ user, isActive, onUpdate }) {
  return (
    <div>
      <h2>{user.name}</h2>
      <p>Age: {user.age}</p>
      <p>Status: {isActive ? 'Active' : 'Inactive'}</p>
      <button onClick={onUpdate}>Update</button>
    </div>
  );
}
```

**Important:**
- Never modify props inside component
- Use destructuring for cleaner code
- Can set default props: `Button.defaultProps = { color: 'blue' }`

---

### Q6: What is State?
**Answer:** State is a built-in React object used to store data that changes over time and affects component rendering.

**Deep Explanation:**
- **Mutable** (unlike props)
- Local to the component
- When state changes, component re-renders
- Managed using `useState` hook in function components
- **Why interviewers ask:** State management is core to React interactivity

**Example:**
```javascript
import { useState } from 'react';

function Counter() {
  // Declare state variable
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(count - 1)}>Decrement</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
```

**Key Rules:**
- Never mutate state directly: `count++` ❌
- Always use setter function: `setCount(count + 1)` ✅
- State updates may be asynchronous
- Use functional updates for dependent state: `setCount(prev => prev + 1)`

---

### Q7: Props vs State - What's the difference?
**Answer:** Props are passed from parent and immutable; State is internal and mutable.

**Deep Explanation:**

| Props | State |
|-------|-------|
| Passed from parent | Defined inside component |
| Read-only | Can be changed |
| Cannot be modified | Modified using setState/setter |
| Functional parameters | Component memory |
| Cause re-render when changed in parent | Cause re-render when changed |

**Example:**
```javascript
function Parent() {
  const [parentState, setParentState] = useState('Hello');

  return <Child message={parentState} />; // message is a prop
}

function Child({ message }) {
  const [childState, setChildState] = useState(0); // childState is state

  return (
    <div>
      <p>{message}</p> {/* Cannot modify message here */}
      <p>{childState}</p>
      <button onClick={() => setChildState(childState + 1)}>Update</button>
    </div>
  );
}
```

---

### Q8: What are React Hooks?
**Answer:** Hooks are functions that let you use state and other React features in function components.

**Deep Explanation:**
- Introduced in React 16.8
- Allow function components to have state and lifecycle features
- **Replace class components** in modern React
- Must follow **Rules of Hooks**
- **Why interviewers ask:** Hooks are the modern React standard

**Common Built-in Hooks:**
- `useState` - Add state
- `useEffect` - Side effects
- `useContext` - Access context
- `useRef` - Access DOM or persist values
- `useMemo` - Memoize values
- `useCallback` - Memoize functions
- `useReducer` - Complex state logic

**Rules of Hooks:**
1. Only call hooks at the **top level** (not in loops, conditions, or nested functions)
2. Only call hooks from **React functions** (components or custom hooks)

**Example:**
```javascript
import { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(`/api/users/${userId}`)
      .then(res => res.json())
      .then(data => {
        setUser(data);
        setLoading(false);
      });
  }, [userId]); // Dependency array

  if (loading) return <p>Loading...</p>;
  return <div>{user.name}</div>;
}
```

---

### Q9: What is useState Hook?
**Answer:** useState is a hook that allows you to add state to function components.

**Deep Explanation:**
- Returns array with 2 elements: `[currentState, setterFunction]`
- Initial state passed as argument
- Setter function triggers re-render
- Can hold any data type
- **Why interviewers ask:** Most commonly used hook

**Example:**
```javascript
import { useState } from 'react';

function Form() {
  // Simple state
  const [name, setName] = useState('');

  // Object state
  const [user, setUser] = useState({ name: '', email: '' });

  // Array state
  const [items, setItems] = useState([]);

  // Updating object state (must spread)
  const updateEmail = (email) => {
    setUser(prev => ({ ...prev, email }));
  };

  // Updating array state
  const addItem = (item) => {
    setItems(prev => [...prev, item]);
  };

  return (
    <form>
      <input value={name} onChange={(e) => setName(e.target.value)} />
    </form>
  );
}
```

**Common Patterns:**
```javascript
// Lazy initialization (expensive computation)
const [state, setState] = useState(() => {
  return expensiveComputation();
});

// Functional updates (when new state depends on old)
setCount(prev => prev + 1);

// Resetting state
const reset = () => setState(initialState);
```

---

### Q10: What is useEffect Hook?
**Answer:** useEffect is a hook for performing side effects in function components (data fetching, subscriptions, DOM manipulation).

**Deep Explanation:**
- Runs **after render** (doesn't block painting)
- Replaces `componentDidMount`, `componentDidUpdate`, `componentWillUnmount`
- Can return cleanup function
- Controlled by **dependency array**
- **Why interviewers ask:** Essential for async operations and side effects

**Dependency Array Patterns:**
```javascript
// Run once on mount (like componentDidMount)
useEffect(() => {
  console.log('Mounted');
}, []); // Empty array

// Run on every render
useEffect(() => {
  console.log('Every render');
}); // No array

// Run when specific values change
useEffect(() => {
  console.log('userId changed');
}, [userId]); // Dependency array

// Cleanup function (like componentWillUnmount)
useEffect(() => {
  const timer = setInterval(() => console.log('tick'), 1000);

  return () => clearInterval(timer); // Cleanup
}, []);
```

**Common Use Cases:**
```javascript
// 1. Data Fetching
useEffect(() => {
  async function fetchData() {
    const response = await fetch('/api/data');
    const data = await response.json();
    setData(data);
  }
  fetchData();
}, []);

// 2. Event Listeners
useEffect(() => {
  const handleResize = () => setWidth(window.innerWidth);
  window.addEventListener('resize', handleResize);

  return () => window.removeEventListener('resize', handleResize);
}, []);

// 3. Document Title
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]);
```

---

## INTERMEDIATE LEVEL - Core Concepts

### Q11: What is the Component Lifecycle?
**Answer:** Component lifecycle refers to the series of phases a component goes through from creation to removal from the DOM.

**Deep Explanation:**
- **In Class Components:** mount → update → unmount
- **In Function Components:** Handled by hooks (mainly useEffect)
- Understanding lifecycle helps manage side effects properly
- **Why interviewers ask:** To assess understanding of when code executes

**Class Component Lifecycle:**
1. **Mounting:** constructor → render → componentDidMount
2. **Updating:** render → componentDidUpdate
3. **Unmounting:** componentWillUnmount

**Function Component Equivalent:**
```javascript
function MyComponent() {
  // componentDidMount
  useEffect(() => {
    console.log('Component mounted');
  }, []);

  // componentDidUpdate (when prop/state changes)
  useEffect(() => {
    console.log('userId updated');
  }, [userId]);

  // componentWillUnmount
  useEffect(() => {
    return () => {
      console.log('Component will unmount');
    };
  }, []);

  return <div>Content</div>;
}
```

**Interview Tip:** Always mention that modern React prefers hooks over lifecycle methods.

---

### Q12: What is useRef Hook?
**Answer:** useRef creates a mutable reference that persists across renders without causing re-renders when changed.

**Deep Explanation:**
- Returns object with `.current` property
- Changes to `.current` don't trigger re-render
- **Two main uses:** DOM access and storing mutable values
- Persists between renders
- **Why interviewers ask:** Tests understanding of refs vs state

**Example:**
```javascript
import { useRef, useEffect } from 'react';

function TextInput() {
  const inputRef = useRef(null);
  const countRef = useRef(0);

  useEffect(() => {
    // Focus input on mount
    inputRef.current.focus();
  }, []);

  const handleClick = () => {
    // Access DOM element
    console.log(inputRef.current.value);

    // Store value without re-render
    countRef.current += 1;
    console.log('Clicked:', countRef.current);
  };

  return (
    <div>
      <input ref={inputRef} />
      <button onClick={handleClick}>Submit</button>
    </div>
  );
}
```

**useRef vs useState:**
- `useRef`: No re-render on change, access DOM
- `useState`: Re-renders on change, manage UI state

---

### Q13: What is Context API?
**Answer:** Context API provides a way to pass data through the component tree without manually passing props at every level.

**Deep Explanation:**
- Solves **prop drilling** problem
- Shares data globally (themes, auth, language)
- Creates Provider and Consumer
- Modern approach uses `useContext` hook
- **Why interviewers ask:** Important for state management

**Example:**
```javascript
import { createContext, useContext, useState } from 'react';

// Create Context
const ThemeContext = createContext();

// Provider Component
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// Consumer Component
function ThemedButton() {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <button
      style={{ background: theme === 'light' ? '#fff' : '#333' }}
      onClick={toggleTheme}
    >
      Toggle Theme
    </button>
  );
}

// App
function App() {
  return (
    <ThemeProvider>
      <ThemedButton />
    </ThemeProvider>
  );
}
```

**When to Use:**
- Global settings (theme, language)
- User authentication
- Shopping cart state
- Avoid for frequently changing data (performance)

---

### Q14: What is useMemo Hook?
**Answer:** useMemo memoizes expensive computations and only recalculates when dependencies change.

**Deep Explanation:**
- **Performance optimization** hook
- Caches computed value
- Recalculates only when dependencies change
- Prevents unnecessary expensive operations
- **Why interviewers ask:** Tests optimization knowledge

**Example:**
```javascript
import { useState, useMemo } from 'react';

function ExpensiveComponent({ items }) {
  const [filter, setFilter] = useState('');

  // Without useMemo: recalculates on every render
  // With useMemo: only when items or filter change
  const filteredItems = useMemo(() => {
    console.log('Filtering items...');
    return items.filter(item =>
      item.name.toLowerCase().includes(filter.toLowerCase())
    );
  }, [items, filter]);

  return (
    <div>
      <input value={filter} onChange={(e) => setFilter(e.target.value)} />
      {filteredItems.map(item => <div key={item.id}>{item.name}</div>)}
    </div>
  );
}
```

**When to Use:**
- Expensive calculations
- Referential equality matters
- Processing large datasets
- Not for simple operations (overhead not worth it)

---

### Q15: What is useCallback Hook?
**Answer:** useCallback memoizes function references to prevent unnecessary re-renders of child components.

**Deep Explanation:**
- Returns **memoized callback function**
- Prevents function recreation on every render
- Useful when passing callbacks to optimized child components
- Dependencies control when function is recreated
- **Why interviewers ask:** Tests understanding of re-render optimization

**Example:**
```javascript
import { useState, useCallback, memo } from 'react';

// Child component wrapped in React.memo
const ChildComponent = memo(({ onClick }) => {
  console.log('Child rendered');
  return <button onClick={onClick}>Click</button>;
});

function ParentComponent() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');

  // Without useCallback: new function on every render
  // Child re-renders even when count doesn't change

  // With useCallback: same function reference unless count changes
  const handleClick = useCallback(() => {
    console.log('Clicked:', count);
  }, [count]);

  return (
    <div>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <p>Count: {count}</p>
      <ChildComponent onClick={handleClick} />
    </div>
  );
}
```

**useMemo vs useCallback:**
```javascript
// useMemo returns memoized VALUE
const memoizedValue = useMemo(() => computeExpensive(), [dep]);

// useCallback returns memoized FUNCTION
const memoizedCallback = useCallback(() => doSomething(), [dep]);

// These are equivalent:
useCallback(fn, deps) === useMemo(() => fn, deps)
```

---

### Q16: What is React.memo?
**Answer:** React.memo is a higher-order component that memoizes component output to prevent unnecessary re-renders.

**Deep Explanation:**
- **Performance optimization** for function components
- Shallow compares props
- Only re-renders if props change
- Similar to PureComponent for classes
- **Why interviewers ask:** Essential for optimization

**Example:**
```javascript
import { memo, useState } from 'react';

// Without memo: re-renders every time parent renders
function ExpensiveComponent({ value }) {
  console.log('Rendering ExpensiveComponent');
  return <div>{value}</div>;
}

// With memo: only re-renders when value prop changes
const MemoizedComponent = memo(ExpensiveComponent);

// Custom comparison function
const MemoizedWithCustomCompare = memo(ExpensiveComponent, (prevProps, nextProps) => {
  // Return true if props are equal (don't re-render)
  return prevProps.value === nextProps.value;
});

function App() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');

  return (
    <div>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <button onClick={() => setCount(c => c + 1)}>Increment</button>
      <MemoizedComponent value={count} />
      {/* Doesn't re-render when text changes */}
    </div>
  );
}
```

**When to Use:**
- Component renders often with same props
- Component is expensive to render
- Pure functional component (same props = same output)

---

### Q17: What is useReducer Hook?
**Answer:** useReducer is a hook for managing complex state logic using a reducer function, similar to Redux.

**Deep Explanation:**
- Alternative to useState for **complex state logic**
- Takes reducer function and initial state
- Returns current state and dispatch function
- Better for multiple sub-values or next state depends on previous
- **Why interviewers ask:** Shows advanced state management understanding

**Example:**
```javascript
import { useReducer } from 'react';

// Reducer function
function counterReducer(state, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    case 'RESET':
      return { count: 0 };
    case 'SET':
      return { count: action.payload };
    default:
      throw new Error('Unknown action type');
  }
}

function Counter() {
  const [state, dispatch] = useReducer(counterReducer, { count: 0 });

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>+</button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>-</button>
      <button onClick={() => dispatch({ type: 'RESET' })}>Reset</button>
      <button onClick={() => dispatch({ type: 'SET', payload: 10 })}>Set 10</button>
    </div>
  );
}
```

**Complex Example (Form State):**
```javascript
const formReducer = (state, action) => {
  switch (action.type) {
    case 'UPDATE_FIELD':
      return { ...state, [action.field]: action.value };
    case 'RESET':
      return action.initialState;
    default:
      return state;
  }
};

function FormComponent() {
  const initialState = { name: '', email: '', age: '' };
  const [state, dispatch] = useReducer(formReducer, initialState);

  const handleChange = (field) => (e) => {
    dispatch({ type: 'UPDATE_FIELD', field, value: e.target.value });
  };

  return (
    <form>
      <input value={state.name} onChange={handleChange('name')} />
      <input value={state.email} onChange={handleChange('email')} />
      <input value={state.age} onChange={handleChange('age')} />
      <button onClick={() => dispatch({ type: 'RESET', initialState })}>Reset</button>
    </form>
  );
}
```

**useState vs useReducer:**
- Simple state → `useState`
- Complex state with multiple related values → `useReducer`
- State transitions depend on previous state → `useReducer`

---

### Q18: What is React Router?
**Answer:** React Router is a library for handling routing and navigation in React applications.

**Deep Explanation:**
- Enables **Single Page Application** (SPA) navigation
- Renders components based on URL
- Client-side routing (no page reload)
- v6 is the latest version
- **Why interviewers ask:** Essential for multi-page React apps

**Example:**
```javascript
import { BrowserRouter, Routes, Route, Link, useParams, useNavigate } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/users/123">User Profile</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/users/:id" element={<UserProfile />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}

// Access URL params
function UserProfile() {
  const { id } = useParams();
  return <div>User ID: {id}</div>;
}

// Programmatic navigation
function LoginForm() {
  const navigate = useNavigate();

  const handleLogin = () => {
    // After successful login
    navigate('/dashboard');
  };

  return <button onClick={handleLogin}>Login</button>;
}
```

**Key Concepts:**
- `BrowserRouter`: Wraps app
- `Routes`: Contains all routes
- `Route`: Defines path and component
- `Link`: Navigation without reload
- `useParams`: Access URL parameters
- `useNavigate`: Programmatic navigation
- `useLocation`: Current location object

---

### Q19: What are Controlled vs Uncontrolled Components?
**Answer:** Controlled components have form data handled by React state; Uncontrolled components use refs to access DOM values.

**Deep Explanation:**
- **Controlled:** React controls input value (single source of truth)
- **Uncontrolled:** DOM controls input value (accessed via refs)
- Controlled is recommended approach
- **Why interviewers ask:** Tests form handling knowledge

**Controlled Component:**
```javascript
function ControlledForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log({ email, password });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <input
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Uncontrolled Component:**
```javascript
function UncontrolledForm() {
  const emailRef = useRef();
  const passwordRef = useRef();

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log({
      email: emailRef.current.value,
      password: passwordRef.current.value
    });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="email" ref={emailRef} defaultValue="" />
      <input type="password" ref={passwordRef} defaultValue="" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**When to Use:**
- **Controlled:** Form validation, dynamic forms, instant feedback
- **Uncontrolled:** File inputs, integration with non-React code, simple forms

---

### Q20: What are Higher-Order Components (HOC)?
**Answer:** HOCs are functions that take a component and return a new enhanced component with additional props or behavior.

**Deep Explanation:**
- **Design pattern** for reusing component logic
- Function that returns a component
- Used for cross-cutting concerns (auth, logging, theming)
- Common convention: prefix with "with" (withAuth, withTheme)
- **Why interviewers ask:** Tests advanced patterns knowledge

**Example:**
```javascript
// HOC that adds loading behavior
function withLoading(Component) {
  return function WithLoadingComponent({ isLoading, ...props }) {
    if (isLoading) {
      return <div>Loading...</div>;
    }
    return <Component {...props} />;
  };
}

// Original component
function UserList({ users }) {
  return (
    <ul>
      {users.map(user => <li key={user.id}>{user.name}</li>)}
    </ul>
  );
}

// Enhanced component
const UserListWithLoading = withLoading(UserList);

// Usage
function App() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  return <UserListWithLoading isLoading={loading} users={users} />;
}
```

**Authentication HOC:**
```javascript
function withAuth(Component) {
  return function AuthenticatedComponent(props) {
    const { isAuthenticated } = useAuth();

    if (!isAuthenticated) {
      return <Navigate to="/login" />;
    }

    return <Component {...props} />;
  };
}

const ProtectedDashboard = withAuth(Dashboard);
```

**Modern Alternative:** Custom hooks are now preferred over HOCs in many cases.

---

## ADVANCED LEVEL - Expert Topics

### Q21: What is Redux and when to use it?
**Answer:** Redux is a predictable state management library that centralizes application state in a single store.

**Deep Explanation:**
- **Three core principles:** Single source of truth, state is read-only, changes via pure functions
- Components connect to store instead of prop drilling
- Better for large apps with complex state
- **Why interviewers ask:** Tests state management at scale

**Core Concepts:**
- **Store:** Holds entire app state
- **Actions:** Plain objects describing what happened
- **Reducers:** Pure functions that update state
- **Dispatch:** Send actions to store
- **Selectors:** Extract data from store

**Example:**
```javascript
// Action Types
const INCREMENT = 'INCREMENT';
const DECREMENT = 'DECREMENT';

// Action Creators
const increment = () => ({ type: INCREMENT });
const decrement = () => ({ type: DECREMENT });

// Reducer
function counterReducer(state = { count: 0 }, action) {
  switch (action.type) {
    case INCREMENT:
      return { count: state.count + 1 };
    case DECREMENT:
      return { count: state.count - 1 };
    default:
      return state;
  }
}

// Store
import { createStore } from 'redux';
const store = createStore(counterReducer);

// Component
import { useSelector, useDispatch } from 'react-redux';

function Counter() {
  const count = useSelector(state => state.count);
  const dispatch = useDispatch();

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
    </div>
  );
}
```

**When to Use Redux:**
- Large amounts of app state
- State used in many places
- State updates frequently
- Complex state update logic
- Medium to large codebase

**Alternatives:** Context API (simpler), Zustand, Recoil, Jotai

---

### Q22: What is Redux Toolkit (RTK)?
**Answer:** Redux Toolkit is the official, opinionated Redux library that simplifies Redux development.

**Deep Explanation:**
- **Modern Redux** approach (recommended by Redux team)
- Includes utilities to simplify store setup, reducers, and async logic
- Reduces boilerplate significantly
- Built-in with Immer for immutable updates
- **Why interviewers ask:** RTK is now the standard way to use Redux

**Example:**
```javascript
import { configureStore, createSlice } from '@reduxjs/toolkit';

// Slice (combines actions + reducer)
const counterSlice = createSlice({
  name: 'counter',
  initialState: { count: 0 },
  reducers: {
    increment: (state) => {
      state.count += 1; // Immer allows "mutation"
    },
    decrement: (state) => {
      state.count -= 1;
    },
    incrementByAmount: (state, action) => {
      state.count += action.payload;
    }
  }
});

// Export actions
export const { increment, decrement, incrementByAmount } = counterSlice.actions;

// Create store
const store = configureStore({
  reducer: {
    counter: counterSlice.reducer
  }
});

// Component
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement } from './counterSlice';

function Counter() {
  const count = useSelector(state => state.counter.count);
  const dispatch = useDispatch();

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
    </div>
  );
}
```

**Async Example (createAsyncThunk):**
```javascript
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

// Async action
export const fetchUsers = createAsyncThunk('users/fetch', async () => {
  const response = await fetch('/api/users');
  return response.json();
});

const usersSlice = createSlice({
  name: 'users',
  initialState: { data: [], loading: false, error: null },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchUsers.pending, (state) => {
        state.loading = true;
      })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.loading = false;
        state.data = action.payload;
      })
      .addCase(fetchUsers.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message;
      });
  }
});
```

---

### Q23: What is React.lazy and Suspense?
**Answer:** React.lazy enables code splitting by lazily loading components, and Suspense handles loading states for lazy components.

**Deep Explanation:**
- **Code splitting:** Load components only when needed
- Reduces initial bundle size
- Improves performance
- Suspense shows fallback while loading
- **Why interviewers ask:** Tests performance optimization knowledge

**Example:**
```javascript
import { lazy, Suspense } from 'react';

// Lazy load component
const HeavyComponent = lazy(() => import('./HeavyComponent'));

function App() {
  return (
    <div>
      <h1>My App</h1>

      {/* Suspense with fallback */}
      <Suspense fallback={<div>Loading...</div>}>
        <HeavyComponent />
      </Suspense>
    </div>
  );
}
```

**Route-based Code Splitting:**
```javascript
import { lazy, Suspense } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';

const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
const Dashboard = lazy(() => import('./pages/Dashboard'));

function App() {
  return (
    <BrowserRouter>
      <Suspense fallback={<div>Loading page...</div>}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
          <Route path="/dashboard" element={<Dashboard />} />
        </Routes>
      </Suspense>
    </BrowserRouter>
  );
}
```

**Benefits:**
- Faster initial load
- Load features on demand
- Better user experience
- Optimized bundle size

---

### Q24: What are Error Boundaries?
**Answer:** Error Boundaries are React components that catch JavaScript errors in child component tree and display fallback UI.

**Deep Explanation:**
- **Catch errors** during rendering, lifecycle methods, and constructors
- Prevent entire app from crashing
- Must be **class components** (no hook equivalent yet)
- Can log errors to error reporting services
- **Why interviewers ask:** Tests error handling knowledge

**Example:**
```javascript
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    // Update state to show fallback UI
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // Log error to service
    console.error('Error caught:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return (
        <div>
          <h2>Something went wrong</h2>
          <button onClick={() => this.setState({ hasError: false })}>
            Try again
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}

// Usage
function App() {
  return (
    <ErrorBoundary>
      <MyComponent />
    </ErrorBoundary>
  );
}
```

**What Error Boundaries DON'T Catch:**
- Event handlers (use try-catch)
- Async code (setTimeout, promises)
- Server-side rendering
- Errors in error boundary itself

---

### Q25: What is Server-Side Rendering (SSR)?
**Answer:** SSR renders React components on the server and sends HTML to client, improving initial load and SEO.

**Deep Explanation:**
- **Pre-renders** pages on server
- Client receives full HTML (faster first paint)
- Better for **SEO** (search engines see content)
- Next.js is popular SSR framework for React
- **Why interviewers ask:** Important for production apps

**CSR vs SSR:**

**Client-Side Rendering (CSR):**
1. Server sends minimal HTML + JS bundle
2. Browser downloads JS
3. React renders content
4. User sees content
5. **Problem:** Slow initial load, poor SEO

**Server-Side Rendering (SSR):**
1. Server renders React to HTML
2. Browser receives full HTML
3. User sees content immediately
4. React hydrates (attaches events)
5. **Benefit:** Fast initial load, good SEO

**Next.js Example:**
```javascript
// pages/index.js

// This function runs on server
export async function getServerSideProps() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return {
    props: { data } // Passed to component
  };
}

// Component receives props from server
function HomePage({ data }) {
  return (
    <div>
      <h1>Server-Rendered Page</h1>
      <p>{data.message}</p>
    </div>
  );
}

export default HomePage;
```

**Rendering Strategies:**
- **SSR:** Render on each request (dynamic data)
- **SSG (Static Site Generation):** Pre-render at build time (static content)
- **ISR (Incremental Static Regeneration):** Rebuild pages periodically
- **CSR:** Render in browser (interactive apps)

---

### Q26: What is useLayoutEffect?
**Answer:** useLayoutEffect is similar to useEffect but fires synchronously after DOM mutations, before browser paint.

**Deep Explanation:**
- Runs **before browser paints screen**
- Use for DOM measurements or synchronous updates
- Can cause visual flicker if overused
- Most cases should use useEffect
- **Why interviewers ask:** Tests understanding of rendering lifecycle

**Example:**
```javascript
import { useLayoutEffect, useRef, useState } from 'react';

function Tooltip() {
  const [tooltipHeight, setTooltipHeight] = useState(0);
  const tooltipRef = useRef(null);

  // useLayoutEffect: measure DOM before paint
  useLayoutEffect(() => {
    const height = tooltipRef.current.getBoundingClientRect().height;
    setTooltipHeight(height);
  }, []);

  // useEffect would cause flicker here

  return (
    <div ref={tooltipRef} style={{ marginTop: -tooltipHeight }}>
      Tooltip content
    </div>
  );
}
```

**useEffect vs useLayoutEffect:**
- `useEffect`: After paint (most cases)
- `useLayoutEffect`: Before paint (DOM measurements, avoiding flicker)

---

### Q27: What is Custom Hooks?
**Answer:** Custom hooks are reusable functions that encapsulate stateful logic and can use other hooks.

**Deep Explanation:**
- Start with "use" prefix
- Extract common logic
- Can use built-in hooks inside
- Promote code reuse
- **Why interviewers ask:** Tests ability to write reusable code

**Example 1: useFetch**
```javascript
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    async function fetchData() {
      try {
        setLoading(true);
        const response = await fetch(url);
        const json = await response.json();
        setData(json);
      } catch (err) {
        setError(err);
      } finally {
        setLoading(false);
      }
    }

    fetchData();
  }, [url]);

  return { data, loading, error };
}

// Usage
function UserProfile({ userId }) {
  const { data, loading, error } = useFetch(`/api/users/${userId}`);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;
  return <div>{data.name}</div>;
}
```

**Example 2: useLocalStorage**
```javascript
import { useState, useEffect } from 'react';

function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    const item = localStorage.getItem(key);
    return item ? JSON.parse(item) : initialValue;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  return [value, setValue];
}

// Usage
function Settings() {
  const [theme, setTheme] = useLocalStorage('theme', 'light');

  return (
    <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
      Current: {theme}
    </button>
  );
}
```

**Common Custom Hooks:**
- useAuth (authentication logic)
- useForm (form handling)
- useDebounce (debounce values)
- useWindowSize (window dimensions)
- usePrevious (track previous value)

---

### Q28: What is React Testing?
**Answer:** React testing involves writing tests to verify components render correctly and behave as expected.

**Deep Explanation:**
- **Testing libraries:** Jest (test runner) + React Testing Library (utilities)
- Test user interactions and outputs, not implementation
- Types: Unit tests, integration tests, E2E tests
- **Why interviewers ask:** Tests are crucial for production apps

**Example:**
```javascript
import { render, screen, fireEvent } from '@testing-library/react';
import Counter from './Counter';

describe('Counter Component', () => {
  test('renders initial count', () => {
    render(<Counter />);
    expect(screen.getByText('Count: 0')).toBeInTheDocument();
  });

  test('increments count on button click', () => {
    render(<Counter />);
    const button = screen.getByText('Increment');
    fireEvent.click(button);
    expect(screen.getByText('Count: 1')).toBeInTheDocument();
  });

  test('handles multiple clicks', () => {
    render(<Counter />);
    const button = screen.getByText('Increment');
    fireEvent.click(button);
    fireEvent.click(button);
    fireEvent.click(button);
    expect(screen.getByText('Count: 3')).toBeInTheDocument();
  });
});
```

**Testing Async Code:**
```javascript
import { render, screen, waitFor } from '@testing-library/react';
import UserProfile from './UserProfile';

test('loads and displays user data', async () => {
  render(<UserProfile userId="123" />);

  // Shows loading state
  expect(screen.getByText('Loading...')).toBeInTheDocument();

  // Wait for data to load
  await waitFor(() => {
    expect(screen.getByText('John Doe')).toBeInTheDocument();
  });
});
```

**Best Practices:**
- Test behavior, not implementation
- Use `getByRole` over `getByTestId`
- Mock external dependencies
- Test user interactions
- Keep tests simple and readable

---

### Q29: What is React Performance Optimization?
**Answer:** Performance optimization involves techniques to minimize unnecessary renders and improve app speed.

**Deep Explanation:**
- React is fast, but large apps need optimization
- Key metrics: First Contentful Paint, Time to Interactive
- Tools: React DevTools Profiler, Lighthouse
- **Why interviewers ask:** Essential for production apps

**Optimization Techniques:**

**1. Memoization:**
```javascript
// React.memo for components
const ExpensiveComponent = React.memo(({ data }) => {
  return <div>{data.name}</div>;
});

// useMemo for values
const sortedList = useMemo(() => {
  return items.sort((a, b) => a.value - b.value);
}, [items]);

// useCallback for functions
const handleClick = useCallback(() => {
  console.log('clicked');
}, []);
```

**2. Code Splitting:**
```javascript
const HeavyComponent = lazy(() => import('./HeavyComponent'));

<Suspense fallback={<Loading />}>
  <HeavyComponent />
</Suspense>
```

**3. Virtualization (Long Lists):**
```javascript
import { FixedSizeList } from 'react-window';

function VirtualList({ items }) {
  return (
    <FixedSizeList
      height={600}
      itemCount={items.length}
      itemSize={50}
      width="100%"
    >
      {({ index, style }) => (
        <div style={style}>{items[index].name}</div>
      )}
    </FixedSizeList>
  );
}
```

**4. Debouncing:**
```javascript
function SearchInput() {
  const [query, setQuery] = useState('');

  const debouncedSearch = useMemo(
    () => debounce((value) => {
      // Expensive search operation
      searchAPI(value);
    }, 500),
    []
  );

  const handleChange = (e) => {
    setQuery(e.target.value);
    debouncedSearch(e.target.value);
  };

  return <input value={query} onChange={handleChange} />;
}
```

**5. Avoid Inline Functions/Objects:**
```javascript
// Bad: Creates new function on every render
<button onClick={() => handleClick(id)}>Click</button>

// Good: Memoized function
const onClick = useCallback(() => handleClick(id), [id]);
<button onClick={onClick}>Click</button>
```

---

### Q30: What is Reconciliation and Diffing Algorithm?
**Answer:** Reconciliation is the process React uses to update the DOM efficiently by comparing Virtual DOM trees using a diffing algorithm.

**Deep Explanation:**
- React compares old and new Virtual DOM
- **Diffing algorithm** finds minimal changes
- Uses heuristics for O(n) complexity
- Key prop helps identify elements
- **Why interviewers ask:** Core to React's performance

**How Diffing Works:**

**1. Different Element Types:**
```javascript
// Old: <div><Counter /></div>
// New: <span><Counter /></span>
// Result: Destroys old tree, builds new one
```

**2. Same Element Type:**
```javascript
// Old: <div className="old" />
// New: <div className="new" />
// Result: Updates className attribute only
```

**3. Recursion on Children:**
```javascript
// Old: <ul><li>A</li><li>B</li></ul>
// New: <ul><li>A</li><li>B</li><li>C</li></ul>
// Result: Keeps A and B, adds C
```

**4. Keys Importance:**
```javascript
// Without keys: React re-renders all items
<ul>
  {items.map(item => <li>{item.name}</li>)}
</ul>

// With keys: React updates only changed items
<ul>
  {items.map(item => <li key={item.id}>{item.name}</li>)}
</ul>
```

**Key Rules:**
- Keys should be **stable** (not array index)
- Keys should be **unique** among siblings
- Keys help React identify which items changed
- Never use index as key for dynamic lists

---

## CODING QUESTIONS - Practical Implementation

### Coding Q1: Build a Todo App with CRUD operations

**Requirements:**
- Add new todo
- Mark todo as complete
- Delete todo
- Filter (all/active/completed)

**Solution:**
```javascript
import { useState } from 'react';

function TodoApp() {
  const [todos, setTodos] = useState([]);
  const [input, setInput] = useState('');
  const [filter, setFilter] = useState('all');

  const addTodo = () => {
    if (input.trim()) {
      setTodos([...todos, { id: Date.now(), text: input, completed: false }]);
      setInput('');
    }
  };

  const toggleTodo = (id) => {
    setTodos(todos.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
  };

  const deleteTodo = (id) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };

  const filteredTodos = todos.filter(todo => {
    if (filter === 'active') return !todo.completed;
    if (filter === 'completed') return todo.completed;
    return true;
  });

  return (
    <div>
      <h1>Todo App</h1>
      <input
        value={input}
        onChange={(e) => setInput(e.target.value)}
        onKeyPress={(e) => e.key === 'Enter' && addTodo()}
      />
      <button onClick={addTodo}>Add</button>

      <div>
        <button onClick={() => setFilter('all')}>All</button>
        <button onClick={() => setFilter('active')}>Active</button>
        <button onClick={() => setFilter('completed')}>Completed</button>
      </div>

      <ul>
        {filteredTodos.map(todo => (
          <li key={todo.id}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => toggleTodo(todo.id)}
            />
            <span style={{ textDecoration: todo.completed ? 'line-through' : 'none' }}>
              {todo.text}
            </span>
            <button onClick={() => deleteTodo(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

### Coding Q2: Implement Debounced Search

**Requirements:**
- Search input with API call
- Debounce API calls (wait 500ms after typing stops)
- Show loading state

**Solution:**
```javascript
import { useState, useEffect } from 'react';

function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => clearTimeout(handler);
  }, [value, delay]);

  return debouncedValue;
}

function SearchComponent() {
  const [searchTerm, setSearchTerm] = useState('');
  const [results, setResults] = useState([]);
  const [loading, setLoading] = useState(false);

  const debouncedSearchTerm = useDebounce(searchTerm, 500);

  useEffect(() => {
    if (debouncedSearchTerm) {
      setLoading(true);
      fetch(`/api/search?q=${debouncedSearchTerm}`)
        .then(res => res.json())
        .then(data => {
          setResults(data);
          setLoading(false);
        });
    } else {
      setResults([]);
    }
  }, [debouncedSearchTerm]);

  return (
    <div>
      <input
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search..."
      />
      {loading && <p>Loading...</p>}
      <ul>
        {results.map(item => <li key={item.id}>{item.name}</li>)}
      </ul>
    </div>
  );
}
```

---

### Coding Q3: Build Infinite Scroll

**Requirements:**
- Load 20 items initially
- Load more when scrolling to bottom
- Show loading indicator

**Solution:**
```javascript
import { useState, useEffect, useRef } from 'react';

function InfiniteScroll() {
  const [items, setItems] = useState([]);
  const [page, setPage] = useState(1);
  const [loading, setLoading] = useState(false);
  const [hasMore, setHasMore] = useState(true);
  const loader = useRef(null);

  useEffect(() => {
    loadItems();
  }, [page]);

  useEffect(() => {
    const observer = new IntersectionObserver((entries) => {
      if (entries[0].isIntersecting && hasMore && !loading) {
        setPage(prev => prev + 1);
      }
    });

    if (loader.current) {
      observer.observe(loader.current);
    }

    return () => observer.disconnect();
  }, [hasMore, loading]);

  const loadItems = async () => {
    setLoading(true);
    const response = await fetch(`/api/items?page=${page}&limit=20`);
    const newItems = await response.json();

    setItems(prev => [...prev, ...newItems]);
    setHasMore(newItems.length > 0);
    setLoading(false);
  };

  return (
    <div>
      <ul>
        {items.map(item => <li key={item.id}>{item.name}</li>)}
      </ul>
      {loading && <p>Loading...</p>}
      <div ref={loader} />
    </div>
  );
}
```

---

### Coding Q4: Create Modal Component

**Requirements:**
- Open/close modal
- Close on outside click
- Close on Escape key
- Prevent body scroll when open

**Solution:**
```javascript
import { useEffect } from 'react';
import { createPortal } from 'react-dom';

function Modal({ isOpen, onClose, children }) {
  useEffect(() => {
    if (isOpen) {
      document.body.style.overflow = 'hidden';
    } else {
      document.body.style.overflow = 'unset';
    }

    return () => {
      document.body.style.overflow = 'unset';
    };
  }, [isOpen]);

  useEffect(() => {
    const handleEscape = (e) => {
      if (e.key === 'Escape') onClose();
    };

    if (isOpen) {
      document.addEventListener('keydown', handleEscape);
    }

    return () => document.removeEventListener('keydown', handleEscape);
  }, [isOpen, onClose]);

  if (!isOpen) return null;

  return createPortal(
    <div
      className="modal-overlay"
      onClick={onClose}
      style={{
        position: 'fixed',
        top: 0,
        left: 0,
        right: 0,
        bottom: 0,
        backgroundColor: 'rgba(0,0,0,0.5)',
        display: 'flex',
        alignItems: 'center',
        justifyContent: 'center'
      }}
    >
      <div
        className="modal-content"
        onClick={(e) => e.stopPropagation()}
        style={{
          backgroundColor: 'white',
          padding: '20px',
          borderRadius: '8px',
          maxWidth: '500px',
          width: '90%'
        }}
      >
        {children}
        <button onClick={onClose}>Close</button>
      </div>
    </div>,
    document.body
  );
}

// Usage
function App() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div>
      <button onClick={() => setIsOpen(true)}>Open Modal</button>
      <Modal isOpen={isOpen} onClose={() => setIsOpen(false)}>
        <h2>Modal Title</h2>
        <p>Modal content here</p>
      </Modal>
    </div>
  );
}
```

---

### Coding Q5: Implement Form Validation

**Requirements:**
- Validate email, password, confirm password
- Show errors on blur
- Disable submit if invalid

**Solution:**
```javascript
import { useState } from 'react';

function RegistrationForm() {
  const [formData, setFormData] = useState({
    email: '',
    password: '',
    confirmPassword: ''
  });

  const [errors, setErrors] = useState({});
  const [touched, setTouched] = useState({});

  const validate = (name, value) => {
    switch (name) {
      case 'email':
        if (!value) return 'Email is required';
        if (!/\S+@\S+\.\S+/.test(value)) return 'Email is invalid';
        return '';
      case 'password':
        if (!value) return 'Password is required';
        if (value.length < 8) return 'Password must be at least 8 characters';
        return '';
      case 'confirmPassword':
        if (!value) return 'Please confirm password';
        if (value !== formData.password) return 'Passwords do not match';
        return '';
      default:
        return '';
    }
  };

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));

    if (touched[name]) {
      const error = validate(name, value);
      setErrors(prev => ({ ...prev, [name]: error }));
    }
  };

  const handleBlur = (e) => {
    const { name, value } = e.target;
    setTouched(prev => ({ ...prev, [name]: true }));
    const error = validate(name, value);
    setErrors(prev => ({ ...prev, [name]: error }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();

    const newErrors = {};
    Object.keys(formData).forEach(key => {
      const error = validate(key, formData[key]);
      if (error) newErrors[key] = error;
    });

    if (Object.keys(newErrors).length === 0) {
      console.log('Form submitted:', formData);
    } else {
      setErrors(newErrors);
      setTouched({ email: true, password: true, confirmPassword: true });
    }
  };

  const isValid = Object.values(errors).every(error => !error) &&
                  Object.values(formData).every(value => value);

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          name="email"
          type="email"
          placeholder="Email"
          value={formData.email}
          onChange={handleChange}
          onBlur={handleBlur}
        />
        {touched.email && errors.email && <span>{errors.email}</span>}
      </div>

      <div>
        <input
          name="password"
          type="password"
          placeholder="Password"
          value={formData.password}
          onChange={handleChange}
          onBlur={handleBlur}
        />
        {touched.password && errors.password && <span>{errors.password}</span>}
      </div>

      <div>
        <input
          name="confirmPassword"
          type="password"
          placeholder="Confirm Password"
          value={formData.confirmPassword}
          onChange={handleChange}
          onBlur={handleBlur}
        />
        {touched.confirmPassword && errors.confirmPassword && (
          <span>{errors.confirmPassword}</span>
        )}
      </div>

      <button type="submit" disabled={!isValid}>Register</button>
    </form>
  );
}
```

---

## TRICKY QUESTIONS - Interview Gotchas

### Tricky Q1: Why is setState asynchronous?

**Interviewer's Expectation:** Test understanding of React's batching and performance.

**Answer:**
setState is asynchronous for performance optimization. React batches multiple state updates together to avoid unnecessary re-renders.

**Deep Explanation:**
```javascript
function Counter() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    // This won't work as expected
    setCount(count + 1);
    setCount(count + 1);
    setCount(count + 1);
    // count will be 1, not 3

    // Correct way: use functional updates
    setCount(c => c + 1);
    setCount(c => c + 1);
    setCount(c => c + 1);
    // count will be 3
  };

  return <button onClick={handleClick}>{count}</button>;
}
```

**Why it's async:**
- Batches updates for better performance
- Prevents unnecessary re-renders
- Allows React to optimize rendering
- In React 18: automatic batching everywhere

---

### Tricky Q2: What's the difference between useEffect and useLayoutEffect?

**Interviewer's Expectation:** Test knowledge of rendering phases.

**Answer:**
useEffect runs after paint (asynchronous), useLayoutEffect runs before paint (synchronous).

**Visual Difference:**
```javascript
// useEffect: User might see flicker
function WithUseEffect() {
  const [height, setHeight] = useState(0);

  useEffect(() => {
    setHeight(100); // Runs after paint, causes flicker
  }, []);

  return <div style={{ height }}></div>;
}

// useLayoutEffect: No flicker
function WithUseLayoutEffect() {
  const [height, setHeight] = useState(0);

  useLayoutEffect(() => {
    setHeight(100); // Runs before paint, no flicker
  }, []);

  return <div style={{ height }}></div>;
}
```

**When to use which:**
- useEffect: 99% of cases (side effects, data fetching)
- useLayoutEffect: DOM measurements, preventing visual flicker

---

### Tricky Q3: Why shouldn't you use array index as key?

**Interviewer's Expectation:** Test understanding of reconciliation.

**Answer:**
Using index as key can cause bugs when list order changes because React identifies elements by key.

**Problem Demonstration:**
```javascript
// Bad: Using index as key
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map((todo, index) => (
        <li key={index}>
          <input type="checkbox" />
          {todo.text}
        </li>
      ))}
    </ul>
  );
}

// Problem: If you delete first item,
// React thinks you deleted the last item and updated all others
// Checkbox states get messed up

// Good: Using unique ID
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>
          <input type="checkbox" />
          {todo.text}
        </li>
      ))}
    </ul>
  );
}
```

**When index is OK:**
- Static list (never changes)
- List is never reordered
- List has no state

---

### Tricky Q4: What's the difference between controlled and uncontrolled components?

**Interviewer's Expectation:** Test form handling knowledge.

**Answer:**
Controlled: React state controls input value. Uncontrolled: DOM controls input value.

**Comparison:**
```javascript
// Controlled: React is source of truth
function Controlled() {
  const [value, setValue] = useState('');

  return (
    <input
      value={value}
      onChange={(e) => setValue(e.target.value)}
    />
  );
}

// Uncontrolled: DOM is source of truth
function Uncontrolled() {
  const inputRef = useRef();

  const handleSubmit = () => {
    console.log(inputRef.current.value);
  };

  return <input ref={inputRef} defaultValue="" />;
}
```

---

### Tricky Q5: Explain closure problem in loops with useState

**Interviewer's Expectation:** Test understanding of closures and stale state.

**Answer:**
Event handlers capture state value at render time, causing stale closure problem.

**Problem:**
```javascript
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => {
      setCount(count + 1); // Always uses initial count (0)
    }, 1000);

    return () => clearInterval(timer);
  }, []); // Empty deps cause stale closure

  return <div>{count}</div>; // Stuck at 1
}

// Solution 1: Use functional update
useEffect(() => {
  const timer = setInterval(() => {
    setCount(c => c + 1); // Uses current count
  }, 1000);

  return () => clearInterval(timer);
}, []);

// Solution 2: Add to dependencies
useEffect(() => {
  const timer = setInterval(() => {
    setCount(count + 1);
  }, 1000);

  return () => clearInterval(timer);
}, [count]); // But this recreates interval
```

---

### Tricky Q6: What happens if you forget return in useEffect cleanup?

**Interviewer's Expectation:** Test understanding of memory leaks.

**Answer:**
Without cleanup, subscriptions/timers continue running, causing memory leaks.

**Example:**
```javascript
// Memory leak: Event listener never removed
function BadComponent() {
  useEffect(() => {
    window.addEventListener('scroll', handleScroll);
    // Missing cleanup!
  }, []);
}

// Correct: Cleanup function
function GoodComponent() {
  useEffect(() => {
    window.addEventListener('scroll', handleScroll);

    return () => {
      window.removeEventListener('scroll', handleScroll);
    };
  }, []);
}
```

---

### Tricky Q7: Can you modify props in a component?

**Interviewer's Expectation:** Test fundamental React principle.

**Answer:**
No, props are read-only. Modifying props violates React's unidirectional data flow.

**Explanation:**
```javascript
// Wrong: Never do this
function BadComponent({ user }) {
  user.name = 'New Name'; // Mutation!
  return <div>{user.name}</div>;
}

// Correct: Create new object
function GoodComponent({ user }) {
  const updatedUser = { ...user, name: 'New Name' };
  return <div>{updatedUser.name}</div>;
}

// Best: Lift state up if modification needed
function Parent() {
  const [user, setUser] = useState({ name: 'John' });

  return (
    <Child
      user={user}
      onUpdate={(newName) => setUser({ ...user, name: newName })}
    />
  );
}
```

---

### Tricky Q8: What's the difference between createElement and cloneElement?

**Interviewer's Expectation:** Test advanced React API knowledge.

**Answer:**
createElement creates new element. cloneElement copies existing element and can override props.

**Example:**
```javascript
// createElement: Create new element
const element = React.createElement('div', { className: 'box' }, 'Hello');

// cloneElement: Clone and modify
const original = <Button color="blue">Click</Button>;
const cloned = React.cloneElement(original, { color: 'red' });
// Result: <Button color="red">Click</Button>

// Use case: Wrapper component adding props to children
function Wrapper({ children }) {
  return React.Children.map(children, child => {
    return React.cloneElement(child, { className: 'wrapped' });
  });
}
```

---

### Tricky Q9: Why can't hooks be called conditionally?

**Interviewer's Expectation:** Test Rules of Hooks understanding.

**Answer:**
React relies on call order to track hook state. Conditional calls break this order.

**Explanation:**
```javascript
// Wrong: Conditional hook
function BadComponent({ condition }) {
  if (condition) {
    const [state, setState] = useState(0); // Breaks rules!
  }
}

// How React tracks hooks internally:
// Render 1: [hook1, hook2, hook3]
// Render 2: [hook1, hook3] // hook2 missing! Order broken!

// Correct: Conditional inside hook
function GoodComponent({ condition }) {
  const [state, setState] = useState(condition ? 0 : null);
}
```

---

### Tricky Q10: What's the difference between useMemo and useCallback?

**Interviewer's Expectation:** Test optimization hooks knowledge.

**Answer:**
useMemo memoizes return value. useCallback memoizes function itself.

**Comparison:**
```javascript
// useMemo: Memoizes computed value
const expensiveValue = useMemo(() => {
  return computeExpensive(a, b);
}, [a, b]);

// useCallback: Memoizes function reference
const handleClick = useCallback(() => {
  doSomething(a, b);
}, [a, b]);

// They're equivalent:
useCallback(fn, deps) === useMemo(() => fn, deps)

// Example showing difference:
const value = useMemo(() => a + b, [a, b]); // Returns number
const callback = useCallback(() => a + b, [a, b]); // Returns function
```

---

## REAL-WORLD SCENARIOS

### Scenario 1: Optimizing Large List Performance

**Problem:** Rendering 10,000 items causes lag.

**Solution:** Use virtualization
```javascript
import { FixedSizeList } from 'react-window';

function VirtualList({ items }) {
  const Row = ({ index, style }) => (
    <div style={style}>{items[index].name}</div>
  );

  return (
    <FixedSizeList
      height={600}
      itemCount={items.length}
      itemSize={50}
      width="100%"
    >
      {Row}
    </FixedSizeList>
  );
}
```

---

### Scenario 2: Preventing Unnecessary API Calls

**Problem:** Component re-renders trigger duplicate API calls.

**Solution:** Use ref to track request status
```javascript
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const fetchingRef = useRef(false);

  useEffect(() => {
    if (fetchingRef.current) return;

    fetchingRef.current = true;

    fetch(`/api/users/${userId}`)
      .then(res => res.json())
      .then(data => {
        setUser(data);
        fetchingRef.current = false;
      });
  }, [userId]);
}
```

---

### Scenario 3: Sharing State Between Sibling Components

**Problem:** Two components need same data but aren't parent-child.

**Solution:** Lift state to common parent or use Context
```javascript
// Option 1: Lift state
function Parent() {
  const [sharedData, setSharedData] = useState(null);

  return (
    <>
      <ChildA data={sharedData} onUpdate={setSharedData} />
      <ChildB data={sharedData} />
    </>
  );
}

// Option 2: Context for deeply nested components
const DataContext = createContext();

function Parent() {
  const [sharedData, setSharedData] = useState(null);

  return (
    <DataContext.Provider value={{ sharedData, setSharedData }}>
      <DeepChild />
    </DataContext.Provider>
  );
}
```

---

### Scenario 4: Handling Authentication Flow

**Problem:** Protect routes and manage auth state globally.

**Solution:** Auth context + protected routes
```javascript
const AuthContext = createContext();

function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    checkAuth().then(userData => {
      setUser(userData);
      setLoading(false);
    });
  }, []);

  const login = async (email, password) => {
    const user = await loginAPI(email, password);
    setUser(user);
  };

  const logout = () => setUser(null);

  return (
    <AuthContext.Provider value={{ user, login, logout, loading }}>
      {children}
    </AuthContext.Provider>
  );
}

function ProtectedRoute({ children }) {
  const { user, loading } = useContext(AuthContext);

  if (loading) return <div>Loading...</div>;
  if (!user) return <Navigate to="/login" />;

  return children;
}
```

---

### Scenario 5: Optimistic UI Updates

**Problem:** User waits for API response to see changes.

**Solution:** Update UI immediately, rollback on error
```javascript
function TodoList() {
  const [todos, setTodos] = useState([]);

  const toggleTodo = async (id) => {
    // Optimistic update
    const previousTodos = [...todos];
    setTodos(todos.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));

    try {
      await fetch(`/api/todos/${id}/toggle`, { method: 'POST' });
    } catch (error) {
      // Rollback on error
      setTodos(previousTodos);
      alert('Failed to update todo');
    }
  };
}
```

---

## INTERVIEW TIPS & BEST PRACTICES

### How to Answer Interview Questions

1. **Start with a clear, concise answer** (1-2 sentences)
2. **Provide deeper explanation** (how it works, why it matters)
3. **Give practical example** (code snippet or use case)
4. **Mention edge cases or alternatives** if relevant

### Red Flags to Avoid

- "I haven't used that feature"
- "I just use libraries for that"
- "I'm not sure about the internals"
- Guessing without saying "I think" or "I believe"

### Confidence Boosters

- Always mention you're familiar with official docs
- Reference recent React versions (18+)
- Discuss real projects where you used the concept
- Ask clarifying questions before answering

### Common Interviewer Follow-ups

- "Can you explain the internals?"
- "What are the trade-offs?"
- "When would you NOT use this?"
- "How would you test this?"

---

## FINAL CHECKLIST - Are You Interview Ready?

### Beginner Level
- [ ] Explain React, JSX, Virtual DOM
- [ ] Difference between props and state
- [ ] Understand components and lifecycle
- [ ] Use useState and useEffect confidently

### Intermediate Level
- [ ] Master all common hooks (useRef, useContext, useMemo, useCallback)
- [ ] Explain useReducer and when to use it
- [ ] Understand Context API and prop drilling
- [ ] Know controlled vs uncontrolled components
- [ ] Explain React Router basics

### Advanced Level
- [ ] Redux/Redux Toolkit fundamentals
- [ ] Code splitting with lazy/Suspense
- [ ] Error Boundaries
- [ ] Performance optimization techniques
- [ ] Custom hooks creation
- [ ] Testing with React Testing Library
- [ ] SSR concepts (Next.js basics)

### Coding Skills
- [ ] Build todo app from scratch
- [ ] Implement debounced search
- [ ] Create modal component
- [ ] Handle forms with validation
- [ ] Implement infinite scroll

### Tricky Concepts
- [ ] Explain why setState is async
- [ ] Closure problems with hooks
- [ ] Why not use index as key
- [ ] Rules of Hooks explanation
- [ ] Reconciliation and diffing

---

## NEXT STEPS

Ready to practice? Here's your action plan:

1. **Review this guide section by section**
2. **Code along with examples** in your editor
3. **Build mini-projects** for each concept
4. **Mock interview practice** with peers
5. **Review common interview questions** on LeetCode/GeeksforGeeks
6. **Stay updated** with React blog and new features

---

**Remember:** Interviews test not just knowledge, but how you explain concepts. Practice explaining out loud!

Good luck with your React.js interview! 🚀
