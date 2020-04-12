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

