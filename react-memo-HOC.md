# React.memo HOC
Ghi nhớ props của component để quyết định có render component đó hay không -> Tránh re-render component khi không cần thiết
### 1. memo() -> Higher Order Component (HOC)
### 2. useCallback()

#
### Thông tin thêm
Được sinh ra đều để kế thừa logic (lặp đi lặp lại logic ở component)
- Hooks -> Dùng trong function component
- HOC -> Được thiết kế để dùng cho cả Class và Function Component -> Wrap component lại
- Render props

### Cách dùng

```jsx
// App.js
import { useState } from 'react'
import Content from './Content'

function App() {
    const [count, setCount] = useState(0)
    const [count2, setCount2] = useState(0)

    const increase = () => {
        // Khi set lại state Content component sẽ được render lại 
        // Khi dùng memo child component chỉ bị thay đổi khi props truyền vào có sự thay đổi
        setCount(count + 1)
    }

     const increase2 = () => {
        setCount2(count2 + 1)
    }

    return (
        <div>
            <Content count={count} /> 
            <h1>{count}</h1>
            <button onClick={increase}>Increase</button>
            <button onClick={increase2}>Increase2</button>
        </div>
    )
}

// Content.js
import { memo } from 'react'
// ===
function Content({ count }) {
    return ( <h2>Hello moi nguoi {count}</h2> )
}

export default memo(Content)
```