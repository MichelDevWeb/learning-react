# useContext hook

### Dùng khi nào?
- Truyền dữ liệu từ component cha đến component con không cần dùng đến props ex: CompA => CompB => CompC  (A->C)

### Cách dùng

Example
- ThemeContext.js
```jsx
import { createContext, useState } from "react";

const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("dark");

  const toggleTheme = () => {
    setTheme(theme === "dark" ? "light" : "dark");
  };

  const value = {
    theme,
    toggleTheme
  };

  return (
    <ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>
  );
}

export { ThemeContext, ThemeProvider };
```
- index.js
```jsx
import { StrictMode } from "react";
import ReactDOM from "react-dom";

import App from "./App";
import { ThemeProvider } from "./ThemeContext";

const rootElement = document.getElementById("root");
ReactDOM.render(
  <StrictMode>
    <ThemeProvider>
      <App />
    </ThemeProvider>
  </StrictMode>,
  rootElement
);
```
- App.js
```jsx
import "./styles.css";
import Content from "./content";
import { ThemeContext } from "./ThemeContext";
import { useContext } from "react";

// 1. Create context
// 2. Provider
// 3. Consumer

export default function App() {
  const context = useContext(ThemeContext);

  return (
    <div>
      <button onClick={context.toggleTheme}>Toggle them</button>
      <Content />
    </div>
  );
}
```
- Content.js
```jsx
import Paragraph from "./paragraph";
export default function Content() {
  return (
    <div>
      <Paragraph />
    </div>
  );
}
```
- Paragraph.js
```jsx
import { useContext } from "react";
import { ThemeContext } from "./ThemeContext";

export default function Paragraph() {
  const context = useContext(ThemeContext);

  return <div>{context.theme}</div>;
}
```

### Lưu ý
