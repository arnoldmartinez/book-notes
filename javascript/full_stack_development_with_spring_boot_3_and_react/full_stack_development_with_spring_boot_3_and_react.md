## Creating React components

React is component-based, and the components are independent and reusable.

![React Components](img/8.1.png)

The components can then be arranged in a **tree hierarchy**.

![Component tree](img/8.2.png)

The **root component** has two **child component**: the search component and the table component.
The table component has one child component: the table row component. The important thing to
understand with React is that the data flow goes from a parent component to a child component.

React uses the **virtual document object model (VDOM)** for selective re-rendering of the UI. The 
**document object model (DOM)** is a programming interface for web documents that represent the web
page as a structured tree of objects. Each object in a tree corresponds to a part of the document.
The VDOM is lightweight copy of the DOM, and manipulation of the VDOM is much faster than it is whith the
real DOM. After the VDOM is updated, React compares it to a snapshot that was taken of the VDOM before
updates were run. After the comparison, React will know which parts have been changed, and only these 
parts will be updated to the real DOM.

A React component can be defined either by using a JavaScript function - a **functional component**
or the ES6 JavaScript class - a **class component**.

JavaScript function
```
// Using JavaScript function
function App() {
    return <h1>Hello World</h1>;
}
```

ES6 to create a component
```
// Using ES6 class
class App extends React.Component {
    render() {
        return <h1>Hello World</h1>;
    }
}
```

The class component contains the required render() method, which shows and updates the rendered output of the component.
If you compare the functional and class App components, you can see that the render() method is not needed in the 
functional component. Before React version 16.8, you had to use class components to be able to use states. Now, you can
use hooks to create states with functional components as well.

The name of the React component should start with a capital letter. It is also recommended to use the PascalCase naming
convention.

If your component returns multiple elements, you have to wrap these inside one parent element. Such as a div.

```
// Wrap elements inside the div
function App() {
    return (
        <div>
            <h1>Hello World</h1>
            <h2>This is my first React component</h2>
        </div>
    );
}
```

You can also use a React **fragment**. Fragments don't add any extra nodes to the DOM tree:

```
// Using fragments
function App() {
    return (
        <React.Fragment>
            <h1>Hello World</h1>
            <h2>This is my first React component</h2>
        </React.Fragment>
    );
}
```

```
// Using fragments short syntax
function App() {
    return (
        <>
            <h1>Hello World</h1>
            <h2>This is my first React component</h2>
        </>
    );
}
```

The react-dom package provides DOM-specific methods for us.

main.jsx file

```
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')).render(
    <React.StrictMode>
        <App />
    </React.StrictMode>
)
```

The react-dom package provides DOM-specific methods for us. To render the React component to the DOM, we can use the 
render method from the react-dom package. React.StrictMode is used to find potential problems in your React app and these
are printed in the browser console. Strict Mode only runs in development mode.

The **root API** is used to render React components inside a browser DOM node. we first create a root by passing the DOM
element to the createRoot method. The root calls the render method to render an element to the root.

```
import ReactDOM from 'react-dom/client';
import App from './App';

const container = document.getElementById('root');

// Create a root
const root = ReactDOM.createRoot(container);

// Render an element to the root
root.render(<App />);
```

The container in the root API is the <div id="root"></div> can be found in the index.html

```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <link rel="icon" type="image/svg+iml" href="/vite.svg" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Vite + React</title>
    </head>
    <body>
        <div id="root"></div>
        <script type="module" src="/src/main.jsx"></script>
    </body>
</html>
```

At the end of the source code, there is an export default statement that exports the component, and it can be made 
available to other components by using the import statement

```
import {useState} from 'react'
import reactLogo from './assets/react.svg'
import viteLogo from '/vite.svg'
import './App.css'

function App() {
    const [count, setCount] = useState(0)
    
    return (
        <div className="App">
            <div>
                <a href="https://vitejs.dev" target="_blank">
                    <img src="{viteLogo}" className="logo" alt="Vite logo" />
                </a>
                <a href="https://reactjs.org" target="_blank">
                    <img src="{reactLogo}" className="logo react" alt="React logo" />
                </a>
            </div>
            <h1>Hello React</h1>
            <div className="card">
                <button onClick={() => setCount((count) => count + 1)}>
                    count is {count}
                </button>
                <p>
                    Edit <code>src/App.jsx</code> and save to test HMR
                </p>
            </div>
            <p className="read-the-docs">
                Click on the Vite and React logos to learm more
            </p>
        </div>
    )
}

export default App
```

There can only be one export default statement per file, but there can be multiple named export statements. Default exports
are commonly used to export React components. Named exports are commonly used to export specific functions or objects from
module.

```
import React from 'react' // Import default value
import { name } from ... // Import named value
```

```
export default React // Default export
export { name } // Named export
```

### Const and variables

Constants, or immutable variables, can be defined by using a **const** keyword. When using the const keyword, the variable
content cannot be reassigned:

```
contst PI = 3.14159;
```

The const is block-scoped. This means that the const variable can only be used inside the block in which it is defined.
If const is defined outside of any function or block, it becomes a global variable, and you should try to avoid this
situation. 

```
let count = 10;

if (count > 5) {
    const total = count * 2;
    console.log(total); // Prints 20 to console
}

console.log(total); // Error, outside the scope
```

The following example demonstrates what happens when const is an object or array:

```
const myObj = {foo: 3};
myObj.foo = 5; // This is ok
```

When const is an object or array, its properties or elements can be updated.
The **let** keyword allow you to decalre **mutable** block-scoped variables. The variables declared using **let** can be
used inside the block in which it is declared(it can also be used inside sub-blocks).

### Arrow functions

The traditional way of defining a function in JavaScript is by using a **function** keyword.

```
function(x) {
    return x * 2;
}
```

When we use the ES6 arrow function, the function look like this:

```
x => x * 2
```

The function is a so-called **anonymous function**, and we can't call it. Anonymous function are often used as an argument
for other functions. In JavaScript, functions are *first-class citizens* and you can store functions in variables.

```
const calc = x => x * 2;
```

Now, you can use the variable name to call the function

```
calc(5);
```

When you have more than one argument, you have to wrap the arguments in parenthesis and separate the arguments with a 
comma to use the arrow function effectively.

```
const calcSum = (x, y) => x + y
// function call
calcSum(2, 3); // return 5;
```

If the function body is an expression, then you don't need to use the **return** keyword. When you have multiple lines
in the function body, you have to use curly brackets and a **return** statement.

```
const calcSum = (x , y) => {
    console.log('Calculating sum');
    return x + y;
}
```

If the function doesn't have any arguments, then you should use the empty parenthesis.

```
const sayHello = () => "Hello"
```

### Template literals

Template literals can be used to concatenate strings. You have to use backticks(``) instead of single or double quotes:

```
let person = {firstName: 'John', lastName: 'Johnson'};
let greeting = `Hello ${person.firstName} ${person.lastName}`;
```

### Object destructuring

Object destructuring feature allows you to extract values from an object and assign them to a variable.

```
conts person = {
    firstName: 'John',
    lastName: 'Johnson',
    email: 'j.johnson@mail.com'
};
```

You can destructure it using the following statement:

```
const { firstName, lastName, email } = person;
```

It creates three variables , firstName, lastName, and email, which get their values from the person object.

### Classes and inheritance

The keyword for defining classes is **class**. A class can have fields, constructors, and class methods.

```
class Person {
    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
}
```

Inheritance is performed with an **extends** keyword. The following sample code shows an **Employee** class that inherits
a **Person** class. This means that it inherits all fields from the parent class andd can have its own fields that are 
specific to **Employee**. In the constructor, we first call the parent class constructor by using the **super** keyword.
That call is required by the rest of the code, and you will get an error if it is missing:

```
class Employee extends Person {
    constructor(firstName, lastName, title, salary) {
        super(firstName, lastName);
        this.title = title;
        this.salary = salary;
    }
}
```

**Babel** is a JavaScript compiler that is used to compile ES6 (or newer versions) to an older version that is compatible
with all browsers.

### JSX and styling

JavaScript XML (JSX) is the syntax extension for JavaScript. JSX prevents injection attacks because all values are escaped
in JSX before they are rendered. You can embed JavaScript expressions in JSX by wrapping them with curly brackets.

```
function App(props) {
    return <h1>Hello World {props.user}</h1>;
}
```

You can also pass a JavaScript expressions as props.

```
<Hello count={2+2} />
```

You can use both inline and external styling with React JSX elements. This first one defines the style inside the **div**
element:

```
<div style={{ height: 20, width: 200 }}>
    Hello
</div>
```

The second creates a style object first, which is then used in the **div** element. The object name should use the 
**camelCase** naming convention:

```
const divStyle = { color: 'red', height: 30 };

const MyComponent = () => (
    <div style={divStyle}>Hello</div>
);
```

You can import a style sheet into a React component. To reference classes from an external CSS file, you should use a
**className** attribute.

```
import './App.js';
...
<div className="App-header"> This is my app</div>
```

## Props and state
**Props** and **state** are the input data for rendering a component.
### Props
**Props** are inputs to components, and they are a mechanism to pass data from a parent component to its child component.
Props are JavaScript objects, so they can contain multiple key-value pairs.

Props are inmutable, so a component cannot change its props. A component can access props through the props object that is
passed to the function component as a parameter.

```
function Hello() {
    return <h1>Hello John</h1>;
}
```

Instead of using a hardcoded name, we can pass a name to the Hello component by using props.

```
function Hello(props) {
    return <h1>Hello {proprs.user}</h1>;
}
```

The parent component can send props to the Hello component

```
<Hello user="John" />
```

You can also pass multiple props to a component

```
<Hello firstName="John" lastName="Johnson" />
```

Now, you can access both props in the component using the props object.

```
function Hello(props) {
    return <h1>Hello {props.firstName} {props.lastName}</h1>;
}
```

You can also use object destructuring to destructure a props object

```
function Hello({ firstName, lastName }) {
    return <h1>Hello {firstName} {lastName}</h1>;
}
```

### State

The component **state** is an internal data store that holds information that can change over time. The state also 
affects the rendering of the component. When the state is updated, React schedules a re-render of the component. When
the component re-renders, the state retains its latest values. State allows components to be dynamic and responsive to
user interactions or other events.

The state is created using the useState hook function. It takes one argument, which is the initial value of the state,
and returns an array of two elements. The first element is the name of the state, and the second element is a function
that is used to update the state value. 

```
const [state, setState] = React.useState(initialValue);
```

The next code creates a state variable called name, and the initial value is Jim:

```
const [name, setName] = React.useState('Jim');
```

You can also import the useState function from React

```
import React, { useState } from 'react';
```

Then, you don't need to type the React keyword

```
const [name, setName] = useState('Jim');
```

The value of the state can now be updated by using the setName function. This is the only way to modify the state value

```
setName('John');
```

You should never update the state value directly using the = operator. You will also get an error because you cannot
reassign the const variable.

```
// Don't do this, UI won't re-render
name = 'Jonh';
```

You can also define state using an object

```
const [name, setName] = useState({
    firstName: 'John',
    lastName: 'Johnson'
});
```

You can update both the firstName and lastName state object parameters using the setName function

```
setName({ firstName: 'Jim', lastName: 'Palmer' })
```

If you want to do a partial update of the object, you can use the **spread operator**. It clones the name state object
and updates the firstName value to be Jim

```
setName({ ...name, firstName: 'Jim' })
```

A state can be accessed by using the state name. The scope the state is the component, so it cannot be used outside 
the component.

```
// Renders Hello John
import React, { useState } from 'react';

function MyComponent() {
    const [firstName, setFirstName] = useState('John');
    
    return <div>Hello {firstName}</div>
}
```

If you state is an object, then you can access it in the following way:

```
const [name, setName] = useState({
    firstName: 'John',
    lastName: 'Johnson'
});

return <div>Hello {name.firstName}</div>;
```

### Stateless components

The React **stateless component** is just a pure JavaScript function that takes props as an argument and return a React
element 

```
function HeaderText(props) {
    return (
        <h1>
            {props.text}
        </h1>
    )
}

export default HeaderText;
```

This is also called a **pure component**. React provides React.demo(), which optimizes the performance of pure functional
components.

```
import React, { memo } from 'react';

function HeaderText(props) {
    return (
        <h1>
            {props.text}
        </h1>
    )
}

export default memo(HeaderText);
```

React renders a memorized result if the props are not changed. The React.memo() phrase also has a second argument, 
**arePropsEquals()**, which you can use to customize rendering conditions. 

