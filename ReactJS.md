# Environment Setup

**Step 1**: Install 

[Node js]: https://nodejs.org/en/download/current/

**Step 2**: `sudo npm install -g create-react-app`

**Step 3:** `create-react-app my-app`

**Step 4:** `cd my-app && npm start`

# JSX Elements

React applications are structured using a syntax called **JSX**. This is the syntax of a basic **JSX element**.

```js
/* 
  JSX allows us to write in a syntax almost identical to plain HTML.
  As a result, JSX is a powerful tool to structure our applications.
  JSX uses all valid HTML tags (i.e. div/span, h1-h6, form/input, img, etc).
*/

<div>Hello React!</div>

/* 
  Note: this JSX would not be visible because it needs to be rendered by our application using ReactDOM.render() 
*/
```

JSX is the most common way to structure React applications, but it is not required for React.

```js
/* JSX is a simpler way to use the function React.createElement()
In other words, the following two lines in React are the same: */

<div>Hello React!</div>  // JSX syntax

React.createElement('div', null, 'Hello React!'); // createElement syntax
```

JSX is not understood by the browser. It needs to be compiled to plain JavaScript, which the browser can understand. The most commonly used compiler for JSX is called **Babel**.

JSX differs from HTML in several important ways

```js
/* 
  We can write JSX like plain HTML, but it's actually made using JavaScript functions.
  Because JSX is JavaScript, not HTML, there are some differences:

  1) Some JSX attributes are named differently than HTML attributes. Why? Because some attribute words are reserved words in JavaScript, such as 'class'. Instead of class, JSX uses 'className'.

  Also, because JSX is JavaScript, attributes that consist of multiple words are written in camelcase:
*/

<div id="header">
  <h1 className="title">Hello React!</h1>
</div>

/* 
  2) JSX elements that consist of only a single tag (i.e. input, img, br elements) must be closed with a trailing forward slash to be valid (/): 
*/

<input type="email" /> // <input type="email"> is a syntax error

/* 
  3) JSX elements that consist of an opening and closing tag (i.e. div, span, button element), must have both or be closed with a trailing forward slash. Like 2), it is a syntax error to have an unterminated element. 
*/

<button>Click me</button> // <button> or </button> is a syntax error
<button /> // empty, but also valid
```

**Styling in JSX**

```js
/* 
  Properties that accept pixel values (like width, height, padding, margin, etc), can use integers instead of strings.
  For example: fontSize: 22. Instead of: fontSize: "22px"
*/
<h1 style={{ color: 'blue', fontSize: 22, padding: '0.5em 1em' }}>
  Hello React!
</h1>
```

JSX elements are JavaScript expressions and can be used as such. JSX gives us the full power of JavaScript directly within our user interface.

```js
/* 
  JSX elements are expressions (resolve to a value) and therefore can be assigned to plain JavaScript variables... 
*/
const greeting = <div>Hello React!</div>;

const isNewToReact = true;

// ... or can be displayed conditionally
function sayGreeting() {
  if (isNewToReact) {
    // ... or returned from functions, etc.
    return greeting; // displays: Hello React!
  } else {
    return <div>Hi again, React</div>;
  }
}
```

JSX allows us to insert (or embed) simple JavaScript expressions using the curly braces syntax:

```js
const year = 2021;

/* We can insert primitive JS values (i.e. strings, numbers, booleans) in curly braces: {} */
const greeting = <div>Hello React in {year}</div>;

/* We can also insert expressions that resolve to a primitive value: */
const goodbye = <div>Goodbye previous year: {year - 1}</div>

/* Expressions can also be used for element attributes */
const className = 'title';
const title = <h1 className={className}>My title</h1>

/* Note: trying to insert object values (i.e. objects, arrays, maps) in curly braces will result in an error */
```

JSX allows us to nest elements within one another, like we would HTML.

```js
/* 
  To write JSX on multiple lines, wrap in parentheses: ()
  JSX expressions that span multiple lines are called multiline expressions
*/

const greeting = (
  // div is the parent element
  <div>
    {/* h1 and p are child elements */}
    <h1>Hello!</h1>
    <p>Welcome to React</p>
  </div>
);
/* 'parents' and 'children' are how we describe JSX elements in relation
to one another, like we would talk about HTML elements */
```

Comments in JSX are written as multiline JavaScript comments, written between curly braces, like this:

```js
const greeting = (
  <div>
    {/* This is a single line comment */}
  	<h1>Hello!</div>
	<p>Welcome to React</p>
    {/* This is a 
      multiline
      comment */} 
  </div>
);
```

# Components

Components let you split the UI into independent, reusable pieces, and think about each piece in isolation. This page provides an introduction to the idea of components. 

JSX can be grouped together within individual functions called **components**.

There are two types of components in React: **function components** and **class components**.

Component names, for function or class components, are capitalized to distinguish them from plain JavaScript functions that do not return JSX:

**Function Component with props**

```js
import React from 'react'
/* 	
  Function component
  Note the capitalized function name: 'Header', not 'header'
*/
function Welcome() {
  return <h1>Hello, Nitesh</h1>;
}

// Function components which use an arrow function syntax are also valid
const Welcome = () => <h1>Hello, Nitesh</h1>;
```

**Class Component  Basic**

```js
import React, {Component } from "react"
class Welcome extends Component {
  render() {
    return <h1>Hello React</h1>;
  }
}

// When we use only export keyword it means that you're not allowd to change while importing 
// Default keyword means you can change it's name while importing 
export default Welcome;
```

# Props

Data passed to components in JavaScript are called **props**. Props look identical to attributes on plain JSX/HTML elements, but you can access their values within the component itself.

Props are available in parameters of the component to which they are passed. Props are always included as properties of an object.

Props must never be directly changed within the child component. 

Another way to say this is that props should never be **mutated**, since props are a plain JavaScript object.

**Props in Funcation Component**

```js
import React, { Component } from "react";

const Greet = (props) => {
  <h1>Hello, {props.name}</h1>;
};

export default class App extends Component {
  render() {
    return (
      <div>
        <Greet name="Bruce" />
        <Greet name="Diana" />
        <Greet name="Clark" />
      </div>
    );
  }
}

```

**Child Props**

```js
import React, { Component } from "react";
import { Button } from "react-native";

const Greet = (props) => {
  return (
    <div>
      <h1>Hello, {props.name}</h1>
      {props.childern}
    </div>
  );
};

export default class App extends Component {
  render() {
    return (
      <div>
        <Greet name="Bruce">
          <Button>Action</Button>
        </Greet>
        <Greet name="Diana" />
        <Greet name="Clark" />
      </div>
    );
  }
}

// First component will render button tag with text
```

**Props in Class Component**

```js
import React, { Component } from "react";

export default class Welcome extends Component {
  render() {
    return (
      <div>
        <h1>Hello, {this.props.name}</h1>
      </div>
    );
  }
}
```

# State

```js
import React, { Component } from "react";

export default class Welcome extends Component {
  constructor(props) {
    super(props);
    this.state = {
      message: "Welcome Visitor",
    };
    this.changeMessage = this.changeMessage.bind(this);
  }

  changeMessage = () => {
    this.setState({
      message: "Thank you!",
    });
  };

  render() {
    return (
      <div>
        <h1>{this.state.message}</h1>
        <button onClick={() => this.changeMessage()}>Action</button>
      </div>
    );
  }
}

```

### setState

`setState(updater, [callback])`

`setState()` enqueues changes to the component state and tells React that this component and its children need to be re-rendered with the updated state. This is the primary method you use to update the user interface in response to event handlers and server responses.

```js
import React, { Component } from 'react'

class Counter extends Component {
    constructor(){
        this.state = {
            count = 0
        }
        this.onIncrement = this.onIncrement.bind(this)
    }

    onIncrement = () => {
        this.setState({
            count = count + 1
        })
    }
    render() {
        return (
            <div>
                <h1>{this.state.count}</h1>
                <button onClick={() => {this.onIncrement()}}></button>
            </div>
        )
    }
}

export default Counter
```

### How to Use the setState Callback

To perform an action in a React component after calling setState, such as making an AJAX request or throwing an error, we use the setState callback.

> 🔥 Tip : Here’s something extremely important to know about state in React: **updating a React component’s state is asynchronous**. It does *not*happen immediately.

Therefore you *will* run into scenarios whereby parts of your code run before state has had a chance to update.

To solve this specific React issue, we can use the setState function’s callback. Whatever we pass into setState’s second argument executes after the setState function updates.

**setState Callback in a Class Component**

```js
import React, { Component } from 'react';

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      age: 0,
    };
  }
  
  // this.checkAge is passed as the callback to setState
  updateAge = (value) => {
    this.setState({ age: value}, this.checkAge);
  };

  checkAge = () => {
    const { age } = this.state;
    if (age !== 0 && age >= 21) {
      // Make API call to /beer
    } else {
      // Throw error 404, beer not found
    }
  };

  render() {
    const { age } = this.state;
    return (
      <div>
        <p>Drinking Age Checker</p>
        <input
          type="number"
          value={age}
          onChange={e => this.updateAge(e.target.value)}
        />
      </div>
    );
  }

}

export default App;
```

**Note:** 

1. Always make use of setState and never modify the state directly
2. Code has to be executed after the state has been updated? place that code in the call back function which is the second argument to the setState method.
3. When you have to update state based on the previous state value, pass in a function as an argument instated of the regular object.

https://upmostly.com/tutorials/how-to-use-the-setstate-callback-in-react



# Destructuring

## Props

```js
import React, { Component } from "react";

const Greet = (name, heroName) => {
  <h1>Hello, {name} aka {heroName}</h1>;
};

// Or
const Greet = props => {
  const {name, heroName} = props
  <h1>Hello, {name} aka {heroName}</h1>;
};

export default class App extends Component {
  render() {
    return (
      <div>
        <Greet name="Tony Stark" heroName="Iron Man" />
        <Greet name="Diana"  heroName="Wonder Women"/>
        <Greet name="Dr. Bruce" heroName="Hulk" />
      </div>
    );
  }
}
```

## State

```js
export default class Welcome extends Component {
  constructor(){
		this.state = {
      name:"Tony Stark",
      heroName:"Iron Man"
    }
  }
  render() {
  const {name, heroName} = this.state
    return (
      <h1>Hello, {name} aka {heroName}</h1>;
    );
  }
}
```

# Event Handling

```js
import React from "react";

function FunctionClick() {
  function clickHandler() {
    console.log("Button Clicked");
  }
  return (
    <div>
      <button onClick={clickHandler}></button>
    </div>
  );
}

export default FunctionClick;

```

**Note: ** When we use `clickHandler()` instated of `clickHandler` it call automatically it will get worse `clickHandler` is function suppose to update state.

# BindEvents

 Don't know why

```js
import React, { Component } from "react";

class EventBind extends Component {
  constructor(props) {
    super(props);

    this.state = {
      message: "Welcome",
    };
    this.clickHandler = this.clickHandler.bind(this);
  }

  clickHandler() {
    this.setState({
      message: "GoodBye!",
    });
    console.log(this);
  }

  render() {
    return (
      <div>
        <h1>{this.state.message}</h1>
        <button onClick={this.clickHandler.bind(this)}>First method</button>
        <button onClick={() => {this.clickHandler}}>Second method bind in constructor</button>
      </div>
    );
  }
}

export default EventBind;

```

# Methods as props

```js
// App.js
import React, { Component } from "react";
import Parent from "./component/Parent";

export default class App extends Component {
  render() {
    return (
      <div>
        <Parent />
      </div>
    );
  }
}

```

```js
// Parent.js
import React, { Component } from "react";
import Child from "./Child";

class Parent extends Component {
  constructor(props) {
    super(props);

    this.state = {
      parentName: "Parent",
    };
    this.greetParent = this.greetParent.bind(this);
  }
  greetParent(child) {
    alert(`Hello, ${this.state.parentName} from ${child}`);
  }
  render() {
    return (
      <div>
        <Child greetHander={this.greetParent} />
      </div>
    );
  }
}

export default Parent;

```

```js
// Child.js
import React from "react";

function Child(props) {
  return (
    <div>
      <button onClick={() => {props.greetHander("Child");}}>
        Greet Parent
      </button>
    </div>
  );
}

export default Child;

```

# Conditional Rendering

```js
// if/else method
render() {
    if (this.state.isLoggedIn) {
      return <div>Hello, Nitesh</div>;
    } else {
      return <div>Hello, Guest</div>;
    }
  }
```

```js
// element variables method
render() {
		let message
    if (this.state.isLoggedIn) {
      message = <div>Hello, Nitesh</div>;
    } else {
      message = <div>Hello, Guest</div>;
    }
    return <div> {message} </div>
  }
```

```js
// Ternary condition method
render() {
    return this.state.isLoggedIn ? (
      <div>Hello, Nitesh</div>
    ) : (
      <div>Hello, Guest</div>
    );
```

```js
// Shortcircuit operator
render() {
	return this.state.isLoggedIn && <div>Hello, Nitesh</div>
}
```

# List Rendering

Use the **.map()** function to convert lists of data (arrays) into lists of elements.

```js
const people = ["John", "Bob", "Fred"];
const peopleList = people.map(person => <p>{person}</p>);
```

`.map()` can be used for components as well as plain JSX elements.

```js
function App() {
  const people = ['John', 'Bob', 'Fred'];
  // can interpolate returned list of elements in {}
  return (
    <ul>
      {/* we're passing each array element as props to Person */}
      {people.map(person => <Person name={person} />}
    </ul>
  );
}

function Person({ name }) {
  // we access the 'name' prop directly using object destructuring
  return <p>This person's name is: {name}</p>;
}
```

Each React element within a list of elements needs a special **key prop**. Keys are essential for React to be able to keep track of each element that is being iterated over with the `.map()` function.

React uses keys to performantly update individual elements when their data changes (instead of re-rendering the entire list).

Keys need to have unique values to be able to identify each of them according to their key value.

**Keys**

- A `key` is a special string attribure uou need to include when creating lists of elements.
- Keys give the element a stable identity
- Keys help React identify which items have changed, are added, or are removed
- Help in efficient update of the user interface

**Example**

```js
import React, { Component } from "react";
import Person from "./Person";

class ListRender extends Component {
  render() {
    let persons = [
      {
        id: 1,
        name: "Bruce",
        age: 26,
        skills: "ReactJS",
      },
      {
        id: 2,
        name: "Clark",
        age: 30,
        skills: "Python",
      },
      {
        id: 3,
        name: "Diana",
        age: 26,
        skills: "React Native",
      },
    ];
    const personList = persons.map((person) => <Person person={person} />);
    return <div>{personList}</div>;
  }
}

export default ListRender;
```

```js
import React from "react";

function Person({ person }) {
  return (
    <div>
      <h2>
        I am {person.name}, I'm {person.age} years old. I know {person.skills}
      </h2>
    </div>
  );
}

export default Person;
```

# Lifecycle Methods 

## `constructor()`

- A special function that will get called whenever a new component is created.
- Good for initialing state and binding event handlers.
- Do not cause side effects ex. HTTP request.

```js
constructor(props) {
  super(props);
  this.state = {
    list: props.initialList
  };
}

// where props aren't used in constructor
constructor() {
  super();
  this.state = {
    list: []
  };
}
```

##  `render()`

- Only required method for component 
- Read props and state and retrun jsx
- Do not change state or intract with DOM or make Ajax calls 
- Children components lifecycle methods also excuated 

```js
render() {
  return <div />;
}
```

## `componentWillMount()`

```js
componentWillMount() {
  // invoked once.
  // fires before initial 'render'
}
```

## `componentDidMount()`

- Invoked immediately after a component and all its childern components have been rendered to the DOM
- Best way for makeing ajax and HTTP request

```js
componentDidMount() {
  // good for AJAX: fetch, ajax, or subscriptions.
  // invoked once (client-side only).
  // fires before initial 'render'
}
```

## `componentWillReceiveProps()`

```js
componentWillReceiveProps(nextProps) {
  // invoked every time component recieves new props.
  // does not before initial 'render'
}
```

## `shouldComponentUpdate()`

```js
shouldComponentUpdate(nextProps, nextState) {
  // invoked before every update (new props or state).
  // does not fire before initial 'render'.
}
```

## `componentWillUpdate()`

```js
componentWillUpdate(nextProps, nextState) {
  // invoked immediately before update (new props or state).
  // does not fire before initial 'render'.
  // (see componentWillReceiveProps if you need to call setState)
}
```

## `componentDidUpdate()`

```
componentDidUpdate(prevProps, prevState) {
  // invoked immediately after DOM updates
  // does not fire after initial 'render'
}
```

## `componentWillUnmount()`

```
componentWillUnmount() {
  // invoked immediately before a component is unmounted.
}
```

**Example**

```js
// LifecycleA.js
import React, { Component } from "react";
import LifecycleB from "./LifecycleB";
export default class LifecycleA extends Component {
  constructor(props) {
    super(props);
    console.log("Constructor A");
  }
  componentDidMount() {
    console.log("Componentdidmount A");
  }
  render() {
    console.log("Render A");
    return (
      <div>
        <h1>Life A</h1>
        <LifecycleB />
      </div>
    );
  }
}


// LifecycleB.js
import React, { Component } from "react";
export default class LifecycleB extends Component {
  constructor(props) {
    super(props);
    console.log("Constructor B");
  }

  componentDidMount() {
    console.log("Componentdidmount B");
  }

  render() {
    console.log("Render B");
    return (
      <div>
        <h1>Lifecycle B</h1>
      </div>
    );
  }
}

```

```txt
// Output
[Log] Constructor A
[Log] Render A
[Log] Constructor B
[Log] Render B
[Log] Componentdidmount B
[Log] Componentdidmount A
```

## `componentDidCatch()`

```js
componentDidCatch(error, info){
	
}
```

# Fragments

A common pattern in React is for a component to return multiple elements. Fragments let you group a list of children without adding extra nodes to the DOM.

## Motivation 

A common pattern is for a component to return a list of children. Take this example React snippet:

```js
class Table extends React.Component {
  render() {
    return (
      <table>
        <tr>
          <Columns />
        </tr>
      </table>
    );
  }
}
```

would need to return multiple  elements in order for the rendered HTML to be valid. If a parent div was used inside the `render()` of, then the resulting HTML will be invalid.

```js
class Columns extends React.Component {
  render() {
    return (
      <div>
        <td>Hello</td>
        <td>World</td>
      </div>
    );
  }
}
```

results in a output of:

```js
<table>
  <tr>
    <div>
      <td>Hello</td>
      <td>World</td>
    </div>
  </tr>
</table>
```

Fragments solve this problem.

##  Usage 

```js
class Columns extends React.Component {
  render() {
    return (
      <React.Fragment>
        <td>Hello</td>
        <td>World</td>
      </React.Fragment>
    );
  }
}
```

which results in a correct `` output of:

```js
<table>
  <tr>
    <td>Hello</td>
    <td>World</td>
  </tr>
</table>
```

### Short Syntax 

There is a new, shorter syntax you can use for declaring fragments. It looks like empty tags:

```js
class Columns extends React.Component {
  render() {
    return (
      <>
        <td>Hello</td>
        <td>World</td>
      </>
    );
  }
}
```

You can use `<>` the same way you’d use any other element except that it doesn’t support keys or attributes.

### Keyed Fragments 

Fragments declared with the explicit `` syntax may have keys. A use case for this is mapping a collection to an array of fragments — for example, to create a description list:

```js
function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        // Without the `key`, React will fire a key warning
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
}
```

# Pure Component