# useReducer hook
- useReducer
1. Init state: 0
2. Action
3. Reducer
4. Dispatch

### Dùng khi nào?
- Sử dụng khi state phức tạp (object nhiều tầng, lớp)

### Cách dùng
- useState
1. Init state: 0
2. Action: Up (state + 1) / Down (state - 1)

- useReducer
1. Init state: 0
2. Action: Up (state + 1) / Down (state - 1)
3. Reducer
4. Dispatch

Example:
```jsx
// Todo app with useReducer hook
import { useRef, useReducer } from "react";

// 1. Init state
const initState = {
  job: "",
  jobs: []
};

// 2. Actions
const SET_JOB = "set_job";
const ADD_JOB = "add_job";
const REMOVE_JOB = "remove_job";

const setJob = (payload) => {
  return {
    type: SET_JOB,
    payload
  };
};

const addJob = (payload) => {
  return {
    type: ADD_JOB,
    payload
  };
};

const removeJob = (payload) => {
  return {
    type: REMOVE_JOB,
    payload
  };
};

// 3. Reducer
const reducer = (state, action) => {
  console.log(action);

  switch (action.type) {
    case SET_JOB:
      return {
        ...state,
        job: action.payload
      };
    case ADD_JOB:
      return {
        ...state,
        jobs: [...state.jobs, action.payload]
      };
    case REMOVE_JOB:
      const newJobs = [...state.jobs];

      newJobs.splice(action.payload, 1);
      return {
        ...state,
        jobs: newJobs
      };
    default:
      throw new Error("Invalid action.");
  }
};

export default function App() {
  const [state, dispatch] = useReducer(reducer, initState);

  const { job, jobs } = state;
  const inputRef = useRef();

  const handleSubmit = () => {
    dispatch(addJob(job));
    dispatch(setJob(""));

    inputRef.current.focus();
  };

  console.log(job, jobs);

  return (
    <div>
      <h3>Todo</h3>
      <input
        ref={inputRef}
        value={job}
        onChange={(e) => dispatch(setJob(e.target.value))}
      />
      <button onClick={handleSubmit}>Add</button>

      <ul>
        {jobs.map((job, index) => (
          <li key={index}>
            {job}
            <span onClick={() => dispatch(removeJob(index))}>&times;</span>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

### Lưu ý
- Không cần dùng cho những bài toán đơn giản
- Create `logger` function wrap reducer

```jsx
function logger(reducer) {
  return (preState, action) => {
    console.group(action.type)
    console.log('Pre state: ', preState)
    console.log('Action: ', action)

    const newState = reducer(preState, action)

    console.log('Next state: ', newState)

    console.groupEnd()

    return newState
  }
}
```