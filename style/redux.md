## Redux Style Guide

----
* [Directory Structure](#directory structure)
* [Frameworks](#framworks)
----

### Directory Structure

```
+-- src
|   +-- actions
|      +-- todo
|         +-- index.js
|      +-- login
|         +-- index.js
|   +-- components
|      +-- login
|         +-- index.js
|      +-- todo
|         +-- index.js
|   +-- reducers
|      +-- index.js
|      +-- login.js
|      +-- todo.js
|   +-- sagas
|      +-- index.js
|   +-- index.js
```

#### actions

Actions contain all the action creators that are used to notify redux reducers of changes to state. They pretty much always look
like this:

```
export const addTodo = (todo, userId) => {
  return {
    type: 'ADD_TODO',
    todo: todo,
    userId: userId
  };
};
```

The restriction to these actions is that all the types need to be absolutely unique. They usually
take the form of ```[VERB]_[NOUN]_[STATUS]``` where the last STATUS section is optional and
usually represents an event sent asynchronously in response to the action.


#### components

These are dumb components. They should never have any business logic or complex logic. They can largely be constructed
as such:

```

```


### Frameworks

* [redux](https://github.com/reactjs/redux): Obviously. Handles state.
* [react-redux](https://github.com/reactjs/react-redux): Some helper methods to build container components.
* [redux-saga](https://github.com/yelouafi/redux-saga): Handles async actions and stories.
* [redux-devtools-extension](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en): A chrome plugin to provide useful tools for modifying and monitoring redux state changes.
* [react-router-redux](https://github.com/reactjs/react-router-redux): A redux integrated version of react router.



### More details

A lot more information on best practices can be found [here](https://medium.com/lexical-labs-engineering/redux-best-practices-64d59775802e#.gkaj1dltp)
