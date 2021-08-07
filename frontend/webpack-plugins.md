Webpack is a popular module bunlder in Javascript web application projects. It is used to bundle multiple javascript modules into one or several bunlde files. It is needed because can significantly reduce files size that browser needed by code spliting, tree shaking and lazy reload. Webpack comes with handly configuration files that allow users to configure bundling strategies under different enviroment. In this blog, we will review some popular Webpack loaders and plugins. Finally, we will explore what is the webpack.configure file of create-react-app source.

# Webpack loaders

Loaders transfor the source code of a module to different kinds. This transformation includes **compiling** Typescript to Javascript, loading css to Asynchronous Source Tree, etc. We will go through some popular loaders. For a full list of loader, you can find them [here]([https://github.com/webpack-contrib/awesome-webpack#loaders]).

One way to add loaders to webpack.config.js:

```js
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' }, // add css-loader
      { test: /\.ts$/, use: 'ts-loader' }, // add ts-loader
    ],
  },
};
```

### Transpiling

* **babel-loader** Loads ES2015+ code and transpiles to **ES5** using Babel
* **ts-loader** Loads [TypeScript](https://www.typescriptlang.org/) 2.0+ like JavaScript

###  Styling 

* **css-loader** Interprets `@import` and `url()`like `import/require()`and will resolve them

* **style-loader** Injects CSS into the DOM
* **less-loader/sass-load** Loads and compiles an associated file.
* **postcss-loader** Loads and transform a CSS/SSS file using PostCss (Like a css compiler)

### File

* **val-loader** Executes a given module, return its result in the build time. When bundle file requres the module, the loader changes a module from code to result.

* **ref-loader** Create dependencies between any file mannually

* **file-loader** Resolves import/require() on a file into a url and emits the file into the output directory (Deprecated for Webpack 5 since [Asset module](https://webpack.js.org/guides/asset-modules/] ).

  E.g. `import img from './file.png';` 

  

