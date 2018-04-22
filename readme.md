# knowledge

> A collection of software suggestions, best practices, resources and snippets.

## Table of Contents

* [General practices](#general-practices)
* [JavaScript](#javascript)
  * [Tips](#javascript-tips)
  * [Testing](#javascript-testing)
  * [Build Tools](#javascript-build-tools)
  * [Libraries](#javascript-libraries)
  * [Variables](#javascript-variables)
  * [Conditionals](#javascript-conditionals)
  * [Resources](#javascript-resources)
* [Styling](#styling)

## General Practices

* **Optimize for readability**: Code is read much more than it is written so you should always work on writing legible, clear code that will be obvious to others or yourself after not seeing the code for a long time.
* **Focus on clarity**: You should strive for code that is legible and clear. Avoid fancy tricks and obscure (but perhaps elegant) techniques. It is more important to write code that is maintainable for years to come than to save a few lines of code.
* **Document, document, document**: If you're building something besides the most trivial feature, you should document what your code does and how to use it. This includes:

  * Inline comments where a piece of code might not be clear on first reading or where you had to do something to handle an edge case.
  * React components should use `propTypes` and `defaultProps` (see below)
  * Your functions should have clear argument names
  * You should name your variables useful names (eg not `an` but instead `accountName`)
  * All projects should have a `readme.md` file that describes the project, how to get it setup for development, how to deploy it and other relevant information. It should be kept up to date as these details change.
    * Keep your readme file up to date as your project evolves.

## JavaScript

### JavaScript Tips

* Use TypeScript if possible to fix common programming mistakes and better structure your code.
* Use Eslint to check your code for other common issues.
* Use [Prettier][prettier] to format your code so every developer's code looks the same.
* Alphabetically order imports, key values and variables so it's easier to find what you're looking for.
* **Name files in kebab-case**: For example, `admin-dashboard.js` and _not_ `AdminDashboard.js`. This improves legibility and makes completion on the command line a bit easier.
* **One "thing" per file**: With few exceptions, you should have one isolated piece of functionality per file like a utility function, or a React component. The only main exception is when you need a local helper/component that doesn't make sense to break out. If you're building out a large feature, it may make sense to put all related components in a folder to keep things organized.
  * If things start to get more complicated, place the code in a folder and move each unique part of functionality to it's own file and then create an `index.js`/`index.ts` file to hook everything together.
* **camelCase variables and functions**: Follow JavaScript standards and use `camelCase` for all variables. the only exception is using snake_case for `CONSTANT_VARIABLE_NAMES`.

### JavaScript Testing

* [Jest][jest] - A robust, fast and all-in-one test framework by Facebook
* [Enzyme][enzymb] - A React testing library by AirBnB
* Browser testing: (http://nightwatchjs.org/, http://phantomjs.org/, http://casperjs.org/)

### JavaScript Build Tools

* **npm over yarn**: Use npm instead of yarn as a package manager since npm is installed by default when installing node and will most likely become relatively in sync with yarn at some point in the future. If this changes, we will adjust our choice accordingly.
* **Scripts**: Place tasks in your `package.json`'s `scripts` block to be run with `npm run`:
  * [npm-run-all](https://www.npmjs.com/package/npm-run-all) - Run npm scripts in parallel/series
  * `npm start` should run the development environment
  * `npm run serve` should run on production (if node)
  * `npm run build` should build frontend assets (if frontend only)
* [**create-react-app**][cra] - The easiest way to get a React project started

### JavaScript Libraries

* [Rollup][rollup] - Build node modules
* [Lerna][lerna] - Manage multiple node modules in one codebase
* [commander][commander] - Command line argument parsing
* [cosmiconfig][cosmiconfig] - A configuration management library
* [Gatsby](https://www.gatsbyjs.org/) - React static site generator (also see [react-static](https://github.com/nozzle/react-static), [Phenomic](https://phenomic.io))

### JavaScript Variables

Use variables to make code more clear:

```js
// Not ideal:
if (!request.headers['content-type'].startsWith('application/json')) {
  // stuff...
}

// Better:
const requestNotJSON = !request.headers['content-type'].startsWith(
  'application/json'
)
if (requestNotJSON) {
  // stuff...
}
```

While this is more code, it makes reading the code easier since you don't have to mentally parse the conditional since the variable name clearly explains what it represents.

### JavaScript Conditionals

Always prefer to return early rather than nesting if statements:

```js
// Bad!
if (foo) {
  if (bar) {
    return 'bar'
  } else {
    return 'not bar'
  }
} else {
  return 'not foo'
}

// Good!
if (!foo) return 'not foo'
if (!bar) return 'not bar'
return 'bar'
```

If a value needs a default, use an `or` operator:

```js
const active = active || false
```

When a conditional is needed to alternate two simple values, use a "ternary" expression:

```js
const label = user.admin ? 'Admin!' : 'Regular user'
```

**Never nest ternary expressions!**:

```js
// THIS IS BAD DON'T DO IT!
const val = x === 0 ? 'none' : 100 > x ? 'a lot' : 'a few'
```

Nested ternaries are very hard to reason about 99% of the time so they should be avoided.

If a simple conditional has a longer body, use a standard `if` statement:

```js
if (loggedIn) {
  console.log('logged in')
  doSomeThing()
} else {
  console.log('not logged in')
  doSomeOtherThing()
}
```

However, if you can, return early like we discuss above.

If a value needs to be set with a short value but there may be more than two potential values, use inline `if` statements as they read cleanly:

```js
let icon
if (success) icon = 'check'
if (loading) icon = 'spinner'
if (error) icon = 'exclamation-point'
```

#### Deep Equality Checking

If you're writing code to check that a certain key is set or of a particular value in a deeply nested object:

```js
// BAD!
if (foo && foo.bar && foo.bar.baz && foo.bar.baz.biz) {
  // do something...
}

// BETTER!
import dot from 'dot-wild'

if (dot.get('foo.bar.baz.biz')) {
  // do something...
}
```

This code is easier to read, more succinct and more reliable. Currently we recommend using [dot-wild][dotwild] as it supports a large variety of utilties and query strings.

### JavaScript Resources

* [Modern JavaScript cheat sheet](https://github.com/mbeaudru/modern-js-cheatsheet)

### React

* **Use stateless function components where possible**: They're easier to reason about, are less code, and don't manage their own state (which is a good goal to attain).
* **Use component state first** and then move to application state (Redux/MobX/etc) only if you need to share state.
* **Always use `propTypes`**: Makes it easy to know what a component expects
  * **Store common props in `prop-types.js`**: Store any commonly used PropType definitions in a central location so they can be reused.
  * Use `defaultProps`, if relevant
* **Name with `.jsx` extensions**: This makes it clear if it is a React component as well as allows for better editor integrations and build process tooling.
* **Actions should be passed into components**: Pass in actions as function references to components rather than the components defining the functions. Main exception is when a state management solution isn't being used.
* **Use container components**: Wrap state management and behavior around your stateless components using a container component that is responsible for managing state and defining functionality.

#### An Example

This is an example of comping stateless functional React component with a stateful class component, `user-list.jsx`:

```js
import { Person } from './prop-types'
import Alert from './alert'
import LoadingSpinner from './loading-spinner'
import PropTypes from 'prop-types'
import React from 'react'

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
  error: PropTypes.string,
  loading: PropTypes.bool,
  users: PropTypes.arrayOf(Person),
}

List.defaultProps = {
  error: null,
  loading: false,
  users: [],
}

// Encapsulate fetching data within a stateful component
class UserList extends React.Component {
  constructor(props) {
    super(props)
    this.state = { error: null, loading: false, users: [] }
  }

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
  }
}

export default UserList
```

This represents a reasonable division of responsibilities. The "stateful" component class is only responsible for loading data and setting up the sate. The functional "stateless" component is only responsible for rendering data and doesn't care where the data comes from or how it is fetches. This way you can repurpose the function component in other places if you need.

#### Storing Common PropTypes

```jsx
// prop-types.js
export const Person = PropTypes.shape({
  name: PropTypes.string.isRequired,
  email: PropTypes.string.isRequired,
  bio: PropTypes.string,
  age: PropTypes.number,
})
```

#### When to Use Component Classes

You should use "stateful" component classes when you need to manage state that doesn't need to be shared outside the component.

You should start with local state and move to global state when the state is needed in multiple places or it gets cumbersome to pass state down a complicated state tree.

## Styling

* Use PostCSS instead of SCSS/LESS because those tools are proprietary syntax, where PostCSS is allows you to write real CSS.
* Use [Prettier][prettier] to format you CSS/LESS/SCSS
* Name selectors using snake-case: `.button-primary`
* Focus on using reusable styles as much as possible (eg [Euphoria][euphoria] or similar)
* If writing custom CSS:
  * Make sure to think through if it can be made reusable.
  * Use something like (React) CSS modules to scope your CSS to each component.

[commander]: https://github.com/tj/commander.js/
[cosmiconfig]: https://github.com/davidtheclark/cosmiconfig
[cra]: https://github.com/facebook/create-react-app
[enzyme]: https://github.com/airbnb/enzyme
[euphoria]: https://github.com/euphoria-css/euphoria
[dotwild]: https://github.com/tsuyoshiwada/dot-wild
[jest]: https://facebook.github.io/jest/
[rollup]: https://rollupjs.org
