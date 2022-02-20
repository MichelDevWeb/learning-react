# useLayoutEffect hook

### Cách thức hoạt động khác với useEffect một chút
#### useEffect
1. Cập nhật lại state
2. Cập nhật lại DOM (mutated)
3. Render lại UI
4. Gọi cleanup nếu deps thay đổi
5. Gọi `useEffect callback`

#### useLayoutEffect
1. Cập nhật lại state
2. Cập nhật lại DOM (mutated)
3. Gọi cleanup nếu deps thay đổi (sync - đồng bộ)
4. Gọi `useLayoutEffect callback` (sync - đồng bộ)
5. Render lại UI

Example:
```jsx
function Example() {
  const [count, setCount] = useState(0)

  // Khi dùng useEffect thì khi count đến giá trị 3 nó sẽ nhảy lên 4 trước và set lại bằng 0 ngay lập tức
  // Điều đó sẽ thấy rõ hơn khi data là 1 list hoặc data lớn --> Sử dụng useLayoutEffect
  useEffect(() => {
    if (count > 3) 
      setCount(0)
  }, [count])

  const handleRun = () => {
    setCount(count + 1)
  }

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={handleRun}>Run</button>
    </div>
  )
}
```