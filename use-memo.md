# useMemo hook

### Dùng khi nào?
Tránh thực hiện lại logic không cần thiết

### Cách dùng

Example:
```jsx
import { useRef, useState, useMemo } from "react";

export default function App() {
  const [name, setName] = useState("");
  const [price, setPrice] = useState("");
  const [products, setProducts] = useState([]);

  const nameRef = useRef();

  const handleSubmit = () => {
    setProducts([...products, { name, price: +price }]);

    setName("");
    setPrice("");
    nameRef.current.focus();
  };

  // Bị tính lại không cần thiết khi component re-render (trường hợp k dùng useMemo())
  // deps giống useEffect và useCallback (gọi lại khi giá trị deps thay đổi)
  const total = useMemo(() => {
    const result = products.reduce((result, prod) => {
      console.log("Tính toán lại....");

      return result + prod.price;
    }, 0);
    return result;
  }, [products]);

  return (
    <div>
      <input
        ref={nameRef}
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <br />
      <input value={price} onChange={(e) => setPrice(e.target.value)} />
      <br />
      <button onClick={handleSubmit}>Add</button>
      Total: {total}
      <ul>
        {products.map((item, index) => (
          <li key={index}>
            {item.name} - {item.price}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

### Lưu ý
- logic deps giống như `useEffect()` và `useCallback()`