# Sitecore JavaScript Services pipeline utilities

## Known Issues / Limitations
* Only works with node.js, will not run in browser-targeted code
* Incompatible with a `babel` config that uses `babel-preset-env` with the `"modules": false` option, e.g.:
  ```
  "presets": [
    [
      "env",
      {
        "modules": false
      }
    ],
  ]
  ```
  The issue is that `"modules": false` instructs Babel to _not_ transpile your ES6+ modules to a format like `CommonJS`, which typically allows Webpack to apply tree shaking to your code.

  Unfortunately, this doesn't play well with file-based pipelines when you use `babel-register` as a compiler for your pipeline processor files. `babel-register` transpiles your spec files on-the-fly and uses whatever Babel configuration it can find. Therefore, if you have a single `.babelrc` file or define your Babel config in `package.json`, then `babel-register` will use that config. And if that config uses `bable-preset-env` with `"modules": false`, then your spec files won't be transpiled.

  The workaround at the moment, assuming you're using Webpack, is to move your Webpack-specific Babel config into your `babel-loader` options. Then use `.babelrc` (or equivalent) for other environments like unit testing and manifest generation.