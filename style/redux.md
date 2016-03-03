## Redux Style Guide

----

* [Frameworks](#framworks)


----

### Redux

The general layout of a redux project:

```
...[configs]
main.js
src
+-- [container]
   +-- index.js
   +-- components
   +-- actions.js
   +-- sagas.js
   +-- reducers.js
+-- store
   +-- index.js
+-- app.js
```

#### main.js

This file contains pretty much nothing. It will just mount the app component to
the root node.

#### src/[container]

These directories are the main composition of your application. They contain all
the actions and components that make up this state aware component.

##### src/[container]/index.js

The entry file that connects the container component to state and actions. looks
something like this:

```
import React from 'react';
import { connect } from 'react-redux';
import todo from './components/todo';
import * as actions from './actions';

export default connect(
  ({ count }) => ({ count }),
  actions
)(Counter);
```

##### src/[container]/components

This directory is entirely for dumb components that don't need to know anything
about the structure that they are rendering. Ideally, this means that they look
like this:

```
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

##### src/[container]/actions.js

Actions contain all the action creators that are used to notify redux reducers of changes to state. They pretty much always look like this:

```
export const ADD_TODO = 'ADD_TODO';
export const addTodo = (todo, userId) => ({
    type: ADD_TODO,
    todo: todo,
    userId: userId
});
```

The restriction to these actions is that all the types need to be absolutely unique. They usually
take the form of ```[VERB]_[NOUN]_[STATUS]``` where the last STATUS section is optional and
usually represents an event sent asynchronously in response to the action. The NOUN
should be related to the container in which the action is related to.

You can also use what is called a Thunk, which is basically a function instead of
an object:

```
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


##### src/[container]/sagas.js

Sagas are more complicated async actions that allows your asynchronous code to
look synchronous. However, they still behave like actions and cannot directly modify
state. They need to issue actions themselves that will be read by the reducers.

```
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

##### src/[container]/reducers.js

These are all the reducers responsible for the top level state of this container.
That means store.getState() would return a state something like this:
```
{
  [container]: {
    ...
  }
}
```

What you do with this state is entirely up to you.

Eg:
```
import * as actionTypes from './actions';

export const count = (state = {}, action) => {
  switch(action.type) {
    case actionTypes.ADD_TODO:
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
