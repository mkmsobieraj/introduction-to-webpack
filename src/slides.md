## Introduction to webpack


> At its core, webpack is a static module bundler for modern JavaScript applications. When webpack processes your application, 
> it internally builds a dependency graph from one or more entry points and then combines every module your project needs into one or more bundles, 
> which are static assets to serve your content from.

*https://webpack.js.org/concepts/*


Concepts

- entry
- output
- loaders
- plugins
- mode
- browser compatibility

---

## Entry

<aside class="notes">
An entry point is a file or files from which webpack starts building out internal dependency graph.

By default it is <code>./src/index.js</code>
</aside>


```js
module.exports = {
  entry: './src/example.js',
};
```


```js
module.exports = {
  entry: {
    main: './src/example.js',
  },
};
```
<aside class="notes">
Equivalent to previous one.
</aside>


```
module.exports = {
  entry: {
    main: './src/app.js',
    vendor: './src/vendor.js',
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

---

## Loaders

---

## Plugins

---

## Mode

---

## Browser Compatibility