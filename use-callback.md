# useCallback hook
- Refernce types
- React memo()

### Dùng khi nào?
Tránh tạo ra hàm mới không cần thiết trong function component mặc dù đã sử dụng react memo()

### Cách dùng

Example:
```jsx
// App.js
import { useState, useCallback } from 'react'
import Content from './Content'

function App() {
    const [count, setCount] = useState(0)

    // Tham chiếu khác nhau khi click > 1 lần (tạo ra pham vi mới) -> khi react memo so sanh(===) -> false -> vẫn cập nhật lại thằng con
    // const handleIncrease = () => {
    //     setCount(count + 1)
    // }

    // deps trống -> trả lại tham chiếu trước đó (không tạo ra hàm mới)
    // logic deps giống như useEffect
    const handleIncrease = useCallback(() => {
        setCount(count + 1)
    }, [])

    return (
        <div>
            <Content onIncrease={handleIncrease} /> 
            <h1>{count}</h1>
        </div>
    )
}

// Content.js
import { memo } from 'react'
// ===
function Content({ onIncrease }) {
    return ( 
        <>
            <h2>Hello ae</h2>
            <button onClick={onIncrease}>Increase</button>
        </>
     )
}

export default memo(Content)
```

### Lưu ý
- Nếu sử dụng `react memo()` trong component con -> nên sử dụng `useCallback()` để tránh việc gọi lại function
- Tránh lạm dụng `useCallback()` nếu component con không dùng `react memo()`
- Trong class component: `function component` + `memo` = `PureComponent`