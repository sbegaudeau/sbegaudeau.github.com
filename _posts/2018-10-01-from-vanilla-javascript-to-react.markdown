---
layout: post
title:  "From Vanilla JavaScript to React"
subtitle: "Understanding React from scratch"
author: "Stéphane Bégaudeau"
date: 2018-10-01 11:25:37 +0200
image: "/img/posts/2018/10/01/from-vanilla-javascript-to-react/react-preview.png"
include_obeo_rss: false
---
React is a JavaScript framework used to build user interfaces. It can be used to create JavaScript applications by manipulating dynamically the content of the page. The browser already provides an API to create elements in the page, the DOM, so newcomers may wonder what does React bring to the table and how it relates to the DOM.

# Vanilla JavaScript and the DOM

In JavaScript, just like in most programming language, you will have access to a global scope with various objects and functions that you can manipulate to build your application. In a JavaScript application running in a Web environment, you will have access to the Document Object Model (DOM) API. If you were using JavaScript in a Node-based application, you would not have access to the DOM but you could import an alternate implementation like [JSDOM](https://github.com/jsdom/jsdom).

The DOM is a simple API which let you manipulate the HTML document of a page in pretty much any way you want. You can start using it thanks to the global ```document``` object.

Starting with ```document```, you can easily [create new elements](https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement), [modify their attributes](https://developer.mozilla.org/en-US/docs/Web/API/Element/setAttribute) or even [add them as a child](https://developer.mozilla.org/en-US/docs/Web/API/Node/appendChild) of other elements. Thanks to the DOM, you can create programmatically any HTML document, even it would be quite verbose to do so.

In the example below, we will create programmatically a simple title in our HTML document.

```
<!DOCTYPE html>
<html>
  <head>
    <script src="app.js"></script>
  </head>
  <body>
    <div id="app" />
  </body>
</html>
```

For that, we will create an ```h1``` element which will be inserted in the body of our HTML page.

```
// The document object is accessible since it is in the global scope
const h1Element = document.createElement('h1');
h1Element.setAttribute('class', 'title');
const textElement = document.createTextNode('I am Groot');
h1Element.appendChild(textElement);

// document.getElementById('app') will retrieve the div with the identifier app
document.getElementById('app').appendChild(element);
```

The code above start by creating a new ```h1``` then it adds a new ```class``` attribute with the value ```title``` to this element. It also creates a simple text node and adds the text as a child of the ```h1``` element. Finally it adds the ```h1``` to the div with the identifier ```app``` of the HTML document. After the execution of this code, the resulting HTML document will look like this:

```
<!DOCTYPE html>
<html>
  <head>
    <script src="app.js"></script>
  </head>
  <body>
    <div id="app">
      <h1 class="title">I am Groot</h1>
    </div>
  </body>
</html>
```

With the DOM, we can also manipulate the class attribute of an element directly thanks to the property [className](https://developer.mozilla.org/en-US/docs/Web/API/Element/className) (since the name ```class``` is a reserved keyword in JavaScript). As such, the following code would produce exactly the same result.

```
const h1Element = document.createElement('h1');
// h1Element.setAttribute('class', 'title');
h1Element.className = 'title';
const textElement = document.createTextNode('I am Groot');
h1Element.appendChild(textElement);

document.getElementById('app').appendChild(element);
```

# The foundations of React

Most React tutorials will let you start by using all the wonders of React directly. We will go for another approach as we will start by writing some React code that you may never write ever again in order to have a better understanding of the way React works.

React has been created with the web in mind and as such, at its core, some of its API feel like the DOM. To illustrate this, we will have a look at one of the most important React API, ```React.createElement```.

To manipulate the DOM with React, you will need two dependencies ```React``` and ```ReactDOM```. ```React.createElement``` will let you create a cheap and fast data structure called the virtual DOM representing the structure of your user interface. ```ReactDOM``` will render this virtual DOM in the real DOM of your web application.

```React.createElement``` will need three arguments to create an element of the virtual DOM:

- the name of the element to create
- its properties
- its children

```
import React from 'react';

const name = 'h1';
const props = { className: 'title' };
const children = 'I am Groot';
const element = React.createElement(name, props, children);
```

```React.createElement``` can also accept an array containing all the children of the element to create.

```
import React from 'react';

const name = 'h1';
const props = { className: 'title' };
const children = ['I am Groot'];
const element = React.createElement(name, props, children);
```

The argument ```children``` is also a regular property of the element and as such it can be part of the ```props``` object.

```
import React from 'react';

const props = {
  className: 'title',
  children: ['I am Groot']
};
const element = React.createElement('h1', props);
```

In order to render this element in the DOM, we need to select where in the DOM it will be rendered, in our case in the ```div``` with the identifier ```app``` and tell ReactDOM to render it.

```
import React from 'react';
import ReactDOM from 'react-dom';

const props = {
  className: 'title',
  children: ['I am Groot']
};
const element = React.createElement('h1', ...props);

ReactDOM.render(element, document.getElementById('app'));
```

All the code samples shown here can be tested by using them with the unpackaged version of React and Babel. Such a configuration should only be used for simple tests since they are not as optimized as a production build would be. In this specific situation, both imports of ```react``` and ```react-dom``` should be removed (both are exposed as global variables here).

```
<!DOCTYPE html>
<html>
  <head>
    <title>React</title>
    <script src="https://unpkg.com/react@16/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>

    <script type="text/babel">
    const props = {
      className: 'title',
      children: ['I am Groot']
    };
    const element = React.createElement('h1', props);

    ReactDOM.render(element, document.getElementById('app'));
    </script>
  </head>
  <body>
    <div id="app" />
  </body>
</html>
```

# JSX for all

While we could create all the pages of our web applications using this approach, it would still be quite verbose. In order to make it easy to manipulate the DOM, React comes with a simple and powerful language named [JSX](https://facebook.github.io/jsx/).

JSX is used by preprocessors to be converted during the build into regular JavaScript. A regular React project uses a preprocessor in order to transform JSX code into calls to ```React.createElement```. As such, JSX is never directly interpreted by React and you could use React without a single line of JSX. The two pieces of code below are thus exactly the same. First, using React programmatically:

```
import React from 'react';

const props = { className: 'title' };
const children = ['I am Groot'];
const element = React.createElement('h1', props, children);
```

or declaratively with JSX:

```
import React from 'react';

const element = <h1 className="title">I am Groot</h1>;
```

Since the JSX code will be converted to calls using ```React.createElement```, you will need to import React even if it doesn't seem to be used.

Using JSX you can declaratively create a large part of the DOM very quickly while React will only see calls to ```React.createElement```. Since a JSX element is only a call to ```React.createElement```, children is still a regular property. As a result, you could also write the previous example like this:

```
import React from 'react';

const element = <h1 className="title" children="I am Groot"/>;
```

With JSX, you have access to variables thanks to curly braces:

```
import React from 'react';
const title = 'title';
const text = 'I am Groot';

const element = <h1 className={title} children={text}/>;
```

We could also, of course, name our variable like the properties that we want to manipulate

```
import React from 'react';

const className = 'title';
const children = 'I am Groot';

const element = <h1 className={className} children={children}/>;
```

This would allow us to use the [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) to obtain a more concise code

```
import React from 'react';

const props = {
  className: 'title',
  children: ['I am Groot']
};
const element = <h1 {...props}/>;
```

Finally, we can render this element inside of the DOM in a similar fashion as with ```React.createElement``` before.

```
import React from 'react';
import ReactDOM from 'react-dom';

const props = {
  className: 'title',
  children: ['I am Groot']
};
ReactDOM.render(<h1 {...props}/>, document.getElementById('app'));
```

Now that we have rendered our first piece of virtual DOM with React using JSX, we are ready to see how we could build a basic application using React. Next week, we will see how to move forward with more dynamic code using React components.

For more information regarding React or to be sure not to miss an update, follow me on [twitter](https://www.twitter.com/sbegaudeau).