# redux-pouchdb-middleware
A redux middleware to sync data between redux with pouchdb and vice-versa.

## How it works?
- This package is a middleware for [Redux](https://redux.js.org/advanced/middleware#middleware).
- It persists the state of chosen reducer of the Redux into [PouchDB](http://pouchdb.com/) every time store get updated.
- It reducers will be passed the state from PouchDB on application loads.
- This is a one way data sync (for now). That means, it only persists the data from reducers to PouchDB. Only on app loads, it fetch the data from PouchDB to reducers.


## Installation

```sh
$ yarn add redux-pouchdb-middleware

or,
$ npm install redux-pouchdb-middleware
```

### Peer Dependencies
- [lodash](https://github.com/lodash/lodash)
- [pouchdb](https://github.com/pouchdb/pouchdb)


## Usage

An example React-redux store configuration.

```javascript
import {
    applyMiddleware,
    compose,
    createStore,
}                                 from "redux"
import { ReduxPouchDBMiddleware } from "redux-pouchdb-middleware"
import PouchDB                    from "pouchdb"
import reducers                   from "./reducers"
import { initialLoad }            from "./reducers/actions"

const initialStore = {}
const middleware = []
const enhancers = []

middleware.push(ReduxPouchDBMiddleware({
    reducer: "todos",
    verbose: false,
    db: new PouchDB("todos-store", { adapter }),
    actions: {
        initialInsert: todos => initialLoad(todos),
    },
}))

const composedEnhancers = compose(applyMiddleware(...middleware), ...enhancers)

export default createStore(reducers, initialStore, composedEnhancers)

```

## API

### `ReduxPouchDBMiddleware(options)`
- `options`: array|object. An array of objects or simply object with specs as specified below.

### Options
- `reducer`: string|required. Name of the reducer to store data. Its a json data key for store's root reducer.
- `db`: required. an instance of PouchDB Database.
- `verbose`: boolean|optional(default = false). Weather to print logs in console or not.
- `actions`:  an object describing the actions to perform when initially inserting items and when a change occurs in the db.
- `actions.initialInsert`: redux action to store data from pouchDB to store on app load.

## License

MIT
