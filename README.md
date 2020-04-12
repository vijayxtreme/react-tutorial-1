# What is React?

- Popular JavaScript framework brought to you by Facebook
- Allows one to build web applications and websites much more quickly, efficiently through the paradigm of reusable components 

## Why not JQuery or just vanilla JS?

JQuery is a popular JavaScript library; it differs from React in that it is not a framework.  JQuery helps you write less JavaScript that is still cross-browser friendly and efficient.  

You can use JQuery with React, however given diminishing support for browsers like Internet Explorer, [you may not need JQuery](https://www.youtube.com/watch?v=PFUVmwmLIzY) given modern vanilla JavaScript has many more functions and features that do the same things much faster than JQuery.

As for why to use React over just plain vanilla JavaScript building a website; the answer is that React saves you a lot of time and overhead-- without it you would be responsible for writing a lot more code to keep track of updates to the webpage manually; especially each time something updated or changed.   

For example, if you've ever worked with a Wordpress site, you may notice lots of JQuery functions everywhere manually updating pieces of HTML on event changes; HTML that may no longer exist either after an "update".  It becomes a pain to hunt and find every snippet of JQuery and HTML mapped together over time.   React solves this problem by tying HTML and JavaScript together directly into a component file, which can then be dropped in place of HTML onto a webpage.  If you no longer need the component functionality, you can toggle it off from within the application instead of manually hunting around for HTML and separate JavaScript/JQuery functions through hundreds of different files and scripts.

## A Simple Intro To React Code Exercise

In this project you'll build two different apps.  The first app is a simple "Hello World" to get started.

The second app is a Movie Search application that will help you understand more of the Component based architecture in React.   You'll learn how to create HTML with React functions, then JSX, and then tie your HTML and custom JavaScript together in a component file that can be inserted wherever you like on a webpage.  

Both apps are written in EcmaScript 6, the latest specification of JavaScript (released 2015 and every year thereafter sometimes called ESNext).  You will also see the term ES5 used frequently; ES5 refers to EcmaScript 5, or basically the current supported version of JavaScript which has been available in all browsers since 2009.  

Need an intro to ES6?  Be sure to check out JavaScriptLA's playlist on ES6 via this link:
[JavaScriptLA Guide To ES6](https://www.youtube.com/watch?v=T9xHk8uNjSg&list=PLzVJ4BkY_MBDdsRI9GaKNDq3Gsfv41o14)

You will need some basic understanding of NPM / Node.js to work with these projects, however each project has detailed instructions in their README files on what to do to get started.   

If you don't want to use NPM, there are also instructions on how to add the scripts for React and ES6 transpiling manually as well for your website (especially in situations where you can't use NPM).  That said, you can always build locally with NPM and a tool like Webpack, Gulp/Grunt, Rollup, then drop the output ES5 script file into a website that you may only have front-end access to.
 