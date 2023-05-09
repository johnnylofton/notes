# Vue - The Complete Guide

Notes based on Udemy Course [Vue - The Complete Guide (incl. Router & Composition API)](https://www.udemy.com/course/vuejs-2-the-complete-guide/)

## 1. Getting Started

### What is Vue.js?

- A Javascript framwork that makes building interactive and reactive web frontends (= browser-side web applications) easier.

### Different Ways of Utilizing Vue

- Control parts of HTML pages or entire pages
    - Widget approach on a multi-page application
        - Some pages are still rendered on and served by a backend server
- Can also be used to control the entire frontend of a web application
    - Single-Page-Applicaton Approach.
        - Server sends only one HTML page, thereafter Vue takes over and controls the UI

### Building a First App with just Javascript

- Append item to a list.

```javascript
const buttonEl = document.querySelector('button');
const inputEl = document.querySelector('input');
const listEl = document.querySelector('ul');

function addGoal() {
    const enteredValue = inputEl.value;
    const listItemEl = document.createElement('li');
    listItemEl.textContent = enteredValue;
    listEl.appendChild(listItemEl);
}

buttonEl.addEventListener('click', addGoal);
```

### Re-building the App with Vue

- Using Vue from CDN

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>A First App</title>
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
    <div id="app">
      <div>
        <label for="goal">Goal</label>
        <input type="text" id="goal" v-model="enteredValue" />
        <button v-on:click="addGoal">Add Goal</button>
      </div>
      <ul>
        <li v-for="goal in goals">{{ goal }}</li>
      </ul>
    </div>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <script src="app.js"></script>
  </body>
</html>
```

```javascript
Vue.createApp({
   data() {
    return {
        goals: [],
        enteredValue: ''
    };
   } ,
   methods: {
    addGoal() {
        this.goals.push(this.enteredValue);
    }
   }
}).mount('#app');
```

### Course Content

- Basics - *Small Demos/Mini Projects*
    - Core Syntax
    - Templates
    - Directives
    - Data
    - Methods
    - Computed Properties
    - Watchers
- Intermediate - *Bigger Course Projects*
    - Components
    - Component Communication
    - Behind the Scenes
    - Forms
    - HTTP
    - Routing
    - Animations
- Advanced - *Assignments & Challenges*
    - Vuex
    - Authentication
    - Deployment & Optimizations
    - Composition API
    - Re-using Code

## Basics & Core Concepts - DOM Interaction with Vue

### Creating and Connecting Vue App Instances

- If we control a HTML element with Vue, we'll also control all child elements of that element

```html
<section id="user-goal">
  <h2>My Course Goal</h2>
  <p></p>
</section>
```

```javascript
const app = Vue.createApp({
  data() {
    return {
        courseGoal: 'Test!'
    };
  },
});

app.mount('#user-goal');
```

### Interpolation and Data Binding

- Reference properties that are part of the object

```html
<section id="user-goal">
  <h2>My Course Goal</h2>
  <p>{{ courseGoal }}</p>
</section>
```

### Binding Attributes with the "v-bind" Directive

- The `{{ }}` syntax is only available between opening and closing HTML tags
    - Passing dynamic value to an attribute, use Vue binding syntax, a directive
    - V-bind is a reserved name detected and understood by Vue
    - All built-in directives start with `v-`
```javascript
const app = Vue.createApp({
  data() {
    return {
        courseGoal: 'Test!',
        vueLink: 'https://vuejs.org/'
    };
  },
});

app.mount('#user-goal');
```
```html
<p>Learn more <a v-bind:href="vueLink">about Vue</a></p>
```

### Understanding "methods" in Vue apps

- Methods allow to define functions that execute when something happens
    - When you call them or a user event such as a click occurs
    - Takes an object, full of methods
    - `methods` is reserved term, such as `data`
    - All properties defined in methods object need to be functions
```javascript
const app = Vue.createApp({
  data() {
    return {
      courseGoal: 'Test!',
      vueLink: 'https://vuejs.org/',
    };
  },
  methods: {
    outputGoal() {
      const randomNumber = Math.random();
      if (randomNumber < 0.5) {
        return 'Learn Vue!';
      } else {
        return 'Master Vue!';
      }
    },
  },
});

app.mount('#user-goal');
```
```html
<section id="user-goal">
  <h2>My Course Goal</h2>
  <p>{{ outputGoal() }}</p>
  <p>Learn more <a v-bind:href="vueLink">about Vue</a></p>
</section>
```

### Working with Data inside of a Vue app

- Referencing data in methods
    - Vue packages data into the app object
```javascript
const app = Vue.createApp({
  data() {
    return {
      courseGoalA: 'Test A!',
      courseGoalB: 'Test B!',
      vueLink: 'https://vuejs.org/',
    };
  },
  methods: {
    outputGoal() {
      const randomNumber = Math.random();
      if (randomNumber < 0.5) {
        return this.courseGoalA;
      } else {
        return this.courseGoalB;
      }
    },
  },
});

app.mount('#user-goal');
```

### Outputting Raw HTML Content with v-html

- Using `v-html`
```javascript
...
courseGoalB: '<h2>Test B!</h2>',
...
```
```html
...
<p v-html="outputGoal()"></p>
...
```

### A First Summary

- Controlling section with Vue
    - And all child elements
- CSS Selector for mount
- Data-binding
- Interpolation syntax in HTML
- `v-bind` & `v-html`
- `data` & `methods` options
    - `data` returns object
    - `methods` returns functions you can call
- Declarative approach
    - Define the goal, define the template
    - Mark what is dynamic
    - Updates the DOM

### Event-binding

- React to user input, events
- `v-on` directive for adding event listeners
    - Takes event as argument after colon
    - i.e `v-on:click`
    - All default events, mouseenter, mouseleave, etc

```html
<section id="events">
  <h2>Events in Action</h2>
  <button v-on:click="counter++">Add</button>
  <button v-on:click="counter--">Reduce</button>
  <p>Result: {{ counter }}</p>
</section>
```
```javascript
const app = Vue.createApp({
  data() {
    return {
      counter: 0,
    };
  },
});

app.mount('#events');
```

### Events and Methods

- Moving logic out of HTML and into the Javascript code
- Can *point* to function, as opposed to calling it 
    - `<button v-on:click="add">Add</button>` instead of `<button v-on:click="add()">Add</button>`

```html
<section id="events">
  <h2>Events in Action</h2>
  <button v-on:click="add">Add</button>
  <button v-on:click="reduce">Reduce</button>
  <p>Result: {{ counter }}</p>
</section>
```
```javascript
const app = Vue.createApp({
  data() {
    return {
      counter: 0,
    };
  },
  methods: {
    add() {
      this.counter++;
    },
    reduce() {
      this.counter--;
    },
  },
});

app.mount('#events');
```

### Working with Event Arguments

- Add and Reduce by other numbers
    - Passing arguments

```javascript
methods: {
    add(num) {
      this.counter = this.counter + num;
    },
    reduce(num) {
      this.counter = this.counter - num;
    },
  },
```
```html
<button v-on:click="add(10)">Add 10</button>
<button v-on:click="reduce(5)">Reduce 5</button>
```

### Using the Native Event Object

- Utilizing default event object

```html
<input type="text"v-on:input="setName">
<p>Your Name: {{ name }}</p>
```
```javascript
methods: {
  setName(event) {
    this.name = event.target.value;
  },
  ...
},
```
- Accessing the event object
    - Default if pointing, need to state parameter with other args
        - `v-on:input="setName"`
        - `v-on:input="setName($event, 'Last Name')"`

### Exploring Event Modifiers

- Using form, input, and button
    - Dealing with button press default behavior to send HTTP request
    - Can do `event.preventDefault();` but there's another way with event modifier
- Can do prevent, right click only, key modifiers, etc

```html
<section id="events">
  <h2>Events in Action</h2>
  <button v-on:click="add(10)">Add 10</button>
  <button v-on:click.right="reduce(5)">Reduce 5</button>
  <p>Result: {{ counter }}</p>
  <input
    type="text"
    v-on:input="setName($event, 'Lofton')"
    v-on:keyup.enter="confirmInput"
  />
  <p>Your Name: {{ confirmedName }}</p>
  <form v-on:submit.prevent="submitForm">
    <input type="text" />
    <button>Sign Up</button>
  </form>
</section>
```
```javascript
const app = Vue.createApp({
  data() {
    return {
      counter: 0,
      name: '',
      confirmedName: '',
    };
  },
  methods: {
    confirmInput() {
      this.confirmedName = this.name;
    },
    submitForm() {
      alert('Submitted!');
    },
    setName(event, lastName) {
      this.name = event.target.value + ' ' + lastName;
    },
    add(num) {
      this.counter = this.counter + num;
    },
    reduce(num) {
      this.counter = this.counter - num;
    },
  },
});

app.mount('#events');
```

### Locking Content with v-once

- Some data that changes but want to preserve initial state
    - Utilizing `v-once`
```html
<p v-once>Starting Counter: {{ counter }}</p>
<p>Result: {{ counter }}</p>
```

