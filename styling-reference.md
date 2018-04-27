# Styling Reference

Reference for CSS/LESS/SASS/PostCSS/etc styling.

* Use PostCSS instead of SCSS/LESS because those tools are proprietary syntax, where PostCSS is allows you to write real CSS.
* Use [Prettier][prettier] to format you CSS/LESS/SCSS
* Name selectors using snake-case: `.button-primary`
* Focus on using reusable styles as much as possible (eg [Euphoria][euphoria] or similar)
* If writing custom CSS:
  * Make sure to think through if it can be made reusable.
  * Use something like (React) CSS modules to scope your CSS to each component.
