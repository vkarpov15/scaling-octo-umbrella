# Hello, World w/ Webpack and React

Let's get started with a "Hello, World" example. By the end of this lesson, you'll finish
the build system and server for this course, and you'll be ready to get to the fun stuff
of actually building an application. I'm going to assume that you already have
Node.js >= 6.0.0 and npm >= 3.8.6 already installed.

## package.json

The first step is to create a `package.json` file, which tells Node.js some high-level
meta-data about your project, including the dependencies. This `package.json` file
will specify a name, the version, dependencies, devDependencies, engines, and scripts.
"Engines" specifies the node versions you can use to run this project, and "scripts"
gives you some handy command-line shortcuts for running common tasks. More on "scripts"
later, but let's take a look at the dependencies:

* React is the view layer library that you're going to be using with Redux. More about
React when we actually take a look at the code.
* ReactDOM is a library that's necessary to render React components to the browser.
* Babel is a transpiler - it compiles JavaScript-like languages into plain-old JavaScript.
In this course, you'll use JSX, which is a convenient shorthand on top of the new
ES2015 JavaScript spec for building React components. You won't use babel directly, you'll
use it through webpack, a build system that abstracts away the quirks of babel for you.
* Finally, express is an HTTP server library. You'll be using express to build a simple
server for serving up your HTML and JavaScript files.

## webpack

Now that you've set up the `package.json`, let's set up a webpack config. Remember that
webpack is a build system - it will compile your JSX into something that can run in
the browser. In this course you'll be using 3 webpack concepts: 'entry', 'output', and
'loaders'. The 'entry' is the file where webpack starts looking for JavaScript files,
in this case `src/index.js`.

Webpack will resolve 'import' statements in those files and package everything into
one JavaScript file, which will be placed in the file path you specify in 'output'.
In this case, webpack will put your finished JavaScript in `bin/main.js`.

Finally, loaders are plugins that transform your JavaScript. In this case, the loader
is 'babel', and these 'presets' tell babel that your JavaScript files use ES2015 and
JSX. So long as you use valid ES2015 and JSX, webpack will use babel to convert your
code into browser-friendly JavaScript.

## src/index.js

Now that you have a webpack config file, let's write some JavaScript. First off,
you're going to import React itself as well as ReactDOM. React projects are broken
up into "components". At a high level, a component is a JavaScript class that
implements a `render()` function, which returns what HTML should be rendered
based on the state of the component. More on state later, but for now this
'App' component will always render the same HTML, an H1 that says "Hello, World".

See these parenthesis? This is what makes JSX so special - these parenthesis are
not valid JavaScript. For now, you just need to know that you can put any valid
HTML in the parenthesis and React will render it. You'll learn more about what
the parenthesis actually do later.

Now that you have a component, you need to use ReactDOM to actually render the
component. The ReactDOM.render() function takes some JSX and a DOM element, and
renders the JSX into the DOM element, in this case the element with ID main.
Notice that instead of using a plain-old HTML tag you're using the `App` component
here. In addition to plain-old HTML tags, you can also use React components as
tags in JSX.

## npm install

Now back to `package.json`. In order to compile the `src/index.js` file all you
need to do is run the `webpack` command. In scripts, notice that the `webpack`
command is in the postinstall script - this means that webpack will automatically
compile when you run `npm install`. So now lets run `npm install`. As you can
see, here is the postinstall script running webpack.

Once that is done, take a look at the 'bin' directory and see that you now have
a `main.js` file.

## index.html and server.js

Once you have the compiled JS, lets actually get that into the browser. First,
lets write a quick server using express. The `express.static()` function lets
you serve a directory of static files, so lets use `express.static()` to serve
`./bin` on port 4000. Next, lets write a quick `index.html` file that will be
the entry point to your React app. Lets add some boilerplate CSS, including
the one for ProductionReady, an empty div with id main and the compiled `main.js`.

Now, back to the shell, start the server with `node server.js`, and point
a browser to `localhost:4000`, and voila, "Hello World"
