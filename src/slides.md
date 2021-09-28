## Introduction to webpack


> At its core, webpack is a static module bundler for modern JavaScript applications. When webpack processes your application,
> it internally builds a dependency graph from one or more entry points and then combines every module your project needs into one or more bundles,
> which are static assets to serve your content from.

*https://webpack.js.org/concepts/*

<aside class="notes">
<ul>
  <li>Recursively created graph of files.</li>
  <li>Building of graph starts from entry points.</li>
  <li>Includes all files/modules with are needed by application.</li>
</ul>

<p>
In Webpack documentation, the term of the module is defined. 
</p>
<p>
In this presentation, we use the term of module and file interchangeable. 
Relevant loaders (like <code>url-loader</code> <code>file-loader</code>) allow us to treat almost any type of file as module.
</p>
</aside>


Concepts

- entry
- output
- loaders
- plugins
- mode
- browser compatibility
- resolve
- devServer

---

## Entry

<aside class="notes">
An entry point is a file or files from which webpack starts building out internal dependency graph.
</aside>


By default it is `./src/index.js`.


```js
module.exports = {
  entry: "./src/example.js",
};
```


```js
module.exports = {
  entry: {
    main: "./src/example.js",
  },
};
```
<aside class="notes">
Equivalent to previous one.
</aside>


```js
module.exports = {
  entry: {
    main: "./src/app.js",
    vendor: "./src/vendor.js",
  },
};
```
<aside class="notes">
Object Syntax
</aside>


> Why? With this you can import required libraries or files that aren't modified (e.g. Bootstrap, jQuery, images, etc) inside vendor.js
> and they will be bundled together into their own chunk. Content hash remains the same,
> which allows the browser to cache them separately thereby reducing load time.

*https://webpack.js.org/concepts/entry-points/*

---

## Output

<aside class="notes">
Destination of created by webpack bundles.
</aside>


By default, it is `./dist/main.js`.


```js
module.exports = {
  output: {
    filename: "bundle.js",
  },
};
```


```js
module.exports = {
  entry: {
    app: "./src/app.js",
    search: "./src/search.js",
  },
  output: {
    filename: "[name].js",
    path: __dirname + "/dist",
  },
};
```
<aside class="notes">
Example with multiple entry points.
</aside>


#### Substitutions

| Template    | Description |
| ----------- | ----------- |
| [id]        | -           |
| [name]      | -           |
| [chunkhash] | -           |


```js
module.exports = {
  //...
  output: {
    filename: "[name].[contenthash].bundle.js",
  },
};
```
<aside class="notes">
Example of substitution.
</aside>

---

## Loaders

<aside class="notes">
Webpack understands only JavaScript and JSON files by default. 
Loaders allow webpack to understand other file formats as well.

Webpack with suitable loaders allow as to import any type of file/module.

Loader is just a node module with export aa anonymous function.

Loaders allow:

<ul>
  <li>Transpile files</li>
  <li>Load images as url</li>
  <li>Includes all files/modules with are needed by application</li>
</ul>

Loaders can be:

<ul>
  <li>Configured with an options</li>
  <li>Chained</li>
  <li>Synchronous or asynchronous</li>
</ul>
</aside>


```js
npm install --save-dev css-loader ts-loader

module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },
      { test: /\.ts$/, use: 'ts-loader' },
    ],
  },
};
```
<aside class="notes">
Example.
</aside>


```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: "style-loader" },
          {
            loader: "css-loader",
            options: {
              modules: true,
            },
          },
          { loader: "sass-loader" },
        ],
      },
    ],
  },
};
```
<aside class="notes">
More advanced example with chaining and options. 
</aside>

---

## Plugins

<aside class="notes">
Are used for wider spectrum of tasks than loaders like bundle optimization, asset management etc.
To use plugin, we have to import them and add to <code>plugins</code> array in configuration.

Plugins are JavaScript object with <code>apply</code> method.
</aside>


```js
const HtmlWebpackPlugin = require("html-webpack-plugin"); //installed via npm
const webpack = require("webpack"); //to access built-in plugins

module.exports = {
  plugins: [
    new webpack.ProgressPlugin(),
    new HtmlWebpackPlugin({ template: "./src/index.html" }),
  ],
};
```
<aside class="notes">
Example.
</aside>

---

## Mode


- `development`
- `production`
- `none`

By default it is set to `production`.

<aside class="notes">
Mode determines webpack optimizations, and set <code>process.env.NODE_ENV</code> to <code>development</code> or <code>production</code>
</aside>

---

## Browser Compatibility

<aside class="notes">
ES5-compliant. Needs for example Promises.
</aside>

---

## Resolve

<aside class="notes">
Allows us to configure, how webpack resolve modules.
</aside>


```js
module.exports = {
  resolve: {
    alias: {
      "react": "preact-compat",
      "react-dom": "preact-compat"
      "components": path.resolve(__dirname, 'src/view/components'),
    },
  },
};
```


```js
import React form 'react'; // in fact we using preact!
import { ExampleComponent } from 'components'; // instead of './src/view/components'
```

---

## DevServer


```
npm i --D webpack-dev-server
```

```js
module.exports = {
  devServer: {
    static: './dist',
  },
};
```


- `allowedHosts`
- `client`
- `overlay`
- `http2`
- `https`
- `headers`
- `host`
- `port`
- `static`
- and much more...

<aside class="notes">
webpack-dev-server has many options to configure
</aside>

---

## There is more

| option        | description                                                        |
| ------------- | ------------------------------------------------------------------ |
| target        | specify target environment (`target: 'node16.01'` for example)     |
| cache         | configure cache og generated webpack modules and chunks            |
| devtool       | configure if and how to generate source maps                       |
| watch         | configure if and how recompile files when changed                  |
| externals     | configure with dependencies should be excluded from output bundles |
| performance   | configure waring and error performance rules                       |
| node          | Node.js options                                                    |
| stats         | configure with information about bundling should be displayed      |
| experiments   | activates experimental features.                                   |
| even more ... | https://webpack.js.org/configuration/other-options/                |

<aside class="notes">
Suggestion <b>Externals</b> is good team name after Marvel Eternals release ;)
</aside>

---

## Popular loaders


- `babel-loader`
- `ts-loader`
- `css-loader`
- `sass-loader`
- `style-loader` - allow us to import styles
- `angular2-template-loader`
- `vue-loader`

---

## Popular plugins


- `CopyWebpackPlugin` - allow us to copy files or directories
- `EslintWebpackPlugin`
- `HtmlWebpackPlugin`
- `TerserPlugin` - minify JavaScript

---

## What about this presentation?


```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const CopyPlugin = require("copy-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "dist"),
  },
  module: {
    rules: [
      {
        test: /\.(ttf|woff|eot)$/,
        loader: "file-loader",
        options: {
          name: "[name].[ext]?[hash]",
        },
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({ template: "src/index.html" }),
    new CopyPlugin({
      patterns: [{ from: "src/*.md", to: "[name][ext]" }],
    }),
  ],
  devServer: {
    static: "./dist",
    client: {
      overlay: {
        errors: true,
        warnings: false,
      },
    },
  },
};
```
