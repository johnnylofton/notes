# Javascript

## Usage and History

- Web Browsers
- Applications
    - Desktop - Electron
    - Mobile - Cordova
    - Server - Node.js
- History / Versions
    - 1995 - Created
    - 1997 - Standardizing
    - 1999 - ECMAScript 3
    - 2009 - ECMAScript 5
    - 2015 ECMAScript 2015 (ES6)
          - Yearly updates since

---

## Language Features

### Constants - variable that **cannot** change

- `const`
    - Must be initiated - `const x = 2`

### Variable Declarations

- `let`
    - Cannot be called before declared
    - Has [block scoping](http://www.benmvp.com/learning-es6-block-level-scoping-let-const/)
- `var`
    - undefined before declaration

### Rest Parameters

- The *rest* or remaining parameters
    - Must be last argument
    - `function x(z, ...y)`
        - `z` - named parameter
        - `...y` - rest parameter, as array

### Destructuring Arrays

- Assign values in array to variables

```javascript
let carIds = [1, 2, 5];
let [car1, car2, car3] = carIds;
// with rest parameters:
let car1, remainingCars;
[car1, ...remainingCars] = carIds
// log
// 1 [2, 5]
```

### Destructuring Objects

- `{ }` instead of `[ ]` for objects

```javascript
let car = { id: 5000, style: "convertible" }
let { id, style } = car;
// log
// 5000, convertible
```

- Put destructuring in `( )` if variables already declared

```javascript
let id, style;
({id, style} = car);
```

### Spread Syntax

- Take array, spread out elements for parameters
- Similar to rest syntax, does the opposite

```javascript
let carIds = [100, 300, 500];
startCars(...carIds);
```

- Can iterate through **arrays** *and* **strings**

### typeof()

- Returns a **string**

```javascript
typeof(1);              // 'number'
typeof(true);           // 'boolean'
typeof('Hello')         // 'string'
typeof(function() {});  // 'function'
typeof({});             // 'object'
typeof(null);           // 'object'
typeof(undefined)       // 'undefined'
typeof(NaN);            // 'number'
// NaN - not a number
```

### Common Type Conversions

- Convert to string - `foo.toString();`
- String to integer - `Number.parseInt('55');`
- String to number - `Number.parseFloat('55.99');`

### Controlling Loops

- Use `break` to get out of a loop
- Use `continue` to finish iteration (without rest of body)

---

## Operators

### Equality operators

- `(var1 == var2)` - JS will attempt to convert to matching types for comparison
- `(var1 === var2)` - No conversion, types must be equal. 'Strict Equality'

### Unary operators

- `++var` or `var++` - Increment
- `--var` or `var--` - Decrement
- `+var` - string to numertical type
- `-var` - negation, changes sign of numeric type

### Logical (Boolean) operators

- `&&`    - AND
- `||`    - OR
- `!`     - NOT, convert to bool, flip

### Relational Operators

- Compared by ASCII
- `>`, `>=` - greater than, greater than or equal to
- `<`, `<=` - less than, less than or equal to

### Conditional Operators

- `?` used in shorthand `if`
- `condition ? exprIfTrue : exprIfFalse`
    - ex: `console.log((5>4) ? 'yes' : 'no');   // yes`

### Assignment Operators

- `+=, -=, /=, *=, %=`
- `<<=` - shift bits to left
- `>>=` - shift bits to right
- `>>>=` - shift but keep the sign

### Operator Precendence

[Full Chart](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

- Highest to Lowest

1. Grouping (...)
2. Multiplication, Division, Remainder
3. Addition, Subtraction
4. Logical AND
5. Logical OR

---

## Functions and Scope

### Function scope

- Variables that can be accessed with a function of a nested function

- ***lifetime***

- If not available in function, looks to parent function

### Block scope

- `{ }` not in a function
- Using `let` for block scope
- No block scope for `var`, no such thing for these
- IIFE's - Immediately Invoked Function Expression
- ex:

```javascript
(function () {
    console.log('in function');
})();
// can return values this way
let app = (function () {
    let carId = 123;
    console.log('in function');
    return {};
})();

console.log(app);
// [Function: app]
```

### Closures

- Keeping a function, it's variables and nested function in scope
- ex:

```javascript
let app = (function() {
    let carId = 123;
    let getId = function() {
        return carId;
    };
    return {
        getId: getId    //  reference
    };
})();
console.log(app.getId());
// 123
```

### The **this** keyword

- Context for the function
- ex:

```javascript
let o = {
    carId: 123,
    getId: function() {
        return this.carId;
    }
};
console.log(o.getId());
// 123

```

- Context can change, ie `this` value can change

#### Call and apply

- Change values of `this` to change the object which is the context of the function
- With previous example:

```javascript
// call ex:
let newCar = { carId: 456 };
console.log(o.getId.call(newCar)); //456
// apply ex
/*
getId: function (prefix) {
    return prefix + this.carId;
}
*/
console.log(o.getId.apply(newCar, ['id: ']))
// ID: 456
```

- Call and apply similar, except *apply* accepts array of args

#### Bind

- Make copy of a function and change context / `this` value.
    - `let newFn = o.getId.bind(newCar);`

### Arrow Functions

- Function declarations
    - Arrow function symbol: `=>`
    - `let getId = () => 123; //no params`
- More examples

```javascript
let getId = prefix => prefix + 123;
console.log(getId('ID: '));         // ID: 123

let getId = (prefix, suffix) => prefix + 123 + suffix;
console.log(getId('ID: ', '!'));    // ID: 123!

// with braces / return keyword required
let getId = (prefix, suffix) => {
    return prefix + 123 + suffix;
};
// same result as previous

// use with underscore:
let getId = _ => 123;
```

>Arrow functions do **NOT** have their own `this` value. `this` refers to enclosing context

### Default Parameters

- ex:

```javascript
let trackCar = function(carId, city='NY') {
    // using backticks for interpreting variables
    console.log(`Tracking ${carId} in ${city}.`);
}
```

- Like other defaults, must be on right side.
- Default overwritten if defined.

---

## Objects and Arrays

### Constructor Functions

- Examples

```javascript
function Car() { }          // capitalized name as convention`
let car = new Car();
```

```javascript
function Car(id) {
    this.carId = id;
}
let car = new Car(123);
console.log(car.carId);     // 123
```

- Method: function run on an object

```javascript
function Car(id) {
    this.carId = id;
    this.start = function () {
        console.log('Start: ' + this.carId);
    };
}
let vehicle = new Car(123);
vehicle.start();            // Start: 123
```

---

### Prototypes

```javascript
// using previous example
Car.prototype.start = function() {
    console.log('Start: ' + this.carId);
}   // single copy, instead of one function for each object
```

- Expanding Objects using Prototypes
    - Give new functionality to objects

```javascript
String.prototype.hello = function() {
    return this.toString() + ' Hello';
};
console.log('foo'.hello());     // foo Hello
```

### Javascript Object Notation (JSON)

```javascript
let car = {
    id: 123,
    style: 'convertible'
};
console.log(JSON.stringify(car));
// { "id": 123, "style": "convertible" }
```

```javascript
// Array to JSON
let carIds = [
    { carId: 123 },
    { carId: 456 },
    { carId: 789 }
];
console.log(JSON.stringify(carIds));
//  [{ "carId": 123 }, { "carId": 456 }, ...]
// Parsing JSON:
let jsonIn = [{ "carId": 123 }, { "carId": 456 }, { "carId": 789 }];
let carIds = JSON.parse(jsonIn);
// log: [ { carId: 123 }, { carId: 456 }, { carId: 789 } ]
```

---

### Array Iteration

Examples:

```javascript
carIds.foreach(car => console.log(car));
carIds.foreach((car, index) => console.log(car, index));

//  only some elements
let convertibles = carIds.filter(car => car.style === 'convertible');

// every case. find, condition, T/F, all elements
let result = carIds.every(car => car.carId > 0);    // true

// retrieve first instance matching condition
let car = carIds.find(car => car.carId > 500);
```

---

## Classes and Modules

- Class Basics

```javascript
class Car { };
let car = new Car();
```

### Constructors and properties

- constructor - function executed when new instance of a class is created

    ```javascript
    class Car() {
        constructor(id) {   // constructor
            this.id = id;   // property
        }
    }   // car.id to access property directly
    ```

### Methods

- No function keyword needed
    - Example within class `Car`

    ```javascript
    identify(params) {
        return `Car Id: ${this.id}`;
        // dont need `this` to access
    }
    ```

### Inheritance

- Example:

```javascript
class Car extends Vehicle {
    constructor() {
        super();    // refers back to parent Vehicle
    }
    start() {
        return 'In Car Start' + super.start();
    }
}
```

### Creating and Importing a Module

- Create & Export
    - `export class Car { ... }`
- Import
    - `import { Car } from './models/car.js'`

---

## Programming the BOM and DOM

- BOM - Browser Object Model
- DOM - Document Object Model

### Window Object

- Global object
- Properties
    - document
    - location
    - console
    - innerHeight
    - innerWidth
    - pageXOffset
    - pageYOffset
- Methods
    - alert()
    - back()
    - confirm()
- Events
    - ( not common )
- gGlobal object, must refer when dealing with modules

### Timers

- fire asynchronously
    - `setTimeout(); // once`
    - `setInterval(); // repeatedly`

    ```javascript
    let timeoutId = setTimeout(function() {
        console.log('1 second paused');
    }, 1000);

    // cancel
    clearTimeout(timeoutId);
    // or
    clearTimeout(id);
    ```

---

### Location object

- Properties
    - href (URL)
    - hostname
    - port
    - pathname
    - search
- Methods
    - assign()
    - reload()
ex: `location.href` or `document.location.href`

---

### Document Object

- Properties
    - body
    - forms
    - links
- Methods
    - createElement()
    - createEvent()
    - getElementById()
    - getElementsByClassName()
- Events
    - onload
    - onclick
    - onKeypress

---

### Selecting DOM Elements

- Common:
    - `document.getElementById('elementId');`
    - `document.getElementByClassName('className');`
    - `document.getElementByTagName('tagName');`

### Modifying DOM Elements

- Example:

    ```javascript
    let el = document.getElementById('elementId');
    el.textContent = 'new text here';
    el.setAttribute('name', 'nameValue');
    el.classList.add('myClassName');
    el.style.color = 'blue';
    ```

---

## Promises and Error Handling

### Errors in Javascript

- `let car = newCar;` // reference error, execution stops
- Error Handling with try and catch

    ```javascript
    try {
        let car = newCar;
    } catch (error) {
        console.log('error: ', error);
    }
    // continue execution
    ```

- With finally (always executes)

    ```javascript
    finally {
        console.log('this always executes');
    }

- Developer defined errors

    ```javascript
    try {
        throw new Error('any custom error');
    }
    ```

- Creating a Promise
    - Temporary holder for a value you will retrieve after asynchronous call

    ```javascript
    let promise = new Promise (
        function(resolve, reject) {
            setTimeout(resolve, 100, 'someValue');
        }
    );
    ```

- Setting a Promise

    ```javascript
    promise.then(
        value => console.log('fulfilled: ' + value);
        error => console.log('rejected: ' + error);
    );
    ```

---

## Data Access Using HTTP

### HTTP Requests using XHR

- XML HTTP Requests

    ```javascript
    let xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function() {
        if (this.readyState == 4 && this.status == 200) {
            console.log(this.responseText);
        }
    };
    xttp.open("GET", "http://myid.mockapi.io/api/v1/users", true);
    xttp.send();
    ```

- HTTP Requests with jQuery

    ```javascript
    import $ from 'jquery';

    let promise = $.get(
        "http://myid.mockapi.io/api/u1/users",
        data => console.log('data: ', data)
    );  // returns promise

    promise.then(
        data => console.log('success', data),
        error => console.log('error: ', error)
    );
    // POST
    $.post("url", user); // user -> data
    ```

---

## Forms

### Preventing Form Submission

- Form -> .js -> Server
    - submit event

    ```javascript
    let form = document.getElementById('user-form');

    form.addEventListener('submit', event => {
        // prevent browser from submitting
        event.preventDefault();
    });
    ```

### Accessing Form Fields

```javascript
form.addEventListener('submit', event => {
    let user = form.elements['user'];
    let avatarFile = form.elements['avatar-file'];

    console.log(user.value, avatarFile.value);
});
```

### Showing Validation Errors

  ```javascript
  let userError = document.getElementById('user-error');
  userError.textContent = 'Invalid Entry';
  userError.style.color = 'red';
  user.style.borderColor = 'red';
  user.focus();
  ```

### Posting from Javascript

post, data to object levels from form

---

## Chrome Dev Tools and Security

- Network >> bundle.js file
- Sources >> Watch, etc
- Don't store sensitive info on browser

### Security and the eval() function

- JS Global eval() function will execute whatever is passed
- Avoid eval() altogether? Script tags?

### Preventing Man-in-the-Middle Attacks

- Code put into HTML between server and intended client
- Use SSL, use HTTP header
- Cookie attributes: Secure and HttpOnly

### Cross-site Scripting (XSS)

- Files from 3rd party servers
- Addressing XSS attacks
    - CSP: Content Security Policy
        - Use HTTP Header: Content-Security-Policy
    - CORS: Cross Origin Resource Sharing
        - Use HTTP Header:
              - Access-Control-Allow-Origin

### Building Your Application for Production

- Minimizing bundle file
    - `npm run dev`
    - `npm run build`
    - dist folder, files for server
