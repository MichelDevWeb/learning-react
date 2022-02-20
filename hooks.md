# useState hook

### Dùng khi nào?
Khi muốn dữ liệu thay đổi thì giao diện tự động được cập nhật (render lại theo dữ liệu).

### Cách dùng

```jsx
import { useState } from 'react'

function Component() {
    const [state, setState] = useState(initState)

    ...
}
```

### Lưu ý
- Component được re-render sau khi `setState`
- Initial state chỉ dùng cho lần đầu
- `setState` với `callback`
- Initial state với `callback`
- `setState` là thay thế state bằng giá trị mới

#
# useEffect hook

// Side effect
- Events: Add/remove event listener
- Observer pattern: Subscribe/unsubscribe
- Closure
- Timers: setInterval, setTimeout, clearInterval, clearTimeout
- useState
- Mounted/unmounted
- Call API
#

1. useEffect(callback)
- Gọi callback mỗi khi component re-render
- Gọi callback sau khi component thêm element vào DOM
2. useEffect(callback, [])
- Chỉ gọi callback 1 lần sau khi component mounted
3. useEffect(callback, [deps])
- Callback sẽ được gọi lại mỗi khi deps thay đổi

#
1. Callback luôn được gọi sau khi component mounted
2. Cleanup function luôn được gọi trước khi component unmounted

```jsx
    useEffect(() => {
        const handleScroll = () => setShowOnTop(window.scrollY >= 200);
        }

        window.addEventListener('scroll', handleScroll);
        
        // Cleanup Function
        return () => {
            window.removeEventListener('scroll', handleScroll)
        }
    })
```

3. Cleanup function luôn được gọi trước khi callback được gọi (trừ lần mounted đầu tiên) 

```jsx
import { useState, useEffect } from "react";

export default function App() {
  const [avatar, setAvatar] = useState();

  useEffect(() => {

    // Cleanup function --> xoá ảnh trước
    return () => {
      avatar && URL.revokeObjectURL(avatar.preview);
    };
  }, [avatar]);

  const handlePreviewAvatar = (e) => {
    const file = e.target.files[0];

    file.preview = URL.createObjectURL(file);
    setAvatar(file);
  };

  return (
    <div>
      <input type="file" onChange={handlePreviewAvatar} />

      {avatar && <img src={avatar.preview} width="80%" alt="" />}
    </div>
  );
}
```

#
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

#
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