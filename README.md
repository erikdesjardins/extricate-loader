# extricate-loader

Webpack loader to extract content from the bundle.

A fork of [`extract-loader`](https://github.com/peerigon/extract-loader), which will better serve your needs if you only want to extract HTML or CSS.

## Installation

`npm install --save-dev extricate-loader`

## Options

By default, `extricate-loader` will resolve all `require()` expressions with Webpack's internal module loader and substitute the resolved values asynchronously.

If you supply a regexp for the `resolve` option, any path that matches will be required synchronously with Node's native `require()`.
This means that none of your Webpack loaders will apply.

This is necessary for `css-loader` to work, since it requires a helper module.

### Examples

All `.js` files: `extricate-loader?resolve=\\.js$`

## Example Usage

**webpack.config.js:**
```js
module.exports = {
  entry: 'manifest.json',
  module: {
    rules: [
      {
        test: /\.json$/,
        use: [
          { loader: 'file-loader', options: { name: '[name].[ext]' } },
          { loader: 'extricate-loader' },
          { loader: 'interpolate-loader' }
        ]
      },
      {
        test: /\.css$/,
        use: [
          { loader: 'file-loader' },
          { loader: 'extricate-loader', options: { resolve: '\\.js$' } },
          { loader: 'css-loader' }
        ]
      },
      { test: /\.js$/, use: ['entry-loader'] },
      { test: /\.png$/, use: ['file-loader'] }
    ]
  }
  // ...
};
```

**manifest.json:**

```json
{
  "scripts": "{{app.js}}",
  "css": "{{main.css}}"
}
```

**main.css:**

```css
body {
  background: url(bg.png);
}
```

### Output

**manifest.json:**

```json
{
  "scripts": "e43b20c069c4a01867c31e98cbce33c9.js",
  "css": "0dcbbaa701328a3c262cfd45869e351f.css"
}
```

**0dcbbaa701328a3c262cfd45869e351f.css:**

```css
body {
  background: url(7c57758b88216530ef48069c2a4c685a.png);
}
```

## License

Unlicense
