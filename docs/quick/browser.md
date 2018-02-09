# TypeScript in the browser
If you are using TypeScript to create a web application here are my recommendations:

## General Machine Setup

* Install [Node.js](https://nodejs.org/en/download/)

## Project Setup
* Create a project dir:

```
mkdir your-project
cd your-project
```

* Create `tsconfig.json`. We discuss [modules here](../project/external-modules.md). Also good to have it setup for `tsx` compilation out of the box:

```json
{
    "compilerOptions": {
        "target": "es5",
        "module": "commonjs",
        "sourceMap": true,
        "jsx": "react"
    },
    "exclude": [
        "node_modules"
    ],
    "compileOnSave": false
}
```

* Create an npm project:

```
npm init -y
```

* Install [TypeScript-nightly](https://github.com/Microsoft/TypeScript), [`webpack`](https://github.com/webpack/webpack), [`ts-loader`](https://github.com/TypeStrong/ts-loader/)

```
npm install typescript@next webpack ts-loader --save-dev
```

* Create a `webpack.config.js` to bundle your modules into a single `bundle.js` file that contains all your resources:

```js
module.exports = {
  devtool: 'inline-source-map',
  entry: './src/app.tsx',
  output: {
    path: __dirname + '/public',
    filename: 'build/app.js'
  },
  resolve: {
    extensions: ['.ts', '.tsx', '.js']
  },
  module: {
    rules: [
      { test: /\.tsx?$/, loader: 'ts-loader' }
    ]
  }
}
```

* Setup an npm script to run a build. Also have it run `typings install` on `npm install`. In your `package.json` add a `script` section:

```json
"scripts": {
    "watch": "webpack --watch"
},
```

Now just run the following (in the directory that contains `webpack.config.js`):

```
npm run watch
```

Now if you make edits to your `ts` or `tsx` file webpack will generate `bundle.js` for you. Serve this up using your web server 🌹.

## More
If you are going to use React (which I highly recommend you give a look), here are a few more steps:

```
npm i react react-dom @types/react @types/react-dom --save
```

A demo `index.html`:

```html
<html>
    <head>
        <meta charset="UTF-8" />
        <title>Hello React!</title>
    </head>
    <body>
        <div id="root"></div>

        <!-- Main -->
        <script src="./dist/bundle.js"></script>
    </body>
</html>
```

A demo `./src/app.tsx`:

```js
import * as React from "react";
import * as ReactDOM from "react-dom";

const Hello = (props: { compiler: string, framework: string }) => {
    return (
        <div>
            <div>{props.compiler}</div>
            <div>{props.framework}</div>
        </div>
    );
}

ReactDOM.render(
    <Hello compiler="TypeScript" framework="React" />,
    document.getElementById("root")
);
```

You can clone this demo project here: https://github.com/basarat/react-typescript

## Live reload

Add webpack dev server. Super easy: 

* Install : `npm install webpack-dev-server` 
* Add to your `package.json`: `"start": "webpack-dev-server --hot --inline --no-info"`

Now when you run `npm start` it will start the webpack dev server with live reload.
