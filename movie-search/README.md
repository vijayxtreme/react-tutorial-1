# Getting Started

In this follow up exercise, you'll build a simple Movie Search application that uses React with the [oMDB API](https://www.omdbapi.com/) (Open Movie Database)

This tutorial will get into more concepts of React, so please be sure you are good to go with getting started when building a new React application.  

To review, you can either use `npx create-react-app <my-folder-name>` to start a project, or you can do it manually using [Babel](https://babeljs.io), [Webpack](https://webpack.js.org/), and then get the packages for [React](https://reactjs.org) via `npm`.  Check the file `install-react-manually.md` one level above this folder for more details.

For simplicity, we'll use `npx create-react-app`, as we are not building any other large Node application with our React file.  I recommend this method especially if you are going to "sandbox" something on your own, then take the concept to drop in on another larger project.  If you are doing this manually, be sure to reference code below, create files and folders as needed, and run your build environment as needed to get the same output here. 

**Sandboxing** is a great way to come up with new MVP (minimum viable product) ideas without interfering with normal production; once you get buy in-- you can then implement the feature into a larger code base.

---

## Create The Project

Be sure to find your project folder, then go ahead and enter in the following commands:

```
npx create-react-app movie-search
cd movie-search
```

At this point you can also open up the project in your favorite code editor.  I usually use VSCode, so I just use the command `code .` to open up my editor to the current folder I'm in on the terminal.  

**Note:** You may need to install code in your PATH to make this work if you are on a Mac or Linux machine.  Here are instructions: [https://code.visualstudio.com/docs/setup/mac]

Let's go to our `src` folder.  We already have an `App.js` but we can remove all the content there and replace it with like so:

`src/App.js`

```jsx
import React from 'react';
import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div className="App">
      My Movie App
    </div>
  );
}

export default App;
```

Remember, we need to `import` React from the `react` library given to us via NPM.  This makes React available as a JavaScript module (or variable in this case), that will look at our code after it's been transpiled from JSX to JavaScript and return back a JavaScript function from our function `App`.  We make `App` exportable using the `export` keyword to the rest of our JavaScript files we create, so they can `import` in `App` and get its return value when used with React.   Thankfully we don't need to worry about all this when writing code, `JSX` is a shortcut for helping us write otherwise confusing JavaScript functions nested in more JavaScript functions.  In this way, code is easier to read and we can pretend our code is "HTML".  Always remember though, it's actually JavaScript.

```jsx
return (
    <div className="App">
      My Movie App
    </div>
  );
```

```jsx
return (
    React.createElement('div', {className:'App'}, "My Movie App") 
   );
```
^JSX vs JS React Code.  Both still do the same thing.

### Breaking Down Our App

Now that we know JSX is a time saver, let's start writing the rest of our Movie Search app with JSX.  Next, let's add the following files (either use your code editor or the command prompt).  I choose the command prompt.

First we need some fakedata to use while building our app.   Create a file called `movies.js` in your `src` folder, then paste in the following.  Later we will remove the fakedata and supply our app with real data via the oMBD API via `fetch`.

```
touch src/movies.js
```

Paste this in your `src/movies.js`: 

`src/movies.js`
```js
export default [
    {
        omdbID: '1',
        Title: "Star Wars I: The Phantom Menace",
        Poster: 'https://m.media-amazon.com/images/M/MV5BYTRhNjcwNWQtMGJmMi00NmQyLWE2YzItODVmMTdjNWI0ZDA2XkEyXkFqcGdeQXVyNTAyODkwOQ@@._V1_UX140_CR0,0,140,209_AL_.jpg',
        Year: 1999
    },
    {
        omdbID: '2',
        Title: "Star Wars II: Attack of the Clones",
        Poster: 'https://m.media-amazon.com/images/M/MV5BMDAzM2M0Y2UtZjRmZi00MzVlLTg4MjEtOTE3NzU5ZDVlMTU5XkEyXkFqcGdeQXVyNDUyOTg3Njg@._V1_UX140_CR0,0,140,209_AL_.jpg',
        Year: 2002
    },
    {
        omdbID: '3',
        Title: "Star Wars III: Revenge of the Sith",
        Poster: 'https://m.media-amazon.com/images/M/MV5BNTc4MTc3NTQ5OF5BMl5BanBnXkFtZTcwOTg0NjI4NA@@._V1_UY209_CR8,0,140,209_AL_.jpg',
        Year: 2005
    },   
    {
        omdbID: '4',
        Title: "Star Wars IV: A New Hope",
        Poster: 'https://m.media-amazon.com/images/M/MV5BNzVlY2MwMjktM2E4OS00Y2Y3LWE3ZjctYzhkZGM3YzA1ZWM2XkEyXkFqcGdeQXVyNzkwMjQ5NzM@._V1_UX140_CR0,0,140,209_AL_.jpg',
        Year: 1977
    },{
        omdbID: '5',
        Title: "Star Wars V: The Empire Strikes Back",
        Poster: 'https://m.media-amazon.com/images/M/MV5BYmU1NDRjNDgtMzhiMi00NjZmLTg5NGItZDNiZjU5NTU4OTE0XkEyXkFqcGdeQXVyNzkwMjQ5NzM@._V1_UX140_CR0,0,140,209_AL_.jpg',
        Year: 1980
    },
    {
        omdbID: '6',
        Title: "Star Wars VI: Return of the Jedi",
        Poster: 'https://m.media-amazon.com/images/M/MV5BOWZlMjFiYzgtMTUzNC00Y2IzLTk1NTMtZmNhMTczNTk0ODk1XkEyXkFqcGdeQXVyNTAyODkwOQ@@._V1_UX140_CR0,0,140,209_AL_.jpg',
        Year: 1983
    },
    {
        omdbID: '7',
        Title: "Star Wars VII: The Force Awakens",
        Poster: 'https://m.media-amazon.com/images/M/MV5BOTAzODEzNDAzMl5BMl5BanBnXkFtZTgwMDU1MTgzNzE@._V1_UY209_CR1,0,140,209_AL_.jpg',
        Year: 2015
    },
    {
        omdbID: '8',
        Title: "Star Wars VIII: The Last Jedi",
        Poster: 'https://m.media-amazon.com/images/M/MV5BMjQ1MzcxNjg4N15BMl5BanBnXkFtZTgwNzgwMjY4MzI@._V1_UX182_CR0,0,182,268_AL_.jpg',
        Year: 2017
    },
    {
        omdbID: '9',
        Title: "Star Wars IX: The Rise of Skywalker",
        Poster: 'https://m.media-amazon.com/images/M/MV5BMDljNTQ5ODItZmQwMy00M2ExLTljOTQtZTVjNGE2NTg0NGIxXkEyXkFqcGdeQXVyODkzNTgxMDg@._V1_UX182_CR0,0,182,268_AL_.jpg',
        Year: 2019
    }
]
```

Next, let's create some component files so we can play with our data.

```
touch src/SearchForm.js src/MovieListings.js src/MovieListing.js
```

Three files are created.  Let's break this down.

Our `App.js` is where everything will finally be output together.  In the `hello-world` project, we created two separate files called `header.js` and `footer.js`.   These two files allowed us to drop in a `<Header />` component and a `<Footer />` component within our `<App />` component.   We could rearrange these different components as we liked and our `App` would change instantly.  We could also make local changes to the `header.js` or `footer.js` and we would see the changes updated everywhere in our App, allowing us to write components that are written one time, and reusable everywhere in our App.

This time, we are creating three new reusable components that we will use more powerfully within this `MovieSearch` app.

The `<SearchForm />` component will be used to render a search form within our app, that users can type into some movie names.   The `<SearchForm />` will then use JavaScript internally to get the typed movie name from the search form as a local JavaScript variable, then use a fetch request to search for the movie name via `oMDB` API.   (Go here to get your API key now: [OMBDI API KEY](https://www.omdbapi.com/apikey.aspx)).

After the `<SearchForm />` gets back a response from `fetch` (in the form of `JSON`), we can parse the results and send them up to our `App` via a handler function to the App's `state`.  (We'll review this part when we get to it.)  For now you can think of `state` as the data for an application; when it changes, the App must also change with it (and any components affected).  This changing of components is called re-rendering, and is what makes React very powerful compared to manually writing changes with JQuery or vanilla JS.  

(If you aren't sure what `fetch` is, please check out this video [https://www.youtube.com/watch?v=xy24AhZHG6U] from our playlist, [JavaScriptLA Guide To ES6](https://www.youtube.com/watch?v=T9xHk8uNjSg&list=PLzVJ4BkY_MBDdsRI9GaKNDq3Gsfv41o14)).   

The `<MovieListings />` component will house an HTML table of individual `<MovieListing />`s.  The individual <MovieListing /> will be an HTML row with the `movie.poster`, `movie.title`, and `movie.year`.  (However, feel free to add more fields as you like to the listing if you get the idea).

Let's go ahead now and write the code for each of these components:

#### Create A Movie Listing Component

`src/MovieListing.js`

```jsx
import React from "react"

function MovieListing(props){

    let {poster, title, year} = props

    return (
        <tr className='movie-listing'>
            <td>
                <p><img alt={title} src={poster}/></p>
            </td>
            <td>
                <p>{title}</p>
            </td>
            <td>
                <p>{year}</p>
            </td>
        </tr>
    )
}

export default MovieListing
```

Simple enough, this `<MovieListing />` receives data through the form of `props` through its function parameter.  `props` are exactly like parameters to a function in JavaScript: You can send values as `strings`, `numbers`, `functions`, `objects` to your component which will then have access to those values internally.   Remember, JSX actually looks like a JavaScript function when it is transpiled -- the syntax is: `React.createElement(element, props, ...children)`.  So it only makes sense you can access `props` internally.

Here we are saying we want to destructure `props` (using the ES6 destructuring syntax) to create three new local variables copied from the `props` object called `poster`, `title` and `year`.  We then return a table row in HTML that has an `img` tag that maps to the `poster` url and the `title` for its `alt` value.  `title` and `year` are also returned along side the image as separate paragraph tags inside our table row's columns.

In summary, we're returning a table-row with data filled from `props`.  How do we send the data? Check out the next component, `<MovieListings />` which houses more than one `<MovieListing />`


#### Create A MovieListings Component

`src/MovieListings.js`

```jsx
import React from "react"
import MovieListing from "./MovieListing"

function MovieListings(props){
    
    let { movies } = props
    if (movies.length > 0){
        let output = movies.map((movie, idx) => {
            if(movie === undefined){
                return ''
            }
            return (
                <MovieListing key={idx} poster={movie.Poster} title={movie.Title} year={movie.Year} />
            )
        })
        return (
            <table className="movies">
                <thead>
                    <tr>
                        <th>Poster</th>
                        <th>Title</th>
                        <th>Year</th>
                    </tr>
                </thead>
                <tbody>
                    {output}
                </tbody>
            </table>
        )
    }else {
        return (
            <div className="no-movies">No movies, why don't you search for something?</div>
        )
    }
}

export default MovieListings;
```

Breaking this component down, we see that this is also a functional component like `<MovieListing />`, that receives `props`.   However this time, we destructure and create a local variable called `movies` from `props`.   This `movies` variable represents a movies object passed to this component as props, which carries all the data about movies in an array of objects.  Very soon, we'll wire up our fakedata with this component from our `App` to see our `<MovieListings />` and its internal `<MovieListing />` creation in action.

That's what's going on anyway inside `<MovieListings />`.  After we get the `movies` from `props`, we expect an array of objects.  If the length of the array is greater than 0, we know we have some data we can iterate over.  

We use the native JavaScript `map` function to iterate over each `movie` in the `movies` array, as well as its index, aka `idx`.   `map` is a special JavaScript function that returns a new array of modified values.  

For example: 

```js
let newArr = [1,2,3].map((num, idx) => {
    console.log(num, idx)
    return num *2
})
console.log(newArr) //[2,4,6]
```

In our case, we're saying we want to return a new array of modified values each containing a `movie` mapped to `<MovieListing />`.   Notice how the props on `<MovieListing />` are being filled with movie data, which will get output as a HTML as we saw in the `movielisting.js` file (refer to the file if you need a refresher).

You'll notice we added a `key` for the `idx`.  React requires a key with unique value any time you are creating more than one component, such as a list item or a table row.  Not doing so will throw a soft error.  

We save the new mapped movies array as a variable called `output`.  Finally, we return from our `<MovieListings />` component an HTML table.   Notice that we put our output inside the HTML table's body using the `{}` tags.  You'll see this pattern a lot in React; retrieving data as an array via props, mapping the data invidividually to individual components with props (or just plain HTML), and then saving the new mapped data array to an output variable, before sending it to our return value as output to be rendered with `{}` tags.

Let's move on to `<SearchForm />`.

#### Create A SearchForm Component

`src/SearchForm.js`

```jsx
import React from "react"

function SearchForm(props){
    let {handleChange} = props
    return (
        <div className="search-form">
            <form>
                <input type='text' placeholder='Search movies' onChange={e => handleChange(e)}/>
            </form>
        </div>
    )
}

export default SearchForm
```

This component also is a functional component and receives `props` from a parent component (the `<App />`).   Here we see that we destructure `handleChange` which is a prop handler we will use to send data back to our App as a JavaScript event when someone types into our HTML `input` field.   

You can see that `<SearchForm />` returns a simple form with input field of type text; the one thing to notice is the `onChange` attribute.  This is a special React attribute that maps to a DOM element's natural `onchange` attribute in HTML.  You can tie in plain JavaScript functions to your React elements in the same way you can tie plain JavaScript functions to HTML using attributes.  Note that this isn't a `prop`, because props are for React components, while attributes are for HTML -- both read data, but in the case of React with `props` you can pass them down to child components, while attributes cannot be passed down.  If you are confused, remember, at this point we are returning HTML, not a component.   HTML does not have props. 

Here we are tying the `onChange` attribute to our prop handler function from above.  This is yet another React paradigm, where if something changes in a child component, we need a way to pass that change as an event up to parent components (like our App) that will know how to respond. 

Data in React flows from parent to child from `state`.  We will visit that term below in a minute.  However data can also be sent upwards from children to parent via events through `props`.   Let's return now back to our `<App />` where we will clarify further.

### Update Our App Component To Be Stateful

With our components now filled out, let's update our `App.js` file so it imports our newly made components `<SearchForm />` and `<MovieListings />`.  Note we don't need to import `<MovieListing />` -- why do you think this is the case?

We'll also load in the fakedata through an import statement, `import movies from "./movies"`.

Finally, we need to change our `<App />` to be a `class` Component instead of a functional component.  What this means now is that our `<App />` component is now responsible for more than just returning simple HTML, we need a way to work with the data being passed to and from the child components inside our application like the `<SearchForm />` component and the `<MovieListings />` component back up to this parent component.  

For now, let's update our code to look like this, and then talk about `state`.

`src/App.js`

```jsx
import React, {Component} from 'react';
import SearchForm from "./SearchForm"
import MovieListings from "./MovieListings"
import movies from "./movies"
import './App.css';

class App extends Component {
  constructor(props){
    super(props)
    this.state = {
      movies: movies
    }

    this.handleChange = this.handleChange.bind(this)
  }

  handleChange(e){
    let term = e.target.value
    console.log(term)
  }

  render(){
    return (
      <div className="App">
        <SearchForm handleChange={this.handleChange}/>
        <MovieListings movies={this.state.movies} />
      </div>
    );
  } 
}

export default App;
```

#### What is State?

You can think of `state` as the overall data for an application.  This makes sense, since we are using this application to find movies from the web.  As soon as we get the movies as data (even if it's fake), we want to store it in our `state`.  Whenever the data changes on the `state`, we want React to update our components that depend on the data with fresh new data (usually called **hydrating** or **hydrating an app**).   

#### What's a Class?

In using a `class` Component, we can set up the `state` of our application through a `constructor` which is similar to a JavaScript constructor in that it initializes values for the class we want to create.   Here we are saying we want to create a `class` called `App` which extends `Component` which comes from React.  (Notice that we now changed the `import React from 'react'` to `import React, {Component} from 'react'` at the top of the file).  

If you remember your ES6 class syntax, anytime we extend another class (in this case `Component`), our class `App` gets all the properties and methods from the parent class without us having to type any of it out.  In simple terms, `App` now has all the methods of a React Component Class, one of them being `render`.   

ES6 functions inside a class are also written a special new way instead of your typical function syntax -- again take note.  If this is confusing, be sure to check out JavaScriptLA's guide to ES6, viewable here: [JavaScriptLA Guide To ES6](https://www.youtube.com/watch?v=T9xHk8uNjSg&list=PLzVJ4BkY_MBDdsRI9GaKNDq3Gsfv41o14).

The key thing to observe is now that our `return` of HTML and React Components now lives inside the `render` function that belongs to our class (which was inherited through `React.Component`, or simply `Component` after we destructure it from the `React` object). 

Getting back to our `constructor`, notice that it also receives `props` from its parent class, and we call `super(props)` to run any methods on our `props` from the `Component` class before we do any of our own custom code.  After this, we create a new object called `this.state`.

Inside `this.state` which refers to the state of our application, we create a property called `movies`, which will represent movie data at the time the `<App />` is loaded.  Since we have no data, it makes sense to just give some fakedata for now, which is why we set `this.state.movies` to our imported `movies` variable from `./movies.js`.

You'll also note that we have another function called `handleChange(e)`.  We'll talk about that in a moment, but in order for us to use this function within our app, we need to `bind` JavaScript scope (aka `this`) to our class (so it gets the proper scope when called belonging to the class itself and not something else like `window` (at runtime)).  If you need a refresher on `this` be sure to check out [JavaScriptLA Guide To ES6](https://www.youtube.com/watch?v=T9xHk8uNjSg&list=PLzVJ4BkY_MBDdsRI9GaKNDq3Gsfv41o14).

Binding in classes is another paradigm of React, if you are going to create event handler functions, you need to `bind` them to your class -- however this is going away with the introduction of [React Hooks](https://reactjs.org/docs/hooks-intro.html) which is starting to take prevalence (out of scope for this lecture but stay tuned)!

Let's now return to our discussion on `state`.

#### How Does State Differ From Props?

`state` differs from `props` in that state is stored in one central area (usually the `<App />` or main component), and is passed to components as `props` typically.  `props` is like read-only data that components use to render their internal HTML (such as the `<MovieListings>` that creates an individual `<MovieListing>`).  `props` are local to the Component, and while we can make changes to the data being sent to the component internally, we can never modify the `<App />` state from a child component directly.  Rather child components must use `prop` handlers to notify the parent component that changes have been made, and then the parent updates its own state which forces a re-render of all children components.  

`handleChange` is a `prop` handler for the `<SearchForm />` component, and it will fire off whenever the input field inside `<SearchForm />` is typed into.   At the `App` level, the prop `handleChange` will call `this.handleChange` which belongs to the `App` class and will get the value of whatever is being typed at the moment via the JavaScript `event` object, in our case called `e` for short.   

In looking at our `App` `handleChange` function, we can see that we get the event as a parameter to our function called `e`, and we can grab the text being typed through `e.target.value`, which is the target element that created the event, aka the input, and its value inside (aka the text).  I've stored this value as a local variable called `term` inside the `App handleChange` function. 

### Let's Run The App

Okay, a lot of work and reading to get to this point.  But hopefully you now have a better understanding of how things work, especially functional vs class components in React.  

The moment of truth -- save and run your application.  By default it should be running, but if not, then go ahead and type the following in your command prompt:

```
yarn start
```

You can check `http://localhost:3000` or whatever port you set this app to run at to see the output.  Hopefully you should see your `<SearchForm />`, as well as `<MovieListings />` with some default data (Star Wars movies).

You can take a moment if you like to style this app with some CSS.  I've added this project on Github at [https://github.com/vijayxtreme/movieSearchReactNew], feel free to look through the folder and take the styles from the `App.css` area.

If all good, go ahead and type in the search box and check the console.  You should see the letters you type appearing in the console.  We're almost done!  We just need to wire up our app with fetch in the `handleChange` function on our `App`.  Let's do that now.

### Wire Up App with fetch

Return back to `src/App.js`

Go ahead and paste the following into the `handleChange` function.  

`src/App.js`
```jsx
handleChange(e){
    let term = e.target.value
    let key = config.omdbApiKey
    
    fetch(`http://www.omdbapi.com/?apikey=${key}&s=${term}`)
      .then(data => data.json())
      .then(data => {
        if(data.Search){
          this.setState({movies:data.Search})
        }
    })    
}
```

You'll also need to create a file called `src/config.js` and `import` it into your `src/App.js` file.  This is where you'll store your API key from oMDB API; and using a `.gitignore` file, set to ignore this file so it isn't shipped up with your codebase during production.  There's a lot of ways to hide API keys; this is a quick and dirty way of doing so.  

Make sure the `config.js` file is in your `src` folder or React may throw an error if you are using `create-react-app`.

`src/config.js`

```js
export default {
    omdbApiKey: `<YOUR API KEY HERE>`
}
```

`src/App.js`

```jsx
import React, {Component} from 'react';
import SearchForm from "./SearchForm"
import MovieListings from "./MovieListings"
import movies from "./movies"
import config from "./config"
import './App.css';
```
^ The top of your `<App />` component should now look like this.

Let's return back to our `handleChange` function.

```js
handleChange(e){
    let term = e.target.value
    let key = config.omdbApiKey
    
    fetch(`http://www.omdbapi.com/?apikey=${key}&s=${term}`)
      .then(data => data.json())
      .then(data => {
        if(data.Search){
          this.setState({movies:data.Search})
        }
    })    
}
```

Here we are saying that after we get the event `e`, we want to store `e.target.value` (aka the input's current value) as a local variable called `term`.  You could wire up `term` to `state` if you wish, I chose not to given that we have the `movies` state variable which is our primary concern.   I use the `config` variable which I imported from `./config` to get my API key and store it in another local variable called `key`.

Next, I call `fetch` and pass in a URL for searching oMDB API.  You can reference the API via [oMDB API](https://www.omdbapi.com/).   

Here I'm making a basic `GET` request to retrieve data from the API in the form of `JSON`.  To do so, I pass in the url along with ES6 placeholder tags using `${}` syntax to drop in my `key` and `term` which will be injected into the resulting URL string.   If all goes well, `fetch` will return my JSON using the `then` method as `data`.  I pass `data` to be converted to json using the `json()` method, `then` send the data as a `JSON` variable to an arrow function where I finally check to see if I have my `data.Search` result (which contains an array of movies), and if so, I updated my App's state using the `this.setState` method.  `data.Search` comes from oMDB API, if it's not present, then we don't do anything.

`this.setState` is a special method for changing the `state` of an application that comes from React.  You are not supposed to set `state` manually like `this.state.movies = data.Search` (you'd get an error).  This is called mutating `state`, where you modify its value and destroy any previous history `state` had.   

To prevent this and to preseve history with your React application, you must always use the `this.setState` method which will take your new `state` and concatenate it with previous states, but apply the new `state` to your application.  In this way, as you build more complicated applications, you can revisit old `state`, reset the `state` for an application, as well as bring `state` back up to current -- think of it like an Undo/Redo command for your React program.  (We'll visit this concept of `state` and `history` in another lecture).

Hence, the proper way to change the `state` of our application is to use `this.setState` and pass in an object to change the property you want in state to be a new value; in this case we want to change `this.state.movies` from being fakedata to our fetch data.  To do so, we call `this.setState({movies: data.Search})` from our `fetch` function.   

Doing so updates our `<App />` immediately, and re-renders all the `<MovieListings />`.  Try it out for yourself!  Search for some movies and watch your app re-render with new results!

### Clean Up

Let's finish this app by cleaning it so it's ready for prime time.  At the moment, it's working great but you'll notice that every time you type a letter it sends a `fetch` request.  Let's control how often we send `fetch` requests by using a library called `LoDash`, that gives lots of great JavaScript utility functions out of the box -- one in particular called `_.debounce`.

First go to your command prompt and type the following:

```
npm install lodash lodash-es --save
```

This will download the `lodash` library and the `lodash-es` library (for ES6) that you can then use to import all the lodash functions including `_.debounce` into your `<App />`. 

`src/App.js`
```js
to their definitions or references in the same repository. Learn more

import React, {Component} from 'react';
import SearchForm from "./SearchForm"
import MovieListings from "./MovieListings"
//import movies from "./movies" <-- you can now comment this out if you like
import _ from "lodash-es" //<-- add lodash-es to your import statements
import config from "./config"
import './App.css';
```

Here we are importing `lodash` as the variable `_` (looks like a low dash), which we can then call the `debounce` method with using `_.debounce`.

The `_.debounce` function takes another function, and suppresses it so it only runs every specified set of milliseconds.  In our case, we want to get the term from the search input, then make our fetch request, but _.debounce the fetch request so it only runs every say 300 milliseconds or so (that way we don't jam up the oMDB API server).   

Change your `handleChange` function to look like so and also add in the followng new function `handleChangeDeBounced`.  

`src/App.js`

```jsx
  handleChangeDebounced = _.debounce(term =>{
    let key = config.omdbApiKey
    
    fetch(`http://www.omdbapi.com/?apikey=${key}&s=${term}`)
      .then(data => data.json())
      .then(data => {
        if(data.Search){
          this.setState({movies:data.Search})
        }
      })
  }, 300)

  handleChange(e){
    let term = e.target.value
    this.handleChangeDebounced(term)
  }
```

You should now have two functions, `handleChange` which was our original function, and `handleChangeDebounced` which calls the _.debounce method on our `term` from `handleChange`, returns a function that calls the `fetch` request to oMDB API and sets the state of our this.state.movies; now waiting 300 milliseconds each time before making a new request -- so in this way, our function will wait for us to type a few more letters before making a request.  Feel free to play around with the milliseconds if you want to make the requests faster or slower.

Feel free to also clean up the app with some CSS.  If you need CSS, you can get it from Github at [https://github.com/vijayxtreme/movieSearchReactNew].  You can also cross reference this tutorial with the project file there especially if you made a mistake.

Okay, that concludes this tutorial on React!  You now have had a crash course in getting started, creating your first Hello World application, you also had a chance to learn about Webpack, Babel under the hood as well as make this awesome Movie App, which taught you about functional components, class components, `props`, `state`, `fetch` and so much more.  Feel free to star this project on Github, and if you read this on a blog, feel free to give it a like or comment!  Thanks for viewing!  See you next time!

Vijay,
Head Organizer,
[JavaScriptLA](https://javascriptla.net)