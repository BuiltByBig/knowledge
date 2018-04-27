# React

## General React tips

* **Use stateless function components where possible**: They're easier to reason about, are less code, and don't manage their own state (which is a good goal to attain).
* **Use component state first** and then move to application state (Redux/MobX/etc) only if you need to share state.
* **Always use `propTypes`**: Makes it easy to know what a component expects
  * **Store common props in `prop-types.js`**: Store any commonly used PropType definitions in a central location so they can be reused.
  * Use `defaultProps`, if relevant
* **Name with `.jsx` extensions**: This makes it clear if it is a React component as well as allows for better editor integrations and build process tooling.
* **Actions should be passed into components**: Pass in actions as function references to components rather than the components defining the functions. Main exception is when a state management solution isn't being used.
* **Use container components**: Wrap state management and behavior around your stateless components using a container component that is responsible for managing state and defining functionality.

## General React Syntax

### Using a container component with a "stupid" component

This is an example of combining a stateless functional React "stupid" component with a stateful class "container" component, `user-list.jsx`:

```js
import 'axios' from 'axios'
import Alert from './alert'
import LoadingSpinner from './loading-spinner'
import t from 'prop-types'
import React from 'react'
import { Person } from './prop-types'

// List just concerns itself with rendering UI and not
// handling any logic.
function List({ error, loading, users }) {
  if (loading) {
    return <LoadingSpinner />
  }

  if (error) {
    return <Alert type="error">{error}</Alert>
  }

  return (
    <ul>
      {users.map((user, key) => (
        <li key={key}>
          {user.name} - {user.email}
        </li>
      ))}
    </ul>
  )
}

List.propTypes = {
  error: t.string,
  loading: t.bool,
  users: t.arrayOf(Person),
}

List.defaultProps = {
  error: null,
  loading: false,
  users: [],
}

// Encapsulate fetching data within a stateful container component.
class UserList extends React.Component {
  state = { error: null, loading: false, users: [] }

  componentWillMount() {
    this.fetch()
  }

  fetch() {
    this.setState({ loading: true })
    axios
      .get('/api/v1/users')
      .then(resp => this.setState({ error: null, users: resp.data }))
      .catch(error => this.setState({ error: error.message }))
      .finally(() => this.setState({ loading: false }))
  }

  render() {
    const { error, loading, users } = this.state
    return <List error={error} loading={loading} users={users} />

    // Or simply:
    // return <List {...this.state} />
  }
}

export default UserList
```

This represents a reasonable division of responsibilities. The "stateful" component class is only responsible for loading data and setting up the sate. The functional "stateless" component is only responsible for rendering data and doesn't care where the data comes from or how it is fetched. This way you can repurpose the function component in other places if you need.

### When to use `state` in your components

You should use "stateful" component classes when you need to manage state that doesn't need to be shared outside the component.

### When to use state management tools like Redux/MobX/etc

You should start with local state and only move to global state when the state is needed in multiple places or it gets cumbersome to pass state down a complicated state tree.

It is almost always best to start with pure React components and then move to state management tools when you need them, not before.

### Storing Common PropTypes

Create a file, say `prop-types.js`, in the root of your components folder and store any commonly used props in here, like so:

```jsx
// prop-types.js
export const Person = PropTypes.shape({
  name: PropTypes.string.isRequired,
  email: PropTypes.string.isRequired,
  bio: PropTypes.string,
  age: PropTypes.number,
})
```

Now, to use them:

```jsx
import React from 'react'
import t from 'prop-types'
import { Person } from './prop-types'

function SomeThing({ person }) {
  return <h1>{person.name}</h1>
}

SomeThing.propTypes = {
  person: Person,
}

export default SomeThing
```

### Using `ref` to get form field values

```jsx
function Input() {}

class Form extends React.Component {
  constructor(props) {
    super(props)
    this.input = React.createRef()
  }

  handleSubmit = e => {
    e.preventDefault()
    const val = this.input.current.value
    console.log('Input value is:', val)
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input type="text" ref={this.input} />
      </form>
    )
  }
}
```

Learn more about [forwarding refs here](https://reactjs.org/docs/forwarding-refs.html).

## React Router

### Setting up a simple router

```jsx
import React from 'react'
import Header from './'
import Dashboard from './dashboard'
import Reprot from './report'
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom'

function App() {
  return (
    <Router>
      <div className="container">
        <Header />
        <Switch>
          <Route exact path="/" component={Dashboard} />
          <Route path="/report" component={Report} />
          <Route path="/posts/:id" component={Post} />
          <Route path="/about" render={() => <h1>About us</h1>} />
        </Switch>
      </div>
    </Router>
  )
}

export default App
```

### Using URL parameters

To use the URL parameters passed into a component from React Router, you need to use the `match` prop passed into the component:

```jsx
import React from 'react'

function Post({ match }) {
  return <h1>Post: {match.params.id}</h1>
}

export default Post
```

See the [docs on `match`](https://reacttraining.com/react-router/web/api/match)
