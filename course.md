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

# To-do List with Redux

Now that "Hello, World" is out of the way, let's take a look at how redux
and React component state work. Redux is an npm module, so let's add it to
`package.json` and re-run npm install.

Now, let's start the server and start
webpack in "watch" mode so all changes recompile live, and open up a browser
to the "Hello, World" example. Over in `index.js`, let's include redux.

## Introducing Stores

The fundamental concepts in redux are called "stores" and "actions".
A store has two parts: a plain-old JavaScript object that represents the
current state of your application, and a "reducer", which is a function that
describes how incoming actions modify your state.

To create a store that represents the state of a checkbox, let's create a
default state, a reducer that allows actions of type 'TOGGLE' to change
the state, and finally use redux's `createStore()` function to create a new
store.

Note that the reducer receives both the current state and the action as
parameters, and returns the modified state. A well-written reducer should
not have any side-effects, that is, if you call a reducer with the same state
and the same action, you should always get the same result. Reducers should
not modify or rely on any global state. It's also good practice to encapsulate
as much of your application logic as possible in reducers, because, since your
reducers don't rely on side-effects or global state, they're really easy to
test, debug, and refactor.

## Displaying the State

So great, now that you have a store, what do you do with it? A store has
three functions that you're going to be using in this course: `subscribe()`,
`dispatch()`, and `getState()`. `getState()` is simple, it just gets the
current state of the store. `dispatch()` is the function you use to fire
off a new action. `subscribe()` fires a callback every time a
new action is fired **after** your reducer is done, so if you call `getState()`
in the `subscribe()` callback you get the newly reduced state.

First off, let's make the App component subscribe to the state. The right
place to do that is in the `componentWillMount()` function. This function
is an example of what's known as a React component "life-cycle hook".
The `componentWillMount()` function is important here because it gets called
when the component is going to be created, so it's a good place to put
initialization logic.

Let's subscribe to the redux store and call React's `setState()` function
every time the store changes. React components also have their own internal
state. React calls the `render()` function every time the
component's state changes, and it's the `render()` function's job to
tell React how to draw the component.

Let's tweak the App component to display a checkbox whose state depends on
the redux store. When the `checked` state is falsy, note that the checkbox
is off. When you change `checked` to true by default, the checkbox is on.

What happens when you click on the checkbox? Absolutely nothing! What gives?
One of the key ideas of React is that what gets displayed is a pure function
of the component's state. In other words, since your state doesn't change,
React will always render the checkbox as checked.

## Dispatching Actions

In order to mutate the redux state, you need to dispatch an action. Recall
that an action is the 2nd parameter that gets passed to your reducer. An
action is a plain-old JavaScript object that tells your reducer what happened.
Let's create a function `onClick` that calls the redux store's `dispatch()`
function, which is how you fire off a new action in redux.

Redux actions
**must** have a `type` property, so let's create an action with type `TOGGLE`
to match the reducer, and add the `onClick` function as an `onClick` handler
for the checkbox. Now, if you look in the browser, you'll notice you can
actually toggle the checkbox.

This seems like a lot of work to just put a toggle-able checkbox on the screen.
However, now the state of the checkbox is tied to your store's state, which
lets you manipulate other elements based on the state of the checkbox.
For example, let's add an element that appears only when the checkbox is
checked. Now this H2 element will show up in the browser when the checkbox is
on.

This curly-brace syntax enables you to put JavaScript expressions into your
HTML. A common cause of bugs when using curly-braces like the conditional
expression here is the subtle difference between an expression and a
statement in JavaScript. As written, this code is an expression, but
once you add a semicolon, it becomes a statement and webpack gives you
a scary compiler error. Make sure you don't put semi-colons in your curly
braces!
