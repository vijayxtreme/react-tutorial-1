# Getting Started

In this simple exercise, you'll build a simple file that outputs "Hello World" using React.

You'll need the latest version of Node.js, which you can get at [https://nodejs.org].  If unsure, choose the LTS version (Long Term Supported) over the version with the newest features.  Often new features end up breaking other NPM packages that are not yet updated for the new changes.   

**Note:** If `touch` command not available, be sure to download it or use a command prompt that supports the command -- aka if you're on Windows, download Git Bash to get Linux commands enabled on your prompt.  If on Linux, use your Linux package manager (apt-get, yum) to download touch*.

*It should be noted as a best practice, to never use brand new features in production as there could be untested security issues, bugs or incompatibilities that could break your website.  That said, if you know what you are doing and unit test properly, you can skip this best practice suggestion.*

## The Easy Way (Use NPX)

The easy way is the preferred way to start a new React project. This way you can focus on creating an application with React first, then just drop the output file into a larger project if you wish (or leave it standalone).  If you are interested in the manual way (because you want to understand what's going on or have a bigger node project) check out the file `install-react-manually.md` one level above this folder.

Open the command prompt to a directory of your choice and type

```
npx create-react-app hello-world
cd hello-world
yarn start
```

**Note:** Sometimes you'll see developers use (Yarn)[https://yarnpkg.com] as opposed to NPM.  `yarn start` and `yarn build` are pretty much interchangeable for NPM commands, while `yarn add <package name>` is a drop in for `npm install <package name>`.   Yarn is touted to be more efficient than NPM and removes redundancies, so check it out!

You should see many packages being downloaded and installed to your project folder, then after you `cd` into the project and `yarn start`, more packages will be loaded and your default browser will open up a webpage with a spiraling React logo.  If you see the logo, you are good to go.   

Be sure to open up a separate tab or command prompt to this same directory to be able to run the instructions given below while your first tab or command prompt continues to run the built in server that watches your files and refreshes on changes of code.

**Note:** If you didn't use `create-react-app` and you started manually, you won't see the React logo, rather just your port will be running from `http://localhost:{PORT_NUMBER_YOU_CONFIGURED}`.  This is normal, so feel free to keep going thru the tutorial and modifying directions as needed to keep up.  You'll need to create all the files shown below on your own, but you can copy and paste many of the examples or also run `npx create-react-app` in a separate directory, then resource those files created as needed to work manually here.

### Hello World!

Go to your `src` folder and let's edit the `App.js` file.

Replace this:

```javascript
function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}
```

with this:

```javascript
function App(){
    return (
        <div>
            Hello World!
        </div>
    )
}
```

Essentially the function `App` is the starting point for your React application; it's what gets loaded into the `index.js` file if you go and look at the `import` section there.   The `App` function is called a **Functional Component** in React language.  Its only job is to return some HTML modified by some JavaScript and the use of `props`.   

We'll explore what all these terms mean later, but for now the main idea is whatever you want to return to your application comes from this `App` function.   

You can `import` other functional components into your `App` like a `header` component or a `footer` component; both which live in their own separate `.js` file.  If you are struggling with the ES6 syntax, I highly recommend you brush up on your ES6 with JavaScriptLA's playlist series: [JavaScriptLA Guide To ES6](https://www.youtube.com/watch?v=T9xHk8uNjSg&list=PLzVJ4BkY_MBDdsRI9GaKNDq3Gsfv41o14).  

Long story short though, you export code from other `.js` files as code components to be used using `<ComponentName />` HTML looking syntax (called JSX) to place in anywhere with your resulting `App.js`.   Think of components as placeholders for HTML and JavaScript tied together.  We'll explore all that soon in the `movie-search` project with this tutorial.  Components must always have a closing symbol `/` if they are singular, or you can also end a component by ending it like HTML like so: `<ComponentName>..some content...</ComponentName>`.

Under the hood, `create-react-app` will pull together all the component files and concatenate them together to build the final `App` output that gets returned from your `App` function like so:

`App.js`
```
import Header from './header'
import Footer from './footer'

function App(){
    return (
        <>
            <Header />
                <div>Hello World</div>
            <Footer />
        </>
    )
}
```

**Note:** the `<></>` syntax is not valid HTML, but in React this is okay, it's called a **Fragment** operator.  In React, all components must be wrapped in a parent div, otherwise React will throw an error.  A fragment is a quick way to wrap your components together.  Alternatively, you can just return one HTML div / section only as a component

You may notice an error on your webpage, saying something like `header.js` and `footer.js` cannot be found.  That's okay, ignore the error for now.   Go ahead and create two files called `header.js` and `footer.js` and make sure they are in your `src` folder.  You can use the following code to drop into your `header.js` and `footer.js` files respectively.


`header.js`
```
import React from "react"

function Header(){
    return (
        <header>
          This is the header
        </header>
    )
}
export default Header
```

`footer.js`
```
import React from "react" 

function Footer(){
    return (
        <footer>
          This is the footer
        </footer>
    )
}
export default Footer
```

**Note:** you need to import React from "react" to get the React library so that you can return HTML (technically JSX).  Not doing so is a JavaScript error, as JavaScript doesn't know how to return HTML, it can only return JavaScript syntax.  Through the React library, React is able to find any JSX syntax, convert it into React JavaScript functions, and then replace the return value as correct JavaScript syntax your browser will understand.  Ultimately, when you write <footer>Some content</footer>, you're actually writing `React.createElement('footer', 'Some content', {/* additional properties */})`.

However, writing out this as JavaScript would be annoying each time we want to create elements, so instead Facebook created JSX as a shortcut to allow us to write HTML instead that gets swapped out for these more mundane functions under the hood.  Long story short, you don't need to write out all the JavaScript, you can use JSX, but realize you always need to import React to be able to return HTML syntax inside your JavaScript -- or you'll get an error.

After saving both these files, you should notice your page working again (hit refresh if for some reason this fails).  If all good you should see: 

This is the header
Hello World!
This is the footer

If you want, you can rearrange the code like so, and you should see:

```
function App(){
    return (
        <Footer />
           Hello World!
        <Header />
    )
}
```


This is the footer
Hello World
This is the header

As you can tell React makes it easy to rearrange and reuse components as often as you like.  Think of React components like Lego blocks, you just write your HTML somewhere else separately, then edit that file whenever you need to, and it will get updated wherever you used the `<ComponentName />` tag.  Pretty awesome right?

Finally, you might be wondering how the `App` gets loaded into your HTML webpage.  

Take a look at the `index.js` file.  

You should see the following imports at the top:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
```

These `import` statements tell our `index.js` file to load React, ReactDOM, our `index.css` as well as our `App.js` file.   It's through these import statements that React is able to load up our own `App.js` content, process it with the React library, then use ReactDOM to inject our code along with css to a target element we select on our HTML page.

```javascript
ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

This code here basically to load our `<App />` in React Strict Mode to a target element on our HTML `index.html` page (or whatever div you choose on some website), with an id of `root`.

You can change `root` if you want to something else, just make sure you change the div on the `index.html` page so you can see your App content.   You might be wondering where `index.html` is.   Let's check the `public` folder.  If you look inside, you'll see the `index.html` file, that if you scroll through has a div with id `root`.   That's where React will drop your React `<App />` code into.

You also need to make sure React is loaded into this index.html webpage, but since you used `create-react-app` to install this application, the NPM package already took care of that for you.

If you are doing this manually instead, you'll need to make sure that whatever bundle file you created (if you used Webpack to create a bundle.js file), is added to the final `index.html` file so your scripts can be loaded in and target the `root` element.  

Alright, hope this tutorial was helpful for getting started with React!  If you feel good about everything thus far, feel free to move on to the `movie-search` project folder where we will explore even more React terminology, some more items under the hood, and essentially build a search widget that can display the latest movies from Open Movie Database API.