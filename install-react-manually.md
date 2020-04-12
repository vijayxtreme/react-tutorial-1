## The Manual Way Using NPM (once you've installed Node.js)

*This method is meant for those who want to know what's happening under the hood of the command `npx create-react-app`.  You may as a result of understanding want to swap out certain commands -- maybe you don't like Webpack or want to remove "code bloat".  You may also be writing a larger application with Node on the backend using many other dependencies, and thus just want the packages you need to install locally so you can get going without creating a brand new folder.*

Open the command prompt to a directory of your choice and type:
```
mkdir hello-world
cd hello-world
mkdir public
touch public/index.html
mkdir css
touch css/style.css
mkdir src
touch src/index.js
npm init 
```

We need a `public` folder, a `src` folder and an `index.js` inside our src folder, where we will play around with React and ES6.  I added in the public folder and css folder (if you wanted to optionally have css files and an index.html file to play with).  Our project will use Webpack to look through all our files in the `src` folder, concatenate them together, then create a `dist` folder where the files will be output as a single file called `bundle.js`.  Webpack can be configured to create a single HTML file (or you can do it yourself) that received the `bundle.js` and targets the DOM element of your choice with React.*

Answer all the prompts after you type `npm init`.
*Alternatively you can type `npm init -y` to skip all the prompts and accept the default values*

Now your app has the ability to download packages from Node Package Manager (NPM).  We'll need to install the following dependencies to build a React application from scratch:

- (Webpack)[https://webpack.js.org/] (to search all our JS files, map them together with Babel, receive the output ES5 code, code split for unused JS, uglify and minify the build as a single bundle.js, then drop into a dist folder with HTML file ready to preview.)  You can use the bundle.js separately to drop in with any other larger application you might be running (perhaps with a CMS like Drupal/Wordpress)
- (Babel)[https://babeljs.io] (to transpile ES6 and JSX code to ES5 code)
- (React)[https://reactjs.org/] (of course, recommended latest stable version)
- (ReactDOM)[https://reactjs.org/] (uses React language to interface with DOM API, necessary to be able to drop components into your webpage)

*Note instructions can always go out of date at the time of viewing, so feel free to resource documentation at each of the websites mentioned above, each dependency has a clickable link*

Let's get the Webpack & Babel dependencies out of the way first:

### Webpack

```
npm install webpack webpack-cli --save-dev 
```

*Note: The `--save-dev` command after a package name means we want to install these packages for a local dev environment, but not for production.*

With Webpack, we want both the webpack package and the webpack command line interface (to make it easier to work with webpack from our command prompt/terminal using custom webpack commands).   You'll see the use of "cli-tools" a lot when working with code libraries.  This is because these commands are not supported by your Operating System by default; you have to install them.

From the command prompt, type:

```
npm install webpack-dev-server style-loader css-loader babel-loader html-webpack-plugin path --save-dev
```

In addition to Webpack and Webpack CLI, we'll also want to have Webpack-Dev-Server and some loaders installed to help Webpack traverse our project folder and files for matching JS, JSX, as well as CSS and HTML files to then compile together with Babel, create a build folder with relevant build files, and spin up a simple server that automatically opens a browser to our built files that reloads whenever we make changes to any files "watched" by Webpack, called "Hot Module Reloading".  `html-webpack-plugin` will autogenerate an index.html file for us in our output folder with the bundle.js added in as a script (technically you don't need this step, but it is a time saver).  The final command, install `path` allows Webpack to use the `path` variable offered by NPM to resolve our project folder's path address, which is needed to create a `dist` folder for our output files.

*I know, we're doing some really cool futuristic stuff here.  Or maybe you're not impressed, because you're from the future already. In that case, hope you brought back a cure for Goku's heart virus.*

Let's take a moment to create a `webpack.config.js` file we will revisit later in the tutorial below.

```
touch webpack.config.js
```

Since we made this file, let's go ahead and edit it so that it has the configuration it needs to search our project folder, files we want to load through Webpack Loaders (such as Babel-Loader).

Go to your `webpack.config.js` file and type the following (or copy/paste):

`webpack.config.js`
```javascript
const path = require("path")
const webpack = require("webpack")
const html-webpack-plugin = require("html-webpack-plugin")

module.exports = {
  entry: "./src/index.js",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        loader: "babel-loader",
        options: { presets: ["@babel/env"] }
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"]
      }
    ]
  },
  resolve: { extensions: ["*", ".js", ".jsx"] },
  output: {
    path: path.resolve(__dirname, "dist/"),
    publicPath: "/dist/",
    filename: "bundle.js"
  },
  devServer: {
    contentBase: path.join(__dirname, "public/"),
    port: 3000,
    publicPath: "http://localhost:3000/dist/",
    hotOnly: true
  },
  plugins: [
      new webpack.HotModuleReplacementPlugin(),
      new HtmlWebpackPlugin({
          title: "Hello World"
      })
    ]
};

```

So a lot going on here, let's break it down:

The first three lines tell Webpack we need the following JavaScript modules to be loaded in before Webpack can get to work:

```javascript
const path = require("path")
const webpack = require("webpack")
const html-webpack-plugin = require("html-webpack-plugin")
```

Next, we export a Webpack configuration object through `module.exports`, that Webpack will use to source our files and start "building" what it needs to build as defined by the configuration object.   

The entry point for Webpack is configured to be:
`entry: "./src/index.js"`, which just means Webpack will start at the `index.js` file first, look for any code as well as other imported code using `import` statements, traverse those files as well and essentially put together a list of files to perform work on.

Inside the `module` property, we see an object with a `rules` property.  This tells Webpack what to do when it comes across a file it's going to work on.  `rules` is an array of commands, you can see that the first object in the array is one that uses Regular Expressions to test for the presence of `.js` or `.jsx` files.  The `exclude` property tells Webpack to not look through our `node_modules` folder (because we only care about our project and this would be a huge waste of time).  

*Note: You can configure the regex as you like especially if you have other files/folders you want to ignore.*

After we exclude folders we don't want, we tell Webpack what to do after finding a matching `.js` or `.jsx` file.  In this case, we want to transpile it using Babel, so we pass in `babel-loader` to the `loader` property.  We can configure the `loader` with more options via the options property, so we tell it to be configured as an object with presets for `@babel/env` which you will remember as ES6 preset definitions.

Next, you'll see another test object in our `rules` array that looks for `.css` files.   We tell Webpack to use the `style-loader` and `css-loader` to essentially load our CSS into the DOM (which makes for faster webpage loading than thru an external stylesheet), and allows us to include CSS with our JavaScript files as imports (rather than adding them manually).  Behind the scenes Webpack is still manually stitching everything together, but hey -- you don't have to anymore, and that's a huge time saver.  Now you probably understand why build tools are better than doing things by yourself!

- (Style Loader)[https://www.npmjs.com/package/style-loader]
- (CSS Loader)[https://www.npmjs.com/package/css-loader]

*Note if you don't want the `css-loader`, or `style-loader`, you do not have to use them.  You can also configure Webpack to load asset files externally if you wish rather than injected into the DOM with `style-loader` if that's what you want too. These loaders are optional for this project, and that's why I'm showing you them manually under the hood with this tutorial.  That said you get all this for free with `npx create-react-app`*

Next, we tell Webpack which extensions we want to `resolve`, in this case we are just using wildcard statements to basically search everything using the `*` symbol.   `*.js` means all files with a JavaScript extension, `*.jsx` means all files with a JSX extension, and of course all of the files we want to resolve are passed in through an array of extensions to resolve.  Without this, we'd have to suffix any file imports in our project with extensions; so `import 'App' from ./App` as opposed to `import 'App' from /App.js`.

The `output` property tells Webpack where we want to drop our processed files; here we are saying that we want the `path` to be our directory name, aka `__dirname` (a special variable holding our directory's path in Node), PLUS a folder called `dist`.  You can change `dist` to be whatever you want, some coders use the term `build`.  The filename for the processed JS file that includes all your dependencies after Webpack is finished is called `bundle.js` but you can change that as well to whatever you like.

The `devServer` is used for configuring `webpack-dev-server` which we downloaded above to help us "watch" files and folders that we make changes to, process them, then spin up a local server at port `3000` on our computer we can access through a browser via the URL `http://localhost:3000`.  You configure the `devServer` to have content pulled from `contentBase` (so your public folder and files inside), as well as the `publicPath` to have your `dist` folder be the path the URL actually points to.  You can change the `port` to be whatever you want, it doesn't have to be `3000` if you have something else running there.  The last flag, `hotOnly` set to `true` enables Hot Module Reloading so we don't have to constantly hit refresh on our browser everytime we make a change.

*Note: If you are doing this tutorial on a cloud server like AWS, be sure that you have security settings updated so you can enable whatever ports you are trying to access via the Web (by default HTTP ports are just port 80 and 8080).*

*Webpack is a lot of work initially, but the more you do it, the easier it gets.  That said, the team at React (as well as many other major companies) now give you Node Package Executable Commands (`NPX`) that do all this legwork for you; rather than you worrying about what dependencies you need, you let a team like Facebook figure it out, then accept whatever they think as the defaults.  Again though, you are responsible for the way your app behaves in the end, so it's always a good idea to look under the hood and kick the tires every once in a while.  Over time, if you know what you are doing, you can write scripts yourself to automate this process as well.*

Before we get to Babel, let's do one more thing:

In your `package.json` file, let's add to our `scripts` property the following NPM scripts, so that we can just run Webpack via `npm start`.  If we don't do this, then when we need to use webpack, we'll have to write out `node_modules/bin/webpack` each time we want to use it to build our project.  Additionally, I've separated out two scripts, `start` and `build` so when we want to use `webpack-dev-server` we use `npm start` to run the local server in development mode, while if we want to just build everything up for production as a single dist folder, we use `npm build`.   You'll see this pattern a lot especially when you use toolchains like (Next.js)[https://nextjs.org] and (Gatsby)[https://gatsbyjs.org].

`package.json`
```javascript
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack-dev-server --mode development",
    "build": "webpack --mode production"
  },
```

*Note: Sometimes you'll see developers use (Yarn)[https://yarnpkg.com] as opposed to NPM, especially if you use (Gatsby)[https://gatsbyjs.org].   `yarn start` and `yarn build` are pretty much interchangeable for NPM commands, while `yarn add <package name>` is a drop in for `npm install <package name>`.   Yarn is touted to be more efficient than NPM and removes redundancies, so check it out!`*

You're good to go now with Webpack, but before you start, you still need to install and configure Babel!

### Babel

After installing webpack, let's get Babel:

```
npm install --save-dev babel-loader @babel/core @babel/preset-react @babel/preset-env @babel/cli
```

Here we are saying we want to download the library for Babel Core (you need it to transpile any unknown JS syntax especially ES6 or beyond).  We also want the presets (think dictionary definitions) for React and ES6, so our Babel core can load these definitions up and transpile code to matching ES5 definitions.   `preset-react` helps transpile `JSX` into JavaScript, while `preset-env` transpiles ES6 into ES5 code.

*Remember, ES6 is not yet fully supported by browsers or even Node.js, especially older browsers and older servers running Node.   If you try to run ES6 without Babel, your application will throw an error.  You need to transpile it to older ES5 code, which Babel is excellent at. Don't worry, your ES6 code will still mean the same thing when mapped to more difficult to read but machine processed ES5 code.*

After downloading Babel, let's be sure to create the necessary `.babelrc` file that Babel needs from us to launch itself.  In this file we specify to Babel which presets we want it to load into it (otherwise Babel won't know what you want to load in-- downloading the package isn't enough, you have to configure Babel too):

```
touch .babelrc
```

Since this was easy, let's take a moment to fill in the `.babelrc` with the configuration we want:

Go to your .babelrc and paste in the following `JSON` conifguration:

```javascript
{
    "presets": ["@babel/env","@babel/preset-react"]
}
```

Here we are saying we want Babel to load in the definitions for ES6 and React (JSX) via `JavaScript Object Notation (JSON) syntax` as an array for the "presets" property.   You can load even more definitions if you wish so long as you download the library and add them to the array in the same way as shown above.

With Babel and Webpack setup, we are one step closer to the fun part, which is React. Let's go~

### React

Let's get React and React-DOM saved as well to our project:

```
npm install --save react react-dom
```

*Here we are saying we want the latest version of React and React DOM.*

All good here -- no config needed to use React, you've already done that with Webpack and Babel!  Just go to your `src` folder and start building away.  

If you got this far, go ahead and start the Hello World folder exercise included with this project.