# Installation

## 1) Install package

```
npm install syncss --save-dev
```

The installation will create for you multiple directories and files in your project (`/src/styles/` by default).

## 2) Require Syncss in your project

`_index.scss` is the entry point of your Syncss instance. They will give you access to all helpers and styles componants, so choice to import it in your project with a global scope.

You need to choice how import it depending of your project:

### SCSS

You can include this file directly in your scss.

```scss
@use "./_index.scss";
```

or 'App.vue' component if you use Vue.js

```scss
@use "./styles/_index.scss";
```

> [!NOTE]  
> You can have need to install a SASS loader.
>
> See: https://vue-loader.vuejs.org/guide/pre-processors.html#sass

### JavaScript

Another option can be to load this file with JavaScript.

```javascript
import "./styles/_index.scss";
```

This is also the more simple usage with React.js.

### CSS

Sur you can also choice to build this file with a "module bundler" like vite or webpack and import your CSS builded file into your html head.

## 3) Change the configuration

You can now use helpers and components like you would. Think to update the configuration files to match with you design system.

Discover the concept to create your hown framwork with Syncss : [Discover the Guide](/guide/).

Or dicover how to personalize your Syncss : [Discover configs files](/config/).


