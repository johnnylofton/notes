# React - The Complete Guide

Notes based on Udemy Course [React - The Complete Guide (incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux)

## 1. Getting Started

- Building elements with reusable components.
- Utilizes JSX

### Writing Our First React Code

```javascript
function Person(props) {
  return (
    <div className="person">
      <h1>{props.name}</h1>
      <p>Your Age: {props.age}</p>
    </div>
  );
}

ReactDOM.render(<Person name="John" age="26" />, document.querySelector('#p1'));

ReactDOM.render(<Person name="Sam" age="28" />, document.querySelector('#p2'));
```

- Turn into:

```javascript
function Person(props) {
  return (
    <div className="person">
      <h1>{props.name}</h1>
      <p>Your Age: {props.age}</p>
    </div>
  );
}

var app = (
  <div>
    <Person name="John" age="26" />
    <Person name="Sam" age="28" />
  </div>
);

ReactDOM.render(app, document.querySelector('#app'));
```

### Why Should we Choose React

1. UI State becomes difficult to handle with Vanilla Javascript
2. Focus on Business Logic, not on preventing your App from exploding
   1. Plus: Framework Creators probably write better Code
3. Huge Ecosystem, Active Community, High Performance

### React Alternatives

- Angular
- Vue
- Not so much: jQuery

### Understanding Single Page Applications and Multi Page Applications

#### Two Kinds of Applications

|               Single Page Applications                |              Multi Page Applications               |
| :---------------------------------------------------: | :------------------------------------------------: |
| Only ONE HTML Page, Content is (re)rendered on Client | Multiple HTML Pages, Content is rendered on Server |
|   Root react component, and other child components    | Typical HTML/CSS/JS, with maybe some React widgets |
|       Typically only ONE ReactDOM.render() call       |      One ReactDOM.render() call per "widget"       |

### Course Outline

1. Getting Started
2. The Basics
3. Debugging
4. Styling Components
5. Components Deep Dive
6. HTTP Requests
7. Routing
8. Forms & Validation
9. Redux
10. Authentication
11. Testing Introduction
12. Deployment
13. Bonus (Animations, Next Steps, Webpack)

## 2. Refreshing Next Generation Javascript (Optional)

<!-- SKIPPED -->

## 3. Understanding the Base Features and Syntax

### The Build Workflow

- Local project
- Using a Build Workflow
    - Recommend for SPAs and MPAs
    - Why?
        - **Optimize** Code
        - Use **Next-Gen** Javascript Features
            - ES6 vs ES5
        - Be **More Productive**
    - How?
        - Use **Dependency Management** Tool
            - `npm` or `yarn`
        - Use **Bundler**
            - Recommended: **Webpack**
        - Use **Compiler** (Next-Gen Javascript)
            - **Babel** + Presets
        - Use a **Development Server**

### Using Create React App

`npm i -g create-react-app`

### Understanding the Folder Structure

- `package.json` lists dependencies
- `node_modules` holds files for dependencies and subdependencies
- `public` folder is root folder served by web server in the end.
    - Contains the single `index.html` file in a project.
        - `<div id="root"></div>` is where React app will be mounted
    - `manifest.json` file defines meta-data
- `src` contains files we will work with

### Understanding Component Basics

- App component our first component
- Typically render one root component, and nest all others
- React component *class* extends Component from React, and has method `render()`
- `.js` and `.jsx` files
- Utilizing JSX

### Understanding JSX

- With `React.createElement()`
    - Our previous JSX ends up compiled to this
    - Takes at least 3 args
        - 1st is element we want to render to DOM
        - 2nd is configuration
        - 3rd is any amount of children
    - `return React.createElement('div', null, 'h1', "Hi, I'm a React App!");`
        - This gets us: `h1Hi, I'm a React App!`
        - Interpreted as text by default, need to once again call `createElement()`

This code

```javascript
return React.createElement(
  'div',
  {className: "App"},
  React.createElement('h1', null, 'Does this work now?')
);
```

is equivilant to this code:

```javascript
return (
  <div className="App">
    <h1>Hi, I'm a React App</h1>
  </div>
);
```

### JSX Restrictions

- `className` is used instead of `class` because the second is a reserved word in javascript
    - Translated to `class`
- We are not using the real HTML tags, React is converting them behind the scenes
- Must have a single root element, can't have multiple

### Creating a Functional Component

- Creating a new component
- `Person/Person.js`:

```javascript
import React from 'react';

const person = () => {
  return <p>I'm a Person!</p>;
};

export default person;
```

In App.js: `<Person />`

### Working with Components and Re-Using Them

- Simply decalre the element multiple times, can do this anywhere.

### Outputting Dynamic Content

```javascript
// Person.js
import React from 'react';

const person = () => {
  return (
    <p>I'm a Person and I am {Math.floor(Math.random() * 30)} years old!</p>
  );
};

export default person;
```

### Working with Props

In App.js:

```javascript
<Person name="John" age="26" />
<Person name="Max" age="28" >My Hobbies: Racing</Person>
<Person name="Sam" age="23" />
```

in Person.js:
`I'm a {props.name} and I am {props.age} years old!`

### Understanding the `children` prop

- `children` is reserved.
- Includes any elements in between opening and closing tag of our component/element
- In Person.js:
    - `<p>{props.children}</p>`

### Understanding and Using State

- Using `state` property

```javascript
class App extends Component {
  state = {
    persons: [
      { name: 'John', age: 26 },
      { name: 'Max', age: 28 },
      { name: 'Same', age: 23 },
    ],
  };

  render() {
    return (
      <div className='App'>
        <h1>Hi, I'm a React App</h1>
        <p>This is really working!</p>
        <button>Switch Name</button>
        <Person
          name={this.state.persons[0].name}
          age={this.state.persons[0].age}
        />
        <Person
          name={this.state.persons[1].name}
          age={this.state.persons[1].age}
        >
          My Hobbies: Racing
        </Person>
        <Person
          name={this.state.persons[2].name}
          age={this.state.persons[2].age}
        />
      </div>
    );
  }
}
```

### Handling Events with Methods

`switchNameHandler = () => { console.log('Was clicked!'); };`

`<button onClick={this.switchNameHandler}>Switch Name</button>`

- [To which Events can you Listen?](https://reactjs.org/docs/events.html#supported-events)

### Manipulating the State

- Only two things update DOM
    - `props` & `state`

```javascript
switchNameHandler = () => {
  // console.log('Was clicked!');
  // DON'T DO THIS: this.state.persons[0].name = 'Jonathonas'
  this.setState({persons: [
    { name: 'Jonathonas', age: 26 },
    { name: 'Maximilian', age: 28 },
    { name: 'Sam', age: 23 },
  ]})
};
```

### Using the `useState()` Hook for State Manipulation

- **Prior to React v16.8, managing state in classes was the only way**
- Used in *functional* React components
- `useState()` *Always* returns array with exactly 2 elements, always
    - 1st will always been our current state
    - 2nd will be a function that allows us to update state

- Replaces old state with what you give it.
- Choice between using class-based vs functional components with hooks

### Stateless vs Stateful Components

- Whether using `this.state` or hooks, they are stateful
- Others are display/functional components
- It's good practice to have containers and few smart or stateful components for handling the necessary logic

### Passing Method References Between Components

- Can pass down a reference to a handler function down as a property
    - `<Person click={switchNameHandler} >`
    - `<p onClick={props.click}>`

- When passing data:
    - `switchNameHandler = (newName) => { }`
    - Using bind:
        - `this.switchNameHandler.bind(this, 'Max!')`
    - Using arrow function:
        - `() => this.switchNameHandler('Maximilian!!')`
    - Better to use bind, arrow function in cases can be more inefficient

### Adding Two Way Binding

- Changing the name on our own with an input

```javascript
nameChangedHandler = (event) => {
  this.setState( {
    persons: [
      { name: 'Max', age: 28 },
      { name: event.target.value, age: 29 },
      { name: 'Stephanie', age: 26 }
    ]
  } )
}
// ...
```
```html
<!-- in render(): -->
  <Person changed={this.nameChangedHandler} />
<!-- in Person component: -->
<input type='text' onChange={props.changed} />
```

- This way, we're able to dynamically update, and also use inputs
- Now, we want to see the state right from the start
    - `value={props.name}`
    - This gives warning, because potentially not handling changes. We need onChange or we cannot change the input
    - Will improve this later

### Adding Styling with Stylesheets

- Give `div` in `Person` a class name: `<div className="Person">`
- Create `Person.css`

```css
.Person {
  width: 60%;
  margin: 16px auto;
  border: 1px solid #eee;
  box-shadow: 0 2px 3px #ccc;
  padding: 16px;
  text-align: center;
}
```

- Now, must import. Thanks to webpack (which is a part of the build tools, react scripts), we can import CSS files into Javascript in React.
    - `import './Person.css';`
- Can see the style tags are injected dynamically by webpack
- It is also ***global***

### Working with Inline Styles

- Styling our button differently

```javascript
  render() {
    const style = {
      backgroundColor: 'white',
      font: 'inherit',
      border: '1px solid blue',
      padding: '8px',
      cursor: 'pointer',
    };
    return (
      // ...
      <button style={style} onClick={/* ... */}>
      // ...
    )
  }
```

- Inline styling has restrictions, for example if we were to try a hover effect on the button.
    - We will look at this more in depth later on for a way to scope styles and still use all the CSS features (Section 29?)

### Resources

- [create-react-app](https://github.com/facebookincubator/create-react-app)
- [Introducing JSX](https://reactjs.org/docs/introducing-jsx.html)
- [Rendering Elements](https://reactjs.org/docs/rendering-elements.html)
- [Components & Props](https://reactjs.org/docs/components-and-props.html)
- [Listenable Events](https://reactjs.org/docs/events.html)

## 4. Working with Lists and Conditionals

### Rendering Content Conditionally



### Handling Dynamic Content "The Javascript Way"

### Outputting Lists

### Lists and State

### Updating State Immutably

### Lists and Keys

### Flexible Lists

## 5. Styling React Components and Elements

## 6. Debugging React Apps
