---
layout: post
title:  "Handling events in React"
subtitle: "How to support DOM events in React"
author: "Stéphane Bégaudeau"
date: 2018-10-15 11:22:37 +0200
image: "/img/posts/2018/10/15/handling-events-in-react/react-preview.png"
include_obeo_rss: false
---

Now that have seen how to create [stateless and stateful React components](/2018/10/08/first-react-component.html), we are going to see how to handle DOM events with a class-based component. You can handle events with React in a similar fashion as with DOM elements with some minor differences.

With JSX, you will have to use properties using the camel-case version of the name of the event that you want to handle. On top of that, you won't give this property a string with the JavaScript to call but instead a reference to the function to execute.

In this example, showing regular HTML and JavaScript, we have to use the ```onclick``` attribute and a string containing the JavaScript expression that we want to execute when the button will be clicked.

```
// DOM, onclick uses a string
<button onclick="doSomething()">
  Do Something
</button>
```

While in JSX, we will handle a click on the button thanks to a JavaScript-like API which requires us to use the camel-case name of the event ```onClick``` and a reference to the function to be executed.

```
// JSX, onClick uses a function
<button onClick={doSomething}>
  Do Something
</button>
```

## Binding

Most of the time, you will use methods to handle events in order to update a state in a class-based component. In such situation, the method used will need to be bound to the class manually. Otherwise ```this``` will be undefined inside of it.

```
class Counter extends Component {
  constructor(props) {
    super(props);

    // Make sure that "this" is available in "handleClick"
    this.handleClick = this.handleClick.bind(this);

    this.state = { count: 0 };
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Count: {this.state.count}
      </button>
    );
  }

  handleClick() {
    this.setState(prevState => ({ count: prevState.count + 1 }));
  }
}
```
You could be tempted to write your event handler by binding it directly in the JSX code but you should be careful, such solution would work but it would also create a new instance of the function each time the component is rendered. As a result, you would most likely encounter a performance issue later.

```
class View extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  render() {
    // Do not bind the method here to prevent performance issue
    return (
      <button onClick={this.handleClick.bind(this)}>
        Count: {this.state.count}
      </button>
    );
  }

  handleClick() {
    this.setState(prevState => ({ count: prevState.count + 1 }));
  }
}
```
You may have noticed that in the previous example, we have used ```setState``` in a new way by using a function as the argument instead of a value.

## Updating the state

In the [previous article](/2018/10/08/first-react-component.html), we have seen that we can call ```setState(newState)``` to create a state change which will update the state with the value of ```newState```. We have also seen that we can specify a callback to be used once the state has been updated, for example:

```
doSomething() {
  const newState = { "key": "value" };
  this.setState(newState, () => {
    console.log('The state has been updated');
  });
}
```

In our ```Counter``` component, we have used the method ```setState``` in another way. In this component, we need to compute the state by incrementing a value from the previous version of the state. In such situation, you should always use a function with ```setState```. This function will give you access to the previous state to compute the new version of the state.

You could try to write the following piece of code instead...

```
handleClick() {
  this.setState({ count: this.state.count + 1 });
}
```

...but you would end up with bugs in your component since ```setState``` is asynchronous. You would need to update your code to the following version.

```
handleClick() {
  this.setState((prevState) => {
    return { count: prevState.count + 1 };
  });
}
```

## Controlled components

In HTML, elements like ```<input type="text">``` or ```<textarea>``` maintain their own state directly in the DOM. Thanks to this, you could ask the DOM for the current value of a text field.

On the other hand, in React, the state is maintained in a class-based component using ```this.state``` and updated with ```setState```. As a result, for some DOM elements like text fields, we have to decide where the source of truth will be. Most of the time, you should use the React state as the source of truth and thus create what is called a controlled component.

In the following example, we have a text field using an ```<input type="text">``` element where the value is computed from the state of the component.

```
class Text extends Component {
  constructor(props) {
    super(props);
    this.state = { value: 'Initial Value' };
  }

  render() {
    return <input type="text" value={this.state.value}/>;
  }
}
```

If we want to create a proper controlled component, we have to handle user interactions to update the state accordingly. With a text field, we will have to handle the fact that the user can change the value by typing some new text. We can handle this scenario by adding an ```onChange``` event handler on the input which will update the state of the component.

```
class Text extends Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = { value: 'Initial Value' };
  }

  render() {
    return <input type="text" value={this.state.value} onChange={this.handleChange}/>;
  }

  handleChange(event) {
    this.setState({ value: event.target.value });
  }
}
```

Each time the user will type in the text field, the method ```handleChange``` will be executed, the state will be updated, the component will be re-rendered and the value of the text field will be updated.

## Uncontrolled components

Instead of managing the state in React, you could use the state of the DOM as the source of truth. With this approach, you would create an uncontrolled component which would have to keep a reference to the DOM element to access its state.

You may use uncontrolled components if you have to interact with existing frameworks which may maintain some complex state in the DOM. A class-based uncontrolled component is an example of a relevant class-based component which would stay stateless with regard to React.

To retrieve the state of the DOM, you can use the ```ref``` property with a function to set an attribute of the class to the DOM element. Then after the first rendering, you can easily access this DOM element to retrieve some information.

```
class Text extends Component {
  render() {
    return <input type="text"
                  defaultValue="Initial Value"
                  ref={(input) => this.input = input}
                  onChange={this.handleChange}/>;
  }

  handleChange(event) {
    console.log(this.input.value);
  }
}
```

Starting with React 16.3, you can use ```React.createRef()``` to create a reference to the DOM element much more easily. Using this approach, we can retrieve the DOM element using the field ```current``` in our reference. As such, the previous example could be written like this:

```
class Text extends Component {
  constructor(props) {
    super(props);
    this.input = React.createRef();
  }
  render() {
    return <input type="text"
                  defaultValue="Initial Value"
                  ref={this.input}
                  onChange={this.handleChange}/>;
  }

  handleChange(event) {
    console.log(this.input.current.value);
  }
}
```

In this article, we have seen how to handle events in a React application and the various approaches available to use React or the DOM as the source of truth. In the next React article, we will have a look at some popular React design patterns. For more information regarding React or to be sure not to miss an update, you can find me on [twitter](https://www.twitter.com/sbegaudeau).