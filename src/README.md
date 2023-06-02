## index.js (main page)

```
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import rootReducer from './reducers';
import { Provider } from 'react-redux';
import { configureStore } from '@reduxjs/toolkit';

const store = configureStore({reducer: rootReducer});

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```
- App is called
- rootReducer (in reducers folder) is configured as the redux store for react components inside App



## reducers > index.js
```
import { combineReducers } from 'redux';
import buttonCount from './buttonCount';

const rootReducer = combineReducers({
    buttonCount
});

export default rootReducer;
```
- set buttonCount as the rootReducer



## App.js

```
import logo from './logo.svg';
import './App.css';
import Button from './components/Button';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <Button />
      </header>
    </div>
  );
}
export default App;
```
- Button is called




## Button.js
```
import {useSelector, useDispatch} from 'react-redux';
import { increment } from '../actions/index.js'
import { decrement } from '../actions/index.js'

export default function Button() {
    const count = useSelector(state => state.buttonCount);

    const dispatch = useDispatch()

  return (
    <div>
      <h1>I want {count} scoops of ice cream!</h1>
      <button onClick={() => dispatch(increment(1))}>More!</button>
      <button onClick={() => dispatch(decrement(1))}>Less!</button>
    </div>
  );
}
```
- clicking the button sends `dispatch()` an action (increment/decrement defined in `actions > index.js`) to Redux's store (rootReducer: buttonCount)
- count updates `useSelector()` according to the value of buttonCount which is in Redux's store


## actions > index.js
```
export const increment = amount => {
	return {
		type: 'INCREMENT_COUNTER',
		payload: amount
	};
};

export const decrement = amount => {
	return {
		type: 'DECREMENT_COUNTER',
		payload: amount
	};
}
```
- export the type according to the action received (increment/decrement)
- export the payload according to the amount received (amount = 1, because increment(1) and decrement(1) as passed by `Button.js` in this case)



## reducers > buttonCount.js
```
const buttonCount = (count = 0, action) => {
	switch(action.type) {
       case 'INCREMENT_COUNTER':
            return count + action.payload;
		   case 'DECREMENT_COUNTER':
			      return count - action.payload;
		    default:
			      return count;
	}
};

export default buttonCount;
```
- updates buttonCount accordingly whenever an action.type is triggered (Note: count is updated automatically according to the returned value)





