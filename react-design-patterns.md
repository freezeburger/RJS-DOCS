
# 🧩 Design Patterns les plus utilisés en React

Ce document présente les principaux patterns utilisés dans React avec des exemples simples pour illustrer leur usage.

---

## 1. Container / Presentational Pattern

**Objectif :** Séparer la logique métier (data, state) de l’affichage.

```tsx
// Presentational.tsx
export const UserCard = ({ name }: { name: string }) => (
  <div>👤 {name}</div>
);
```

```tsx
// Container.tsx
import { useState, useEffect } from 'react';
import { UserCard } from './Presentational';

export const UserCardContainer = () => {
  const [userName, setUserName] = useState('Loading...');

  useEffect(() => {
    fetch('/api/user').then(res => res.json()).then(data => setUserName(data.name));
  }, []);

  return <UserCard name={userName} />;
};
```

---

## 2. Custom Hook Pattern

**Objectif :** Extraire une logique réutilisable dans un hook.

```tsx
// useMousePosition.ts
import { useEffect, useState } from 'react';

export function useMousePosition() {
  const [pos, setPos] = useState({ x: 0, y: 0 });

  useEffect(() => {
    const handler = (e: MouseEvent) => setPos({ x: e.clientX, y: e.clientY });
    window.addEventListener('mousemove', handler);
    return () => window.removeEventListener('mousemove', handler);
  }, []);

  return pos;
}
```

```tsx
// App.tsx
import { useMousePosition } from './useMousePosition';

export const App = () => {
  const { x, y } = useMousePosition();
  return <p>Position : {x}, {y}</p>;
};
```

---

## 3. Compound Components Pattern

**Objectif :** Permettre à un composant parent de contrôler plusieurs sous-composants.

```tsx
// Toggle.tsx
import { createContext, useContext, useState, ReactNode } from 'react';

const ToggleContext = createContext<{ on: boolean; toggle: () => void } | null>(null);

export const Toggle = ({ children }: { children: ReactNode }) => {
  const [on, setOn] = useState(false);
  return (
    <ToggleContext.Provider value={{ on, toggle: () => setOn(!on) }}>
      {children}
    </ToggleContext.Provider>
  );
};

export const ToggleOn = ({ children }: { children: ReactNode }) => {
  const ctx = useContext(ToggleContext);
  return ctx?.on ? <>{children}</> : null;
};

export const ToggleButton = () => {
  const ctx = useContext(ToggleContext);
  return <button onClick={ctx?.toggle}>Toggle</button>;
};
```

```tsx
// App.tsx
import { Toggle, ToggleOn, ToggleButton } from './Toggle';

export const App = () => (
  <Toggle>
    <ToggleButton />
    <ToggleOn>✅ C'est activé !</ToggleOn>
  </Toggle>
);
```

---

## 4. Render Props Pattern

**Objectif :** Déléguer le rendu via une fonction passée en prop.

```tsx
// MouseTracker.tsx
import { useMousePosition } from './useMousePosition';

export const MouseTracker = ({ children }: { children: (pos: { x: number; y: number }) => JSX.Element }) => {
  const pos = useMousePosition();
  return children(pos);
};
```

```tsx
// App.tsx
export const App = () => (
  <MouseTracker>
    {({ x, y }) => <p>🧭 Position : {x}, {y}</p>}
  </MouseTracker>
);
```

---

## 5. Higher-Order Component (HOC) Pattern

**Objectif :** Ajouter une fonctionnalité à un composant existant.

```tsx
// withBorder.tsx
export function withBorder<P>(Component: React.ComponentType<P>) {
  return (props: P) => (
    <div style={{ border: '2px solid blue', padding: 8 }}>
      <Component {...props} />
    </div>
  );
}
```

```tsx
// App.tsx
const Box = ({ label }: { label: string }) => <div>{label}</div>;
const BoxWithBorder = withBorder(Box);

export const App = () => <BoxWithBorder label="Important info" />;
```

---

## 6. Context Provider Pattern

**Objectif :** Partager un état global sans prop drilling.

```tsx
// ThemeContext.tsx
import { createContext, useContext } from 'react';

const ThemeContext = createContext<'light' | 'dark'>('light');

export const useTheme = () => useContext(ThemeContext);

export const ThemeProvider = ({ children }: { children: React.ReactNode }) => {
  return <ThemeContext.Provider value="dark">{children}</ThemeContext.Provider>;
};
```

```tsx
// App.tsx
import { ThemeProvider, useTheme } from './ThemeContext';

const Header = () => {
  const theme = useTheme();
  return <h1>🌙 Thème : {theme}</h1>;
};

export const App = () => (
  <ThemeProvider>
    <Header />
  </ThemeProvider>
);
```

---

## 7. State Reducer Pattern (useReducer)

**Objectif :** Gérer des états complexes de manière prédictible.

```tsx
// counterReducer.ts
export type CounterAction = { type: 'increment' } | { type: 'decrement' };

export function counterReducer(state: number, action: CounterAction): number {
  switch (action.type) {
    case 'increment': return state + 1;
    case 'decrement': return state - 1;
    default: return state;
  }
}
```

```tsx
// App.tsx
import { useReducer } from 'react';
import { counterReducer } from './counterReducer';

export const App = () => {
  const [count, dispatch] = useReducer(counterReducer, 0);

  return (
    <>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <span>{count}</span>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
    </>
  );
};
```

---

Fin.
