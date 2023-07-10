1. npm i @reduxjs/toolkit1 react-redux.
2. Setup the Redux Store
   src/redux/store.ts


```js
import { configureStore } from "@reduxjs/toolkit";

export const store = configureStore({
  reducer: {},
  devTools: process.env.NODE_ENV !== "production",
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

3. Define Typed Hooks
   src/redux/hooks.ts

```js
import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";
import type { RootState, AppDispatch } from "./store";

export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

4. Define a custom provider

```js
"use client";

import { store } from "./store";
import { Provider } from "react-redux";

export function Providers({ children }: { children: React.ReactNode }) {
  return <Provider store={store}>{children}</Provider>;
}
```

5. Provide the Redux Store to Next.js 13

```js
<html lang="en">
  <body className={inter.className}>
    <Providers>{children}</Providers>
  </body>
</html>
```

6. Create the Redux State Slice and Action Types

To start, we will create a “slice” file that will be responsible for managing the state of a counter component. Each slice file can be thought of as a specific piece of state in our application, and they are typically located within the “features” directory in the “redux” folder.

To create the counter slice, navigate to the “redux” directory and create a new folder called “features“. Inside the “features” folder, create a new file named counterSlice.ts and add the following code to it.

src/redux/features/counterSlice.ts

```js
import { createSlice, PayloadAction } from "@reduxjs/toolkit";

type CounterState = {
  value: number;
};

const initialState = {
  value: 0,
} as CounterState;

export const counter = createSlice({
  name: "counter",
  initialState,
  reducers: {
    reset: () => initialState,
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action: PayloadAction<number>) => {
      state.value += action.payload;
    },
    decrementByAmount: (state, action: PayloadAction<number>) => {
      state.value -= action.payload;
    },
  },
});

export const {
  increment,
  incrementByAmount,
  decrement,
  decrementByAmount,
  reset,
} = counter.actions;
export default counter.reducer;

```

7. Add the Slice Reducer to the Store

```js
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "./features/counterSlice";

export const store = configureStore({
  reducer: {
    counterReducer,
  },
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```
