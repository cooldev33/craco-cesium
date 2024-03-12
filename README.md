# craco-cesium

Let's use 🌍[Cesium](https://cesiumjs.org) with [create-react-app](https://github.com/facebook/create-react-app) today! This is a plugin for [@craco/craco](https://github.com/sharegate/craco). [Resium](https://resium.darwineducation.com) is also recommended.

## NOTE: Unmaintained: Vite is recommended rather thant create-react-app

Currently create-react-app development is inactive. Now it would be a good time to start using Vite to start development as it is easier and faster. See [Resium documentation](https://resium.reearth.io/installation#5-vite) for more information.

There are no plans to perform any maintenance on this library. If you have any requests for additional functionality or dependency upgrading, please send us a pull request!

## Very very easy usage

### 1. Create a React project

```sh
npm install -g create-react-app
# or
yarn global add create-react-app

create-react-app example
cd example
```

### 2. Install modules

In your create-react-app project, install modules:

```sh
npm install --save @craco/craco craco-cesium cesium resium
# or
yarn add @craco/craco craco-cesium cesium resium
```

### 3. Rewrite npm scripts

Rewrite npm scripts in your project's `package.json` as following:

```js
{
  // ...
  "scripts": {
    "start": "craco start", // react-scripts -> craco
    "build": "craco build", // react-scripts -> craco
    "test": "craco test",   // react-scripts -> craco
    "eject": "react-scripts eject"
  },
  // ...
}
```

### 4. (NEW!) Add npm overrides

To fix package vulnerabilities, add npm overrides in your project's `package.json` as following:

```js
{
  // ...
  "overrides": {
    "resolve-url-loader": "^5.0.0",
    "@svgr/webpack": "^8.1.0",
    "webpack": "^5.90.3",
    "copy-webpack-plugin": "^12.0.2"
  }
}
```

### 5. (NEW!) Update craco-cesium's index.js

Replace the `index.js` file in your `craco-cesium` directory (the filepath should be something like `example/node_modules/craco-cesium/index.js`) with the [index.js file found in this repo](https://github.com/cooldev33/craco-cesium/blob/master/index.js).

### 6. Create craco config file

Create `craco.config.js` in the project root:

```js
module.exports = {
  plugins: [
    {
      plugin: require("craco-cesium")()
    }
  ]
};
```

### 7. Congratulations! 🎉

Set up is complete! Enjoy your Cesium life.

You can import Cesium as following:

```js
import { Viewer, Entity, Color } from "cesium";
```

If you are using [Resium](https://resium.darwineducation.com), you can import Cesium and Resium as following.

```js
import { Color } from "cesium";
import { Viewer, Entity } from "resium";
import * as Cesium from "cesium";

// Grant CesiumJS access to your ion assets
// Cesium.Ion.defaultAccessToken = "paste your token here";
```

## 🔥Pro Tip: Enabling HMR

- 💡 Example project is [here](https://github.com/rot1024/create-react-app-cesium-example).

1. `yarn add craco-plugin-react-hot-reload react-hot-loader @hot-loader/react-dom`

⚠️ `@hot-loader/react-dom`'s version should be the same as `react`'s.

2. Add an alias of webpack and `craco-plugin-react-hot-reload` plugin to `craco.config.js`:

```js
module.exports = {
  webpack: {
    alias: {
      "react-dom": "@hot-loader/react-dom"
    }
  },
  plugins: [
    { plugin: require("craco-plugin-react-hot-reload") },
    { plugin: require("craco-cesium")() }
  ]
};
```

4. Wrap root component with hot function in `src/App.js`

```js
export default App;
```

to

```js
import { hot } from "react-hot-loader/root";

// ~~~~~~~~~~~~~~~~~~~~~~~~~

export default hot(App);
```

Done!

Please refer to [react-hot-loader](https://github.com/gaearon/react-hot-loader) to refer to the details.

## Options

If the option is omiited, the default options is used:

```js
CracoCesiumPlugin({
  loadPartially: false,
  loadCSSinHTML: true,
  cesiumPath: "cesium",
});
```

### `loadPartially`

If false, whole Cesium will be loaded in HTML and `window.Cesium` is used in `import { ... } from "cesium";`. This is the easiest way.

Otherwise, Cesium will be load partially and bundled in the JS. You have to install `strip-pragma-loader` to build Cesium for production: `npm i -S strip-pragma-loader`.

For more details, refer to [Cesium official tutorial](https://cesium.com/docs/tutorials/cesium-and-webpack/).

### `loadCSSinHTML`

If true, `Widgets/widgets.css` in Cesium is loaded in HTML.

Otherwise, you have to load the CSS once manually as following.

If `loadPartially` is true:

```js
import "cesium/Widgets/widgets.css";
```

Otherwise:

```js
import "cesium/Build/CesiumUnminified/Widgets/widgets.css";
```

### `cesiumPath`

Directory path destination to copy Cesium files.
