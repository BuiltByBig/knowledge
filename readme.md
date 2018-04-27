# knowledge

> A collection of software suggestions, best practices, resources and snippets.

*   [General practices](#general-practices)
*   [JavaScript](/javascript-reference.md)
*   [React](/react-reference.md)
*   [Styling](/styling-reference.md)

## General Practices

*   **Optimize for readability**: Code is read much more than it is written so you should always work on writing legible, clear code that will be obvious to others or yourself after not seeing the code for a long time.
*   **Focus on clarity**: You should strive for code that is legible and clear. Avoid fancy tricks and obscure (but perhaps elegant) techniques. It is more important to write code that is maintainable for years to come than to save a few lines of code.
*   **Document, document, document**: If you're building something besides the most trivial feature, you should document what your code does and how to use it. This includes:

    *   Inline comments where a piece of code might not be clear on first reading or where you had to do something to handle an edge case.
    *   React components should use `propTypes` and `defaultProps` (see below)
    *   Your functions should have clear argument names
    *   You should name your variables useful names (eg not `an` but instead `accountName`)
    *   All projects should have a `readme.md` file that describes the project, how to get it setup for development, how to deploy it and other relevant information. It should be kept up to date as these details change.
        *   Keep your readme file up to date as your project evolves.
