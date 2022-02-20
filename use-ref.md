# useRef hook

- Lưu các giá trị qua một tham chiếu bên ngoài

Example:
```jsx
import { useRef, useState, useEffect } from "react";

// let timerId 
// Vi phạm cấu trúc code

export default function App() {
  const [count, setCount] = useState(60);
  // let timerId  
  // Sẽ là undefined mỗi khi component re-render --> tạo ra phạm vi mới

  const timerId = useRef();
  const preCount = useRef();
  const h1Ref = useRef();

  useEffect(() => {
    // Luôn trỏ đến h1Ref ở DOM thật
    const rect = h1Ref.current.getBoundingClientRect();

    console.log(rect);
  });

  useEffect(() => {
    preCount.current = count;
  }, [count]);

  const handleStart = () => {
    timerId.current = setInterval(() => {
      setCount((pre) => pre - 1);
      // setCount(count - 1);  // Closure
    }, 1000);

    console.log("Start -> ", timerId.current);
  };

  const handleStop = () => {
    clearInterval(timerId.current);

    console.log("Stop -> ", timerId.current);
  };

  console.log(count, preCount.current);

  return (
    <div>
      <h1 ref={h1Ref}>{count}</h1>
      <button onClick={handleStart}>Start</button>
      <button onClick={handleStop}>Stop</button>
    </div>
  );
}
```