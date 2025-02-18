assets-webpack-plugin
=====================

[ ![Codeship Status for sporto/assets-webpack-plugin](https://codeship.com/projects/c9171f30-f64d-0132-8e3e-02d99c35d383/status?branch=master)](https://codeship.com/projects/85994)

Webpack plugin that emits a json file with assets paths.

## Why is this useful?

When working with Webpack you might want to generate your bundles with a generated hash in them (for cache busting).

This plug-in outputs a json file with the generated assets so you can find the assets from somewhere else.

### Example output:

As of version 2.0 the output now includes keys for different assets kinds.

```
{
  "one": {
    "js":    "one-bundle.js",
    "jsMap": "one-bundle.js.map"
  },
  "two": {
    "js":    "two-bundle.js"
  }
}
```

## Install

```sh
npm install assets-webpack-plugin --save-dev
```

## Configuration

In your webpack config include the plug-in. And add it to your config:

```js
var path                 = require('path');
var AssetsPlugin         = require('assets-webpack-plugin');
var assetsPluginInstance = new AssetsPlugin();

module.exports = {
  ...
  output: {
    path:        path.join(__dirname, "public", "js"),
    filename:    "[name]-bundle-[hash].js",
    publicPath:  "/js/"
  },
  ....
  plugins: [assetsPluginInstance]
};  
```

### Options

You can pass the following options:

__path__: 

Path where to save the created json file. Defaults to the current directory.

```js
new AssetsPlugin({path: path.join(__dirname, 'app', 'views')})
```

__filename__: 

Name for the created json file. Defaults to `webpack-assets.json`

```js
new AssetsPlugin({filename: 'assets.json'})
```

### Using this with Rails

You can use this with Rails to find the bundled Webpack assets via sprockets. In `ApplicationController` you might have:

```ruby
def script_for(bundle)
  path = Rails.root.join('app', 'views', 'webpack-assets.json') # This is the file generated by the plug-in
  file = File.read(path)
  json = JSON.parse(file)
  json[bundle]['js']
end
```

Then in the actions:

```ruby
def show
  @script = script_for('clients') # this will retrieve the bundle named 'clients'
end
```

And finally in the views:

```erb
<div id="app">
  <script src="<%= @script %>"></script>
</div>
```

## Test

```sh
npm test
```
