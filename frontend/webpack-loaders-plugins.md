Webpack is a popular module bunlder in Javascript web application projects. It is used to bundle multiple javascript modules into one or several bunlde files. It is needed because can significantly reduce files size that browser needed by code spliting, tree shaking and lazy reload. Webpack comes with handly configuration files that allow users to configure bundling strategies under different enviroment. In this blog, we will review some popular Webpack loaders and plugins. Finally, we will explore what is the what is inside of webpack.configure.js in create-react-app source.

# Webpack loaders

Loaders transfor the source code of a module to different kinds. This transformation includes **transpiling** Typescript to Javascript, loading css to Asynchronous Source Tree, etc. We will go through some popular loaders. For a full list of loader, you can find them [here]([https://github.com/webpack-contrib/awesome-webpack#loaders]).

One way to add loaders to `module.rules` webpack.config.js:

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

* **source-map-loader** Extracts source maps from existing files. This allows Webpack to interprete source map across own and third party libraries. If not extracted and processed into the source map of the webpack bundle, browsers may misinterpret source map data. 

  

## Webpack Plugins

Webpack plugin is a Javascript object with an apply method. This apply method is called by the webpack compiler, giving the access to the entire compilation lifecycle. A plugin is complimentary to a loader. 

#### Webpack optimization

```javascript
optimization: {
  minimize: true,
  minimizer: [
    // ...plugins
  ]
},
```

Optimazition is a webpack property has `minize` property. When it triggers, all plugins inside `minimizer` will be triggered.

* **Terser webpack plugin** minimised the application JavasScript bundle file for production. It is used in tree shaking. Webpack v5 comes with this plugin out of the box.

* **DllPlugin/DllReferencePlugin** stands for Dynamic-link library. Spliting the bundles in a way that can drastically improve build time.

* **CssMinimizerWebpackPlugin** optimise and **minify** CSS with [cssnano](https://cssnano.co/)

* **HtmlWebpackPlugin** It uses lodash templates or provided ones to create html file and include this html file to all bundles file

  E.g. 

  ```html
  <--dist/index.html-->
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>webpack App</title>
    </head>
    <body>
      <script src="index_bundle.js"></script>
    </body>
  </html>
  ```

* **DefinePlugin** allows you to define variable with code. This is useful when you need a global variable to distinguish enviroment and production mode.

  ```js
  new webpack.DefinePlugin({
    PRODUCTION: JSON.stringify(true),
    VERSION: JSON.stringify('5fa3b9'),
    BROWSER_SUPPORTS_HTML5: true,
    TWO: '1+1',
    'typeof window': JSON.stringify('object'),
    'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV),
  });
  // Now in your code, you can check access VERSION	 
  ```

*  **HotModuleReplacementPlugin** enables Hot Module Replacement (HMR)

* **SplitChunksPlugin** Code splitting feature. A splited chunk can be shared among bundle files

* **MiniCssExtractPlugin** For each js file, it will extract the styles into new a css file. This is useful for code spliting and source-map

## Case Study: Create-react-app

Webpack has been preconfigured and hidden in a create-react-app project. You can find the full configuration [here](https://github.com/facebook/create-react-app/blob/main/packages/react-scripts/config/webpack.config.js).

### Loaders

#### Style loaders

* **MiniCssExtractPlugin.loader (Dev)** Usually comes with css-loader
* **css-loader**

* **postcss-loader**

* **resolve-url-loader** Resolve CSS preProcessor files and assets. E.g. url(...)
* **(preProcessor-loader)** Depends on which preProcessor the user chose.
* **source-map-loader (opt-in)** 
* **@svgr/webpack** SVGR can be used as a [webpack](https://webpack.js.org/) loader, this way you can import your SVG directly as a React Component. Then you can import.
* **file-loader**
* **Babel-loader**

### Plugins

* **Terser Plugin (Prod)**

* **HtmlWebpackPlugin**

*  **InlineChunkHtmlPlugin (Prod and checked InlineRuntimeChunk)**

* **InterpolateHtmlPlugin**

  > " Makes some environment variables available in index.html.
  >
  > The public URL is available as %PUBLIC_URL% in index.html, e.g.:
  >
  > <link rel="icon" href="%PUBLIC_URL%/favicon.ico">
  >
  > It will be an empty string unless you specify "homepage"
  >
  > in `package.json`, in which case it will be the pathname of that URL."

* **ModuleNotFoundPlugin** Gives some context to module not found error
* **WebpackDefinePlugin**

* **ReactRefreshWebpackPlugin (Dev and checked use react refresh)** Experimental hot reloading for react
*  **MiniCssExtractPlugin**
* **CaseSensitivePathsPlugin** Using this plugin helps alleviate cases where developers working on OSX, which does not follow strict path case sensitivity, will cause conflicts with other developers or build boxes running other operating systems which require correctly cased paths.
* **WebpackManifestPlugin** Generates an asset manifest file
* **IgnorePlugin** Optimises moment.js with webpack
* **InjectManifest (Env)** Generate a service work script that will precache
* **ForkTsCheckerWebpackPlugin (Typescript)** Checks typescript type
* **ESLintPlugin**
