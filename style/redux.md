## Redux Style Guide

We have a skeleton project setup to get everyone started [here](https://github.com/influitive/react-redux-boilerplate) which should be used for any new front-end project.

Use the generators provided by that project to generate new domain level objects that can be integrated into the application.

### Project layout

The general layout of a redux project:

```
...[configs]
main.js
src
+-- [domain]
   +-- index.js
   +-- components
   +-- action-types.js
   +-- actions.js
   +-- sagas.js
   +-- sagas.test.js
   +-- reducers.js
   +-- reducers.test.js
   +-- selectors.js
+-- store
   +-- index.js
+-- app.js
```

#### main.js

This file contains pretty much nothing. It will just mount the app component to
the root node.

#### src/[domain]

These directories are the main composition of your application. They contain all
the actions and components that make up this state aware component.

##### src/[domain]/index.js

The entry file that connects the domain component to state and actions. looks
something like this:

```js
import React from 'react';
import { connect } from 'react-redux';
import todo from './components/todo';
import * as actions from './actions';

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(Counter);
```

##### src/[domain]/components

This directory is entirely for dumb components that don't need to know anything
about the structure that they are rendering. Ideally, this means that they look
like this:

```jsx
import React from 'react';

const todo = ({ text, isComplete }) => (
  <div>
    <div>
      {text}
    </div>
    <div>
      {isComplete}
    </div>
  </div>
);
```

Note that you could send down a todo object as a single prop but that would imply
that the todo component needs to know the todo structure looks.

##### src/[domain]/action-types.js

The action-types file describes all the constants that describe the actions. The names
always need to be prefixed with their domain. Actions usually take the form
of ```[VERB]_[STATUS]``` where the last STATUS section is optional and
usually represents an event sent asynchronously in response to the action.


##### src/[domain]/actions.js

Actions contain all the action creators that are used to notify redux reducers of changes to state. They pretty much always look like this:

```js
import * as actionTypes from './action-types';

export const addTodo = (todo, userId) => ({
    type: actionTypes.add,
    todo: todo,
    userId: userId
});
```



You can also use what is called a Thunk, which is basically a function instead of
an object:
```js
export const waitAndThenAddTodo = (todo, userId) => {
  return (dispatch, getState) => {
    setTimeout( ()=> {
      dispatch(addTodo(todo,userId));
    }, 5000);
  }
};
```

This method would wait 5000 ms and then dispatch the add action. Optionally, you
can also call `getState` to get the current state of the application.


##### src/[domain]/sagas.js

Sagas are more complicated async actions that allows your asynchronous code to
look synchronous. However, they still behave like actions and cannot directly modify
state. They need to issue actions themselves that will be read by the reducers.

```js
import { takeLatest } from 'redux-saga';
import * as actions from './actions';
import { put } from 'redux-saga/effects';

const wait = (time) => new Promise(resolve => setTimeout(()=>resolve(), time))

export function* waitAndThenAddTodo() {
  yield* takeLatest(actions.ADD_TODO, function* (action) {
    yield wait(5000);
    put(action);
  });
}
```

You can wrap this code in try catch blocks to catch any errors during the process.
Just not that yield works with Promises but not with traditional callback
APIs, so those would have to be wrapped with a promise (as seen with the ```wait```
method above.)

##### src/[domain]/reducers.js

These are all the reducers responsible for the top level state of this domain.
That means store.getState() would return a state something like this:
```js
{
  [domain]: {
    ...
  }
}
```

What you do with this state is entirely up to you.

Eg:
```js
import * as actionTypes from './action-types';

export const count = (state = {}, action) => {
  switch(action.type) {
    case actionTypes.ADD:
      return {
        text: action.todo,
        userId: action.userId
      };
    default:
      return state;
  }
};
```

Or you could combine multiple multiple reducers using ```combineReducers```



### Frameworks

* [redux](https://github.com/reactjs/redux): Obviously. Handles state.
* [react-redux](https://github.com/reactjs/react-redux): Some helper methods to build container components.
* [redux-saga](https://github.com/yelouafi/redux-saga): Handles async actions and stories.
* [redux-devtools-extension](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en): A chrome plugin to provide useful tools for modifying and monitoring redux state changes.



### More details

A lot more information on best practices can be found [here](https://medium.com/lexical-labs-engineering/redux-best-practices-64d59775802e#.gkaj1dltp)
and [here](http://jaysoo.ca/2016/02/28/organizing-redux-application/)
