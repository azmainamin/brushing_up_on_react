# Running React <!-- omit in toc -->
- [The hard way - without create-react-app](#the-hard-way---without-create-react-app)
- [The easy way - with create-react-app](#the-easy-way---with-create-react-app)
- [Babel](#babel)
  - [Terms to know](#terms-to-know)
- [Webpack](#webpack)
  - [Loaders](#loaders)
  - [Environment](#environment)
  - [Running webpack](#running-webpack)
## The hard way - without create-react-app
-   Create a node project:
    ``` npm init -y```
-   Install react and react-dom: 
    ``` npm install react react-dom ```
-   You will need atleast two files: a js file for your react component and a html file to bootstrap your react components.
```
// index.js

import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';

class App extends React.Component{
    render(){
        return(
            <div>Hello World</div>
        )
    }
}

ReactDOM.render(<App />, document.getElementById('app')); 

// index.html

<!DOCTYPE html>
<html>
    <head><title>
        my-app
    </title></head>
    <body>
        <div id="app"></div>
    </body>
</html>
```
*Code taken from: https://dev.to/vish448/create-react-project-without-create-react-app-3goh*

-   When the code is built, the js file will be run and ReactDOM.render() will inject React code into index.html.
- We can use webpack to bundle the code and babel to transpile JSX.  
  
## The easy way - with create-react-app

-   Easiest way to get started with React. Comes with all the tools necessary, including webpack, Babel etc. 
-   Follow the steps from [here](https://github.com/facebook/create-react-app).
  
## Babel

Problem: You want to write newer JS i.e. es6 but not all browser can compile and run the newer syntax and features. 

Solution: Babel transpiles newer JS into older ones so that it works with all browsers. Babel also allows us to *polyfills* to use newer functionalities that cannot be transpiled. 

### Terms to know

-   **preset**: an array of plugins used to support a particular JavaScript language feature.
-   **@babel/preset-env**: a “smart” preset that allows us to use the latest JavaScript without needing to manage which specific syntax transformations or polyfills are needed by your target environments.
-   **target enviroments**: the browsers you want babel to transpile to
-   **browserlist**: roject that lets us specify target environments using queries. Babel recommends putting the queries in a .browserslistrc file.

```
# Browsers we support

last 2 versions
not dead
> 0.5%
```

We want to target the last 2 versions of all major browsers that are not dead and has more than 0.5% of market share.  
-  babel.config.js: Where we put all of our babel config stuff so that we don't have to run babel through long cli commands. 
-  polyfills: Certain newer features of JS cannot be converted to older syntax because there is no older equivalent. For those, we define polyfills, which are code added to the browser API. We can use core-js to add polyfills to our app, via @babel/preset-env.
```
// babel.config.js

const presets = [
  ["@babel/preset-env", { // Pass a config object to the preset
    debug: true, // Output the targets/plugins used when compiling

    // NEW CODE:

    // Configure how @babel/preset-env handles polyfills from core-js.
    // https://babeljs.io/docs/en/babel-preset-env
    useBuiltIns: 'usage',

    // Specify the core-js version. Must match the version in package.json
    corejs: 3,

    // Specify which environments we support/target. (We have chosen to specify
    // targets in .browserslistrc, so there is no need to do it here.)
    // targets: "",

    // END NEW CODE

  }],
];

const plugins = [];

// Export a config object.
module.exports = { presets, plugins };
```
*Code from [here](https://www.sentinelstand.com/article/create-react-app-from-scratch-with-webpack-and-babel)*

## Webpack

Bundles all of your front end resources into one or more modules so that they can comsumed in production. Webpack is not restricted to just bundling resources. It minimizes files, transpiles and adds polyfills using babel, preprocesses and compiles css modules like SASS, etc. 

We pass an entry file to webpack and from there it traverses all the dependencies (probably recursively) from file to file. That's how webpack knows which resources it needs to include in the bundle.

### Loaders

Loaders are used to tell Webpack how to treat the different modules that we import throughout our app. Using loaders we can tell Webpack what to do when we import sum from './sum', import './styles/main.scss' and import logo from './logo.png'. For example, using a loader we can instruct Webpack to run .scss files through a Sass compiler [1].

With that in mind, let's move the babel config into webpack. Then we don't need to use babel.config.js anymore.

### Environment

Depending on the environment, we might want to do different stuff i.e. output more logs to console in dev mode. Webpack allows us to define the behavior for different envs. All we need to do is return a function instead of an object. If we export a function, it will be passed with two parameters, the first
of which is the webpack command line environment option `--env`. The other one is `--arvgv`.

The other option is to define two webpack config files: one for dev and the other for prod.

### Running webpack

```
// package.json
{
  "scripts": {
    "build": "webpack --env.environment=production --config webpack.config.js",
    "dev": "webpack --env.environment=development --config webpack.config.js",
    "devserver": "webpack-dev-server --env.environment=development --config webpack.config.js"
  },
  ...
```