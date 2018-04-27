# JavaScript Reference

* [Tips](#javascript-tips)
* [Testing](#javascript-testing)
* [Build Tools](#javascript-build-tools)
* [Libraries](#javascript-libraries)
* [Variables](#javascript-variables)
* [Conditionals](#javascript-conditionals)
* [Resources](#javascript-resources)

## JavaScript Tips

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
* [Enzyme][enzyme] - A React testing library by AirBnB
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
* [npm-run-all](https://www.npmjs.com/package/npm-run-all) - Run npm scripts in parallel/series
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

[commander]: https://github.com/tj/commander.js/
[cosmiconfig]: https://github.com/davidtheclark/cosmiconfig
[cra]: https://github.com/facebook/create-react-app
[enzyme]: https://github.com/airbnb/enzyme
[euphoria]: https://github.com/euphoria-css/euphoria
[dotwild]: https://github.com/tsuyoshiwada/dot-wild
[jest]: https://facebook.github.io/jest/
[lerna]: https://github.com/lerna/lerna
[prettier]: https://prettier.io/
[rollup]: https://rollupjs.org
