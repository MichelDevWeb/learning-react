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