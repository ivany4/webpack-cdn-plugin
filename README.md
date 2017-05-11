## CDN extension for the HTML Webpack Plugin

Enhances [html-webpack-plugin](https://github.com/ampedandwired/html-webpack-plugin) functionality by allowing you to specify the modules you want to externalize from node_modules in development and a CDN in production.

Basically this will allow you to greatly reduce build time when developing and improve page load performance on production.

### Installation

It is recommended to run webpack on node 5.x or higher

Install the plugin with npm:

```bash
npm install webpack-cdn-plugin --save-dev
```

or yarn

```bash
yarn add webpack-cdn-plugin
```

### Basic Usage

Require the plugin in your webpack config:

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
```

Add the plugin to your webpack config:

```javascript
module.exports = {
  // ...
  plugins: [
    new HtmlWebpackPlugin(),
    new WebpackCdnPlugin([
      {
        name: 'vue',
        var: 'Vue',
        style: 'dist/vue.css'
      },
      {
        name: 'vue-router'
      }
    ])
  ]
  // ...
};
```

This will generate an `index.html` file with something like below:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Webpack App</title>
    <link href="//unpkg.com/vue@2.3.3/dist/vue.css" rel="stylesheet">
  </head>
  <body>
  <script type="text/javascript" src="//unpkg.com/vue@2.3.3/dist/vue.runtime.common.js"></script>
  <script type="text/javascript" src="//unpkg.com/vue-router@2.5.3/dist/vue-router.common.js"></script>
  </body>
</html>
```

You can also use your own custom html template, please refer to [html-webpack-plugin](https://github.com/ampedandwired/html-webpack-plugin).

Please see the [example](example) folder for a basic working example.

### Parameters

The Parameters you can pass in the following order: `modules`, `prod`, `url`

#### `modules`:`array`

The available options for each module, which is part of an array.

`name`:`string`

The name of the module you want to externalize

`var`:`string` (optional)

A variable that will be assigned to the module in global scope, webpack requires this. If not supplied than it will be the same as the name.

`path`:`string` (optional)

You can specify a path to the main file that will be used, this is useful when you want the minified version for example if main does not point to it.

`style`:`string` (optional)

If the module comes with style sheets, you can also specify it as a path.

#### `prod`:`boolean` | `true`

`prod` flag defaults to `true`, which will output uri using the CDN, when `false` it will use the file from `node_modules` folder locally.

#### `url`:`string` | `//unpkg.com/:name@:version/:path`

You can specify a custom template url with the following replacement strings:

`:name`: The name of the module e.g. `vue`

`:version`: The version of the module e.g. `1.0.0`

`:path`: The path to the file e.g. `lib/app.min.js`

A common example is you can use cdnjs e.g. `//cdnjs.cloudflare.com/ajax/libs/:name/:version/:path`. If not specified it will fallback to using unpkg.com.

### Contribution

This is a pretty simple plugin and caters mostly for my needs. However, I have made it as flexible and customizable as possible.

If you do find any bugs, do please report it in the [issues](/../../issues) or can help improve the codebase, [pull requests](/../../pulls) are mostly welcome.