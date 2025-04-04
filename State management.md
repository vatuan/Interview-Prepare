# State management

## Zustand

- [1. Zustand là gì và cách hoạt động của nó?](#1-zustand-la-gi-va-cach-hoat-dong-cua-no)
- [2. So sánh Zustand với các state management khác](#2-so-sanh-zustand-voi-cac-state-management-khac)
- [3. Tối ưu performance khi sử dụng Zustand](#3-toi-uu-performance-khi-su-dung-zustand)
- [4. Các middleware](#4-cac-middleware)

## 1. Zustand là gì và cách hoạt động của nó?

Zustand là một thư viện quản lý state cho React, sử dụng hook `useStore` để quản lý và chia sẻ trạng thái toàn cục trong ứng dụng mà không cần Redux.

Zustand sử dụng một store duy nhất để chứa **state** và các **actions**.

## 2. So sánh Zustand với các state management khác

- **Zustand**: Nhẹ, dễ sử dụng, không cần boilerplate, sử dụng store duy nhất.
- **Redux**: Phức tạp hơn, cần nhiều boilerplate.
- **Context API**: Đơn giản nhưng có thể gặp vấn đề với performance khi dữ liệu thay đổi nhiều.

## 3. Tối ưu performance khi sử dụng Zustand

### 1. Sử dụng `shallow` hoặc hook `useShallow()` để so sánh trạng thái

Dùng `shallow` để so sánh trạng thái ở trên các phần cần thiết, thay vì so sánh toàn bộ

Ví dụ

```ts
import { useEffect } from "react";
import { create } from "zustand";
import { useShallow } from "zustand/react/shallow";
import { shallow } from "zustand/shallow";

type BearFamilyMealsStore = {
  [key: string]: string;
};

const useBearFamilyMealsStore = create<BearFamilyMealsStore>()(() => ({
  papaBear: "large porridge-pot",
  mamaBear: "middle-size porridge pot",
  babyBear: "A little, small, wee pot",
}));

const meals = [
  "A tiny, little, wee bowl",
  "A small, petite, tiny pot",
  "A wee, itty-bitty, small bowl",
];

function UpdateBabyBearMeal() {
  useEffect(() => {
    const timer = setInterval(() => {
      useBearFamilyMealsStore.setState({
        babyBear: meals[Math.floor(Math.random() * (meals.length - 1))],
      });
    }, 1000);

    return () => {
      clearInterval(timer);
    };
  }, []);

  return null;
}

function BearNames() {
  const names = useBearFamilyMealsStore(
    useShallow((state) => Object.keys(state)) // sử dụng useShallow để keep lại key, vì key không change -> component BearNames sẽ không bị re-render
  );

  // sử dụng shallow
  const names = useBearFamilyMealsStore((state) => Object.keys(state), shallow);

  return <div>{names.join(", ")}</div>;
}

export default function App() {
  return (
    <>
      <UpdateBabyBearMeal />
      <BearNames />
    </>
  );
}
```

### 2. Chia nhỏ store thành nhiều phần

```ts
import create from "zustand";

// Store cho phần Count
const useCountStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
}));

// Store cho phần User
const useUserStore = create((set) => ({
  user: { name: "John", age: 30 },
  setUser: (user) => set(() => ({ user })),
}));
```

### 3. Dùng `immer` để tránh thay đổi trực tiếp trạng thái

Sử dụng immer sẽ giúp việc thay đổi trạng thái trở nên dễ dàng hơn mà không lo phải sao chép trạng thái trước khi thay đổi

```js
import create from "zustand";
import { immer } from "zustand/middleware";

const useStore = create(
  immer((set) => ({
    count: 0,
    user: { name: "John", age: 30 },
    increment: () =>
      set((state) => {
        state.count += 1;
      }),
    setUser: (user) =>
      set((state) => {
        state.user = user;
      }),
  }))
);
```

### 4. Dùng `selectors` chỉ để lấy state cần thiết

Dùng selectors để lấy các state cần thiết thay vì lấy toàn bộ state, từ đó giảm bớt số lần render không cần thiết và tăng hiệu suất

```js
const count = useStore((state) => state.count); // Selector chỉ lấy `count`
```

## 4. Các middleware

## Redux
