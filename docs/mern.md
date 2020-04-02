# MERN Stack

Notes based on Udemy Course [React, NodeJS, Express & MongoDB - The MERN Fullstack Guide](https://www.udemy.com/course/react-nodejs-express-mongodb-the-mern-fullstack-guide)

## 1. Introduction

### What is the "MERN Stack"

- **M**ongoDB
- **E**xpress
- **R**eact
- **N**ode.js

#### What is React?

- A Client-Side (Browser) Library which allows you to build highly reactive user interfaces
    - Render UI with Dynamic Data
    - Handle User Input
    - Communicate with Backend Services
    - Provides a "Mobile App"-like User Experience

#### What is Node?

- A Server-side Runtime: Javascript on the Server-side
    - Listen to Requests and Send Responses
    - Execute Server-side Logic
    - Interact with Databases and Files
    - An Alternative to PHP, Ruby on Rails, Java etc. Is rarely used Standalone!

#### What is Express? 

- A Node Framework which simplifies writing Server-side Code and Logic.
    - Based on Node, offers same Functionalities
    - Middleware-based: Funnel Requests through Functions
    - Includes Routing, View-rendering & More
    - Simplifies the Usage of Node. Express is for Node what Laravel would be for PHP.

#### What is MongoDB?

- A NoSQL Database which stores "Documents" in "Collections" (instead of "Records" in "Tables" as in SQL).
    - Store Application Data (Users, Products, ...)
    - Enforces no Data Schema or Relations
    - Easily connected to Node/Express (NOT to React!)
    - A powerful Database which can easily be integrated into a Node/Express Environment

### MERN - A First Overview

![bigpicture](img/mern-big-picture.jpg "mern big picture")

### Course Outline

1. Getting Started
2. MERN in Theory
3. React Frontend
    - Components, Hooks, Routing
    - Frontend User Input Validation
4. Node + Express + MongoDB Backend
    - REST API, ROutes, Controllers, Models
    - Backend User Input
5. Connecting Both Ends
6. File Upload
7. Authentication
8. Deployment

## 2. The MERN Stack - Theory

### Understanding the Big Picture

![bigpicture2](img/mern-big-picture2.jpg "mern big picture2")

### Diving into the Frontend

- React SPA (Single Page Application)

![frontend-dive](img/frontend-dive.jpg "frontend dive")

### Understanding the Backend

![backend-dive](img/backend-dive.jpg "backend dive")

### REST vs GraphQL

![mern-rest1](img/mern-rest1.jpg "mern rest 1")

![mern-rest2](img/mern-rest2.jpg "mern rest 2")

![mern-graphql1](img/mern-graphql1.jpg "mern graphQL 1")

![mern-graphql2](img/mern-graphql2.jpg "mern graphQL 2")

![mern-restvgraphql](img/mern-restvgraphql.jpg "mern rest v graphQL")

### Connecting Node & React

![connecting-ends1](img/mern-connecting-ends1.jpg "connecting ends 1")

![connecting-ends2](img/mern-connecting-ends2.jpg "connecting ends 2")

### Diving Deeper into the Code

#### Using simple-mern-project

##### Backend

- Basic backend server setup

```javascript
// backend/server.js

const express = require('express');
const bodyParser = require('body-parser');
const uuid = require('uuid/v4');

const app = express();

const DUMMY_PRODUCTS = []; // not a database, just some in-memory storage for now

app.use(bodyParser.json());

// CORS Headers => Required for cross-origin/ cross-server communication
app.use((req, res, next) => {
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader(
    'Access-Control-Allow-Headers',
    'Origin, X-Requested-With, Content-Type, Accept, Authorization'
  );
  res.setHeader(
    'Access-Control-Allow-Methods',
    'GET, POST, PATCH, DELETE, OPTIONS'
  );
  next();
});

app.get('/products', (req, res, next) => {
  res.status(200).json({ products: DUMMY_PRODUCTS });
});

app.post('/product', (req, res, next) => {
  const { title, price } = req.body;

  if (!title || title.trim().length === 0 || !price || price <= 0) {
    return res.status(422).json({
      message: 'Invalid input, please enter a valid title and price.'
    });
  }

  const createdProduct = {
    id: uuid(),
    title,
    price
  };

  DUMMY_PRODUCTS.push(createdProduct);

  res
    .status(201)
    .json({ message: 'Created new product.', product: createdProduct });
});

app.listen(5000); // start Node + Express server on port 5000
```

##### Frontend

- Components
    - Functions or classes
- `index.js`
    - Entry point to frontend Application
- `App.js`
    - HTML **in** javascript
    - Use React Hooks
        - Help with managing state
            - useState, useEffect

## 3. Planning the App

### Module Introduction

#### General Planning Steps

1. Come up with an idea / solve a problem
2. Create a design / sketch
3. Plan your data models (the data you'll work with)
4. Plan your endpoints (API, backend) and pages (SPA, frontend)

### Understanding the General App Idea

![the-idea](img/theidea.jpg "the idea")

### Data & API Endpoints used in our App

![app-data](img/app-data.jpg "app data")

### Required SPA Pages for the Frontend

![api-endpoints](img/mern-api-endpoints.jpg "api endpoints")

![spa-pages](img/mern-spa-pages.jpg "spa pages")

## 4. React.js - A Refresher

### What is React?

- A Javascript Library* for Building User Interfaces
    - *started a huge eco-system, effectively forming a "pseudo-framework"
- Declarative Approach
    - You define the result, not the steps that lead to the result
- Imperative Approach ("normal JS")
    - You define all the steps to achieve the result
- You define **components** and build your UI with these components

### Setting Up a Starting Project

`create-react-app`

### Understanding JSX

- Syntax in React
- HTML-like code in Javascript files
    - Translates to instructions React understands
- Example: 
```javascript
import React from 'react';

const App = () => {
  return <h1>A React App!</h1>;
  // similar to React.createElement('h1', {}, 'Hello!');
};

export default App;
```
- Similar example:
```javascript
const App = () => {
  return <h1 title="it works!">Hello!</h1>;
  // similar to React.createElement('h1', {title: 'it works!'}, "Hello!");
};
```

### Understanding Components

- All about Components
- Can be a function which returns JSC or returns React.createElement() calls.
- Can be a javascript class with a render method
    - *some technical differences to function*
    - *in modern React, you can **only use functional components***
- Previous example as a class:
```javascript
class App extends React.Component {
  render() {
      return <h1 title="it works!">Hello! This is ReactJS!</h1>;
  }
}
```
- We'll only be using functional in this course
    - Just a regular javascript function in the end
        - What makes it a React component is that it returns JSX or React.createElement() logic
- Capitalize components in tags, otherwise will be treated as built-in HTML tag: `<App />`

### Working with Multiple Components

- Must use className and not class in JSX code
    - `<div className="name">`
- Styles all applied globally (in App.css here, at least)
    - Styles are not automatically scoped to a component, normal CSS rules are applied, because all CSS files, no matter where you import them, are applied globally to entire page.
- GoalList component example:
```javascript
// src/components/GoalList.js
import React from 'react';

import './GoalList.css';

const GoalList = () => {
	return (
		<ul className='goal-list'>
			<li>Finish the Course</li>
			<li>Learn all about Course main topics</li>
			<li>Help others!</li>
		</ul>
	);
};

export default GoalList;
```
```javascript
// src/App.js
import React from 'react';

import GoalList from './components/GoalList';
import './App.css';

const App = () => {
  return <div className="course-goals">
    <h2>Course Goals</h2>
    <GoalList />
  </div>;
};

export default App;
```

### Using Props to pass Data between Components

- Pass data from component to component with a concept called ***props***
- Changing GoalList to dynamic
- `props` as GoalList parameter, object passed to React functional component automatically, object that is bundle of **all** the `props` you passed to component

### Rendering Lists of Data

- Can use hardcoded data, but need to map every object into a list item in the end.
- Will need to use `map()` JS function.
- Must also use unique key
- GoalList Example:

```javascript
// src/App.js

const App = () => {
  const courseGoals = [
    {id: 'cg1', text: 'Finish the course!'},
    {id: 'cg2', text: 'Learn all about Course main topics'},
    {id: 'cg3', text: 'Help others!'},
  ];

  return <div className="course-goals">
    <h2>Course Goals</h2>
    <GoalList goals={courseGoals} />
  </div>;
};

export default App;
```

```javascript
// src/components/GoalList.js
import React from 'react';

import './GoalList.css';

const GoalList = props => {
	return (
		<ul className='goal-list'>
			{props.goals.map(goal => {
				return <li key={goal.id}>{goal.text}</li>;
			})}
		</ul>
	);
};

export default GoalList;
```

### Handling Events

- React component **must** return JSX
- Can add event listeners to any element
    - Will use `onSubmit` with form here, must ***point*** to a function (don't give parentheses)
    - Can use `onClick` and others
    - Function will get an event object as parameter passed in automatically by React
        - Can prevent default behavior (form submission)
- Now pass data from NewGoal to the parent (App component)

### Efficient Child<=>Parent Communication

- Pass a callback function from the parent to the child
- Technically in form, used all props.
    - No difference between class name where a string is passed and on submit where a pointer to function is passed.
- Can use this to pass a prop onto NewGoal in App.js, which holds pointer to a function.
- Example so far:

```javascript
// src/components/NewGoal/NewGoal.js
import React from 'react';

import './NewGoal.css';

const NewGoal = props => {
  const addGoalHandler = event => {
    event.preventDefault();
    
    const newGoal = {
      id: Math.random().toString(),
      text: 'my new goal!!'
    };

    props.onAddGoal(newGoal);
  };

  return <form className="new-goal" onSubmit={addGoalHandler}>
    <input type="text" />
    <button type="submit">Add Goal</button>
  </form>
};

export default NewGoal;
```

```javascript
// src/App.js
import React from 'react';

import GoalList from './components/GoalList/GoalList';
import NewGoal from './components/NewGoal/NewGoal';
import './App.css';

const App = () => {
  const courseGoals = [
    {id: 'cg1', text: 'Finish the course!'},
    {id: 'cg2', text: 'Learn all about Course main topics'},
    {id: 'cg3', text: 'Help others!'},
  ];

  const addNewGoalHandler = (newGoal) => {
    courseGoals.push(newGoal);
    console.log(courseGoals);
  }

  return <div className="course-goals">
    <h2>Course Goals</h2>
    <NewGoal onAddGoal={addNewGoalHandler} />
    <GoalList goals={courseGoals} />
  </div>;
};

export default App;
```

### Working with "State"

- Need to explicitly tell React when to update UI
    - Do this using state
- Use ***hook function*** `useState` 
- `useState()` returns array of exactly 2 elements
    1. Latest state snapshot
    2. Function that allows us to update that state and tell React it should re-render
- Pass `courseGoals` array into `useState()`
- Updated App.js:

```javascript
// src/App.js
import React, { useState } from 'react';

import GoalList from './components/GoalList/GoalList';
import NewGoal from './components/NewGoal/NewGoal';
import './App.css';

const App = () => {
  const [courseGoals, setCourseGoals] = useState([
    {id: 'cg1', text: 'Finish the course!'},
    {id: 'cg2', text: 'Learn all about Course main topics'},
    {id: 'cg3', text: 'Help others!'},
  ]);

  const addNewGoalHandler = (newGoal) => {
    setCourseGoals(courseGoals.concat(newGoal));
  }

  return <div className="course-goals">
    <h2>Course Goals</h2>
    <NewGoal onAddGoal={addNewGoalHandler} />
    <GoalList goals={courseGoals} />
  </div>;
};

export default App;
```

### More on State

- State is a **crucial** concept in React
- Whenever you update the state snapshot, React does 2 things
    1. Update internally stored state data.
    2. Will then can the component function again
- Re-evaluates entire component, not necessarily the entire DOM
- `useState()` ignores initial and previous snapshots on update, replaces it. This is why we passed `courseGoals.concat(newGoal)` to `setCourseGoals()`
    - This is actually **not** the best possible way to update state, due to deferring state updates.
        - Better method: 
            - `setCourseGoals((prevCourseGoals) => { return prevCourseGoals.concat(newGoal); });`
            - Shorthand version: `setCourseGoals(prevCourseGoals => prevCourseGoals.concat(newGoal));`
        - Only need if state update depends on previous state data.

### Fetching User Input (Two-way Binding)

- Store user input in some variable, the pass to new goal.
    - Using `onChange` event
- Flawed on its own here, even when giving value of variable to input tag
- Must use state
    - Manipulate and reset input field
- Updated NewGoal.js:

```javascript
// src/components/NewGoal/NewGoal.js
import React, { useState } from 'react';

import './NewGoal.css';

const NewGoal = props => {
  const [enteredText, setEnteredText] = useState('');

  const addGoalHandler = event => {
    event.preventDefault();
    
    const newGoal = {
      id: Math.random().toString(),
      text: enteredText
    };

    setEnteredText('');

    props.onAddGoal(newGoal);
  };

  const textChangeHandler = event => {
    setEnteredText(event.target.value);
  };

  return <form className="new-goal" onSubmit={addGoalHandler}>
    <input type="text" value={enteredText} onChange={textChangeHandler} />
    <button type="submit">Add Goal</button>
  </form>
};

export default NewGoal;
```

## 5. React.js - Building the Frontend

### Introduction

1. Create React (Single Page) App
2. Add Routing & Core Pages
3. Split Into Core Components
4. UI Components + CSS Styling

### Starting Setup, Paes & Routes

- Two ways to organize
    1. Pages & Components folders
    2. Separate by Feature
        - Will use this method here:
```
places/-|         
        |- pages/
        |- components/
shared/-|
        |- components/
  user/-|    
        |- pages/
        |- components/
```

- Install third party routing package
    - `npm i --save react-router-dom`
- App component will be starting point for router
    - Route path treated as filter, use `exact` property.
- Use `Redirect` for others as well
- Use `Switch` to not redirect if able to find a path
- App.js so far:
```javascript
import React from 'react';
import { BrowserRouter as Router, Route, Redirect, Switch } from 'react-router-dom';

import Users from './user/pages/Users';
import NewPlace from './places/pages/NewPlace';

const App = () => {
  return (
    <Router>
      <Switch>
        <Route path="/" exact>
          <Users />
        </Route>
        <Route path="/places/new" exact>
          <NewPlace />
        </Route>
        <Redirect to="/" />
      </Switch>
    </Router>
  );
}

export default App;
```

### Adding a UsersList Page / Component

#### Wiring up User components

- User.js so far:
```javascript
// src/user/pages/User.js
import React from 'react';

import UsersList from '../components/UsersList';

const Users = () => {
	const USERS = [
		{
			id: 'u1',
			name: 'John',
			image:
				'https://images.heb.com/is/image/HEBGrocery/cat-medium/flowers-2868.jpg',
			places: 3
		}
	];

	return <UsersList items={USERS} />;
};

export default Users;
```
- UsersList.js so far:
```javascript
import React from 'react';

import UserItem from './UserItem';
import './UsersList.css';

const UsersList = props => {
	if (props.items.length === 0) {
		return (
			<div className='center'>
				<h2>No users found.</h2>
			</div>
		);
	}

	return (
		<ul>
			{props.items.map(user => (
				<UserItem
					key={user.id}
					id={user.id}
					image={user.image}
					name={user.name}
					placeCount={user.places}
				/>
			))}
		</ul>
	);
};

export default UsersList;
```
- UserItem.js so far:
```javascript
import React from 'react';

import './UserItem.css';

const UserItem = props => {};

export default UserItem;
```

### Adding a UserItem Component

- Added `className="users-list"` to `<ul>` tag in UsersList.js

- UserItem.js so far:
```javascript
// src/user/components/UserItem.js
import React from 'react';

import './UserItem.css';

const UserItem = props => {
  return (
    <li className="user-item">
      <div className="user-item__content">
        <div className="user-item__image">
          <img src={props.image} alt={props.name}/>
        </div>
        <div className="user-item__info">
          <h2>{props.name}</h2>
          <h3>{props.placeCount} {props.placeCount === 1 ? 'Place' : 'Places'}</h3>
        </div>
      </div>
    </li>
  );
};

export default UserItem;
```

### Styling our App & More Components

- Use Avatar.js & Avatar.css and Card.js & Card.css provided for styling.
- Updated UserItem.js:
```javascript
import React from 'react';
import { Link } from 'react-router-dom';

import Avatar from '../../shared/components/UIElements/Avatar';
import Card from '../../shared/components/UIElements/Card';
import './UserItem.css';

const UserItem = props => {
	return (
		<li className='user-item'>
				<Card className='user-item__content'>
					<Link to={`/${props.id}/places`}>
						<div className='user-item__image'>
							<Avatar image={props.image} alt={props.name} />
						</div>
						<div className='user-item__info'>
							<h2>{props.name}</h2>
							<h3>
								{props.placeCount} {props.placeCount === 1 ? 'Place' : 'Places'}
							</h3>
						</div>
					</Link>
				</Card>
		</li>
	);
};

export default UserItem;
```

### Presentational vs Stateful Components

- Presentational
    - Just output content, structure, styling
    - Card/Avatar components are good examples of this. UserItem also.
- Stateful
    - Smarter, manage some state, orchestrate that child components receive new data, etc.

### Adding a Main Header

- Add Card component to UsersList in event of no users.
- `props.children` special prop that React knows.
    - Will always refer to the things you pass between your opening and closing tags of component.
    - Placeholder for the content you enter between the opening and closing tag of your own component.

```javascript
// src/share/components/Navigation/MainHeader.js
import React from 'react';

import './MainHeader.css';

const MainHeader = props => {
	return <header className='main-header'>{props.children}</header>;
};

export default MainHeader;
```

```javascript
// src/share/components/Navigation/MainNavigation.js
import React from 'react';
import { Link } from 'react-router-dom';

import MainHeader from './MainHeader';
import './MainNavigation.css';

const MainNavigation = props => {
	return (
		<MainHeader>
			<button className='main-navigation__menu-btn'>
				<span />
				<span />
				<span />
			</button>
			<h1 className='main-navigation__title'>
				<Link to='/'>YourPlaces</Link>
			</h1>
			<nav>...</nav>
		</MainHeader>
	);
};

export default MainNavigation;
```

### Adding Navlinks

```javascript
// src/shared/components/Navigation/NavLinks.js
import React from 'react';
import { NavLink } from 'react-router-dom';

import './NavLinks.css';

const NavLinks = props => {
  return <ul className="nav-links">
    <li>
      <NavLink to="/" exact>ALL USERS</NavLink>
    </li>
    <li>
      <NavLink to="/u1/places">MY PLACES</NavLink>
    </li>
    <li>
      <NavLink to="/places/new">ADD PLACE</NavLink>
    </li>
    <li>
      <NavLink to="/auth">AUTHENTICATE</NavLink>
    </li>
  </ul>
};

export default NavLinks;
```

- Then insert in MainNavigation `<nav><NavLinks /></nav>`

### Implementing a Basic Desktop & Mobile Navigation

#### Two Main Changes

- Add CSS class to nav in MainNavigation.js
    - `<nav className='main-navigation__header-nav'><NavLinks /></nav>
- Develop SideDrawer.js
- Note: Can only have one *root* jsx element per component
    - Can use React.Fragment to fulfill this requirement.
- SideDrawer.js so far:
```javascript
import React from 'react';
import { Link } from 'react-router-dom';

import MainHeader from './MainHeader';
import NavLinks from './NavLinks';
import SideDrawer from './SideDrawer';

import './MainNavigation.css';

const MainNavigation = props => {
	return (
		<React.Fragment>
			<SideDrawer>
				<nav className='main-navigation__drawer-nav'>
					<NavLinks />
				</nav>
			</SideDrawer>
			<MainHeader>
				<button className='main-navigation__menu-btn'>
					<span />
					<span />
					<span />
				</button>
				<h1 className='main-navigation__title'>
					<Link to='/'>YourPlaces</Link>
				</h1>
				<nav className='main-navigation__header-nav'>
					<NavLinks />
				</nav>
			</MainHeader>
		</React.Fragment>
	);
};

export default MainNavigation;
```

### Understanding Portals

- Portals in React allows us to project/render a component in a different place than it would normally be rendered.
- Add `<div id="drawer-hook"></div>` into index.html, in body just after no script. This is where SideDrawer will be rendered.
- SideDrawer.js so far:
```javascript
import React from 'react';
import ReactDOM from 'react-dom';

import './SideDrawer.css';

const SideDrawer = props => {
  const content = <aside className="side-drawer">{props.children}</aside>;

  return ReactDOM.createPortal(content, document.getElementById('drawer-hook'));
};

export default SideDrawer;
```
- Drawer still always visible, no animation.

### Handling the Drawer State

- Add `<div id="backdrop-hook"></div>` into index.html, just before drawer hook.
- Backdrop.js
```javascript
import React from 'react';
import ReactDOM from 'react-dom';

import './Backdrop.css';

const Backdrop = props => {
  return ReactDOM.createPortal(
    <div className="backdrop" onClick={props.onClick}></div>,
    document.getElementById('backdrop-hook')
  );
};

export default Backdrop;
```
- SideDrawer.js so far:
```javascript
import React from 'react';
import ReactDOM from 'react-dom';

import './SideDrawer.css';

const SideDrawer = props => {
  const content = <aside className="side-drawer">{props.children}</aside>;

  return ReactDOM.createPortal(content, document.getElementById('drawer-hook'));
};

export default SideDrawer;
```

### Animating the Sidedrawer

- Install third party library
    - `npm i --save react-transition-group`
- SideDrawer.js:
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { CSSTransition } from 'react-transition-group';

import './SideDrawer.css';

const SideDrawer = props => {
	const content = (
		<CSSTransition
			in={props.show}
			timeout={200}
			classNames='slide-in-left'
			mountOnEnter
			unmountOnExit
		>
			<aside className='side-drawer' onClick={props.onClick}>{props.children}</aside>
		</CSSTransition>
	);

	return ReactDOM.createPortal(content, document.getElementById('drawer-hook'));
};

export default SideDrawer;
```

- MainNavigation.js
```javascript
import React, { useState } from 'react';
import { Link } from 'react-router-dom';

import MainHeader from './MainHeader';
import NavLinks from './NavLinks';
import SideDrawer from './SideDrawer';
import BackDrop from '../UIElements/Backdrop';

import './MainNavigation.css';

const MainNavigation = props => {
  const [drawerIsOpen, setDrawerIsOpen] = useState(false);
  
  const openDrawer = () => {
    setDrawerIsOpen(true);
  };

  const closeDrawerHandler = () => {
    setDrawerIsOpen(false);
  };

	return (
		<React.Fragment>
      {drawerIsOpen && <BackDrop onClick={closeDrawerHandler} />}
      <SideDrawer show={drawerIsOpen} onClick={closeDrawerHandler}>
        <nav className='main-navigation__drawer-nav'>
          <NavLinks />
        </nav>
      </SideDrawer>
			<MainHeader>
				<button className='main-navigation__menu-btn' onClick={openDrawer}>
					<span />
					<span />
					<span />
				</button>
				<h1 className='main-navigation__title'>
					<Link to='/'>YourPlaces</Link>
				</h1>
				<nav className='main-navigation__header-nav'>
					<NavLinks />
				</nav>
			</MainHeader>
		</React.Fragment>
	);
};

export default MainNavigation;
```

### Rendering User Places & Using Dynamic Route Segments

- PlaceItem.js so far:
```javascript
// src/places/components/PlaceItem.js
import React from 'react';

import Card from '../../shared/components/UIElements/Card';
import './PlaceItem.css';

const PlaceItem = props => {
  return (
    <li className='place-item'>
      <Card className='place-item__content'>
        <div className='place-item__image'>
          <img src={props.image} alt={props.title} />
        </div>
        <div className='place-item__info'>
          <h2>{props.title}</h2>
          <h3>{props.address}</h3>
          <p>{props.description}</p>
        </div>
        <div className='place-item__actions'>
          <button>VIEW ON MAP</button>
          <button>EDIT</button>
          <button>DELETE</button>
        </div>
      </Card>
    </li>
  );
};

export default PlaceItem;
```

- PlaceList.js so far:
```javascript
// src/places/components/PlaceList.js
import React from 'react';

import Card from '../../shared/components/UIElements/Card';
import PlaceItem from './PlaceItem';
import './PlaceList.css';

const PlaceList = props => {
  if (props.items.length === 0) {
    return (
      <div className='place-list center'>
        <Card>
          <h2>No places found. Maybe create one?</h2>
          <button>Share Place</button>
        </Card>
      </div>
    );
  }

  return (
    <ul className='place-list'>
      {props.items.map(place => (
        <PlaceItem
          key={place.id}
          id={place.id}
          image={place.imageUrl}
          title={place.title}
          description={place.description}
          address={place.address}
          creatorId={place.creator}
          coordinates={place.location}
        />
      ))}
    </ul>
  );
};

export default PlaceList;
import React from 'react';

import Card from '../../shared/components/UIElements/Card';
import PlaceItem from './PlaceItem';
import './PlaceList.css';

const PlaceList = props => {
  if (props.items.length === 0) {
    return (
      <div className='place-list center'>
        <Card>
          <h2>No places found. Maybe create one?</h2>
          <button>Share Place</button>
        </Card>
      </div>
    );
  }

  return (
    <ul className='place-list'>
      {props.items.map(place => (
        <PlaceItem
          key={place.id}
          id={place.id}
          image={place.imageUrl}
          title={place.title}
          description={place.description}
          address={place.address}
          creatorId={place.creator}
          coordinates={place.location}
        />
      ))}
    </ul>
  );
};

export default PlaceList;
```

- UserPlaces.js so far:
```javascript
// src/places/pages/UserPlaces.js
import React from 'react';

import PlaceList from '../components/PlaceList';

const UserPlaces = () => {
  const DUMMY_PLACES = [
    {
      id: 'p1',
      title: 'Empire State Building',
      description: 'One of the most famous sky scrapers in the world!',
      imageUrl:
        'https://upload.wikimedia.org/wikipedia/commons/1/10/Empire_State_Building_%28aerial_view%29.jpg',
      address: '20 West 34th Street, New York City, NY 10118-0114',
      location: {
        lat: 40.7484405,
        lng: -73.9878584
      },
      creator: 'u1'
    },
    {
      id: 1,
      title: 'Empire State Building',
      description: 'One of the most famous sky scrapers in the world!',
      imageUrl:
        'https://upload.wikimedia.org/wikipedia/commons/1/10/Empire_State_Building_%28aerial_view%29.jpg',
      address: '20 West 34th Street, New York City, NY 10118-0114',
      location: {
        lat: 40.7484405,
        lng: -73.9878584
      },
      creator: 'u2'
    }
  ];

  return <PlaceList items={DUMMY_PLACES} />;
};

export default UserPlaces;
```

- In App.js:
```javascript
// src/App.js
import UserPlaces from './places/pages/UserPlaces';

// in MainNavigation:
<Route path="/:userId/places">
  <UserPlaces />
</Route>
```

### Getting Route Params

- Using `useParams()`

```javascript
// src/places/pages/UserPlaces.js
import { useParams } from 'react-router-dom';
// ...
const UserPlaces = () => {
  const userId = useParams().userId;
  const loadedPlaces = DUMMY_PLACES.filter(place => place.creator === userId);
  return <PlaceList items={loadedPlaces} />;
};
```

### Adding Custom Buttons

- Using provided Button.js:

```javascript
// src/shared/components/FormElements/Button.js
import React from 'react';
import { Link } from 'react-router-dom';

import './Button.css';

const Button = props => {
  if (props.href) {
    return (
      <a
        className={`button button--${props.size || 'default'} ${props.inverse &&
          'button--inverse'} ${props.danger && 'button--danger'}`}
        href={props.href}
      >
        {props.children}
      </a>
    );
  }
  if (props.to) {
    return (
      <Link
        to={props.to}
        exact={props.exact}
        className={`button button--${props.size || 'default'} ${props.inverse &&
          'button--inverse'} ${props.danger && 'button--danger'}`}
      >
        {props.children}
      </Link>
    );
  }
  return (
    <button
      className={`button button--${props.size || 'default'} ${props.inverse &&
        'button--inverse'} ${props.danger && 'button--danger'}`}
      type={props.type}
      onClick={props.onClick}
      disabled={props.disabled}
    >
      {props.children}
    </button>
  );
};

export default Button;
```

- Use in PlaceItem.js:

```javascript
// src/places/components/PlaceItem.js
<div className='place-item__actions'>
  <Button inverse>VIEW ON MAP</Button>
  <Button to={`/places/${props.id}`}>EDIT</Button>
  <Button danger>DELETE</Button>
</div>
```

### Adding a Modal

- Modal.js:
```javascript
// src/shared/components/UIElements/Modal.js
import React from 'react';
import ReactDOM from 'react-dom';
import { CSSTransition } from 'react-transition-group';

import Backdrop from './Backdrop';
import './Modal.css';

const ModalOverlay = props => {
  const content = (
    <div className={`modal ${props.className}`} style={props.style}>
      <header className={`modal__header ${props.headerClass}`}>
        <h2>{props.header}</h2>
        <form
          onSubmit={
            props.onSubmit ? props.onSubmit : event => event.preventDefault()
          }
        >
          <div className={`modal__conent ${props.contentClass}`}>
            {props.children}
          </div>
          <footer className={`modal__footer ${props.footerClass}`}>
            {props.footer}
          </footer>
        </form>
      </header>
    </div>
  );
  return ReactDOM.createPortal(content, document.getElementById('modal-hook'));
};

const Modal = props => {
  return (
    <React.Fragment>
      {props.show && <Backdrop onClick={props.onCancel} />}
      <CSSTransition
        in={props.show}
        mountOnEnter
        unmountOnExit
        timeout={200}
        classNames='modal'
      >
        <ModalOverlay {...props} />
      </CSSTransition>
    </React.Fragment>
  );
};

export default Modal;
```

- In PlaceItem.js
```javascript
src/places/components/PlaceItem.js
import React, { useState } from 'react';

import Card from '../../shared/components/UIElements/Card';
import Button from '../../shared/components/FormElements/Button';
import Modal from '../../shared/components/UIElements/Modal';
import './PlaceItem.css';

const PlaceItem = props => {
  const [showMap, setShowMap] = useState(false);

  const openMapHandler = () => setShowMap(true);

  const closeMapHandler = () => setShowMap(false);

  return (
    <React.Fragment>
      <Modal
        show={showMap}
        onCancel={closeMapHandler}
        header={props.address}
        contentClass='place-item__modal-content'
        footerClass='place-item__modal-actions'
        footer={<Button onClick={closeMapHandler}>CLOSE</Button>}
      >
        <div className="map-container">
          <h2>THE MAP!</h2>
        </div>
      </Modal>
      <li className='place-item'>
        <Card className='place-item__content'>
          <div className='place-item__image'>
            <img src={props.image} alt={props.title} />
          </div>
          <div className='place-item__info'>
            <h2>{props.title}</h2>
            <h3>{props.address}</h3>
            <p>{props.description}</p>
          </div>
          <div className='place-item__actions'>
            <Button inverse onClick={openMapHandler}>VIEW ON MAP</Button>
            <Button to={`/places/${props.id}`}>EDIT</Button>
            <Button danger>DELETE</Button>
          </div>
        </Card>
      </li>
    </React.Fragment>
  );
};

export default PlaceItem;
```

### Rendering a Map with Google Maps

- Will need Google Maps SDK
    - Script in index.html:
        - `<script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyC7ap01wqRWATA7guCbFBvIgpbYZwKQIEo" async defer></script>`
- Also utilize `useRef` & `useEffect`
```javascript
// src/shared/components/UIElements/Map.js
import React, { useRef, useEffect } from 'react';

import './Map.css';

const Map = props => {
  const mapRef = useRef();

  const { center, zoom } = props;

  useEffect(() => {
    const map = new window.google.maps.Map(mapRef.current, {
      center: center,
      zoom: zoom
    });

    new window.google.maps.Marker({ position: center, map: map });
  }, [center, zoom]);

  return (
    <div
      ref={mapRef}
      className={`map ${props.className}`}
      style={props.style}
    ></div>
  );
};

export default Map;
```

### Adding a Custom Form Input Component

- NewPlace.js:
```javascript
// src/places/pages/NewPlace.js
import React from 'react';

import Input from '../../shared/components/FormElements/Input';
import './NewPlace.css';

const NewPlace = () => {
  return (
    <form className='place-form'>
      <Input
        element='input'
        type='text'
        label='Title'
        validators={[]}
        errorText='Please enter a valid title.'
      />
    </form>
  );
};

export default NewPlace;
```

- Input.js:
```javascript
// src/shared/components/FormElements/Input.js
import React from 'react';

import './Input.css';

const Input = props => {
  const element =
    props.element === 'input' ? (
      <input id={props.id} type={props.type} placeholder={props.placeholder} />
    ) : (
      <textarea id={props.id} rows={props.rows || 3} />
    );

  return (
    <div className={`form-control`}>
      <label htmlFor={props.id}>{props.label}</label>
      {element}
    </div>
  );
};

export default Input;
```

### Managing State in the Input Component

- Utilizing `useReducer`
- Input.js:
```javascript
// src/shared/components/FormElements/Input.js
import React, { useReducer } from 'react';

import './Input.css';

const inputReducer = (state, action) => {
  switch (action.type) {
    case 'CHANGE':
      return {
        ...state,
        value: action.val,
        isValid: true
      };
    default:
      return state;
  }
};

const Input = props => {
  const [inputState, dispatch] = useReducer(inputReducer, {
    value: '',
    isValid: false
  });

  const changeHandler = event => {
    dispatch({ type: 'CHANGE', val: event.target.value });
  };

  const element =
    props.element === 'input' ? (
      <input
        id={props.id}
        type={props.type}
        placeholder={props.placeholder}
        onChange={changeHandler}
        value={inputState.value}
      />
    ) : (
      <textarea
        id={props.id}
        rows={props.rows || 3}
        onChange={changeHandler}
        value={inputState.value}
      />
    );

  return (
    <div
      className={`form-control ${!inputState.isValid &&
        'form-control--invalid'}`}
    >
      <label htmlFor={props.id}>{props.label}</label>
      {element}
      {!inputState.isValid && <p>{props.errorText}</p>}
    </div>
  );
};

export default Input;
```

- NewPlace.js:
```javascript
// src/places/pages/NewPlace.js
import React from 'react';

import Input from '../../shared/components/FormElements/Input';
import './NewPlace.css';

const NewPlace = () => {
  return (
    <form className='place-form'>
      <Input
        element='input'
        type='text'
        label='Title'
        validators={[]}
        errorText='Please enter a valid title.'
      />
    </form>
  );
};

export default NewPlace;
```

### Adding Input Validation

- Use provided validators
- in NewPlace.js:
```javascript
<Input
// ...
validators={[VALIDATOR_REQUIRE()]}
>
```

- in Input.js:
```javascript
import React, { useReducer } from 'react';

import { validate } from '../../util/validators';
import './Input.css';

const inputReducer = (state, action) => {
  switch (action.type) {
    case 'CHANGE':
      return {
        ...state,
        value: action.val,
        isValid: validate(action.val, action.validators)
      };
    case 'TOUCH': {
      // keep from erroring immediately on visiting page
      return {
        ...state,
        isTouched: true
      }
    }
    default:
      return state;
  }
};

const Input = props => {
  const [inputState, dispatch] = useReducer(inputReducer, {
    value: '',
    isTouched: false,
    isValid: false
  });

  const changeHandler = event => {
    dispatch({
      type: 'CHANGE',
      val: event.target.value,
      validators: props.validators
    });
  };

  const touchHandler = () => {
    dispatch({
      type: 'TOUCH'
    });
  };

  const element =
    props.element === 'input' ? (
      <input
        id={props.id}
        type={props.type}
        placeholder={props.placeholder}
        onChange={changeHandler}
        onBlur={touchHandler}
        value={inputState.value}
      />
    ) : (
      <textarea
        id={props.id}
        rows={props.rows || 3}
        onChange={changeHandler}
        onBlur={touchHandler}
        value={inputState.value}
      />
    );

  return (
    <div
      className={`form-control ${!inputState.isValid && inputState.isTouched &&
        'form-control--invalid'}`}
    >
      <label htmlFor={props.id}>{props.label}</label>
      {element}
      {!inputState.isValid  && inputState.isTouched && <p>{props.errorText}</p>}
    </div>
  );
};

export default Input;
```

### Sharing Input Values & Adding Multiple Inputs

- Utilizing `useEffect` in Input.js:
```javascript
// in Input component:
  const { id, onInput } = props;
  const { value, isValid } = inputState;

  useEffect(() => {
    onInput(id, value, isValid);
  }, [id, value, isValid, onInput]);
```

- And Utilizing `useCallback`, to avoid infinite loop, in NewPlace.js:
```javascript
import React, { useCallback } from 'react';

import Input from '../../shared/components/FormElements/Input';
import { VALIDATOR_REQUIRE, VALIDATOR_MINLENGTH } from '../../shared/util/validators';
import './NewPlace.css';

const NewPlace = () => {
  const titleInputHandler = useCallback((id, value, isValid) => {}, []);

  const descriptionInputHandler = useCallback((id, value, isValid) => {}, []);

  return (
    <form className='place-form'>
      <Input
        id='title'
        element='input'
        type='text'
        label='Title'
        validators={[VALIDATOR_REQUIRE()]}
        errorText='Please enter a valid title.'
        onInput={titleInputHandler}
      />
      <Input
        id='description'
        element='input'
        label='Description'
        validators={[VALIDATOR_MINLENGTH(5)]}
        errorText='Please enter a valid description (at least 5 characters).'
        onInput={descriptionInputHandler}
      />
    </form>
  );
};

export default NewPlace;
```

### Managing Form-wide State

- NewPlace.js:
```javascript
// src/places/pages/NewPlace.js
import React, { useCallback, useReducer } from 'react';

import Input from '../../shared/components/FormElements/Input';
import Button from '../../shared/components/FormElements/Button';
import {
  VALIDATOR_REQUIRE,
  VALIDATOR_MINLENGTH
} from '../../shared/util/validators';
import './NewPlace.css';

const formReducer = (state, action) => {
  switch (action.type) {
    case 'INPUT_CHANGE':
      let formIsValid = true;
      for (const inputId in state.inputs) {
        if (inputId === action.inputId) {
          formIsValid = formIsValid && action.isValid;
        } else {
          formIsValid = formIsValid && state.inputs[inputId].isValid;
        }
      }
      return {
        ...state,
        inputs: {
          ...state.inputs,
          [action.inputId]: { value: action.value, isValid: action.isValid }
        },
        isValid: formIsValid
      };
    default:
      return state;
  }
};

const NewPlace = () => {
  const [formState, dispatch] = useReducer(formReducer, {
    inputs: {
      title: { value: '', isValid: false },
      description: { value: '', isValid: false }
    },
    isValid: false
  });

  const inputHandler = useCallback((id, value, isValid) => {
    dispatch({
      type: 'INPUT_CHANGE',
      value: value,
      isValid: isValid,
      inputId: id
    });
  }, []);

  return (
    <form className='place-form'>
      <Input
        id='title'
        element='input'
        type='text'
        label='Title'
        validators={[VALIDATOR_REQUIRE()]}
        errorText='Please enter a valid title.'
        onInput={inputHandler}
      />
      <Input
        id='description'
        element='textarea'
        label='Description'
        validators={[VALIDATOR_MINLENGTH(5)]}
        errorText='Please enter a valid description (at least 5 characters).'
        onInput={inputHandler}
      />
      <Button type='submit' disabled={!formState.isValid}>
        ADD PLACE
      </Button>
    </form>
  );
};

export default NewPlace;
```

- Input.js
```javascript
// src/shared/components/FormElements/Input.js
import React, { useCallback, useReducer } from 'react';

import Input from '../../shared/components/FormElements/Input';
import Button from '../../shared/components/FormElements/Button';
import {
  VALIDATOR_REQUIRE,
  VALIDATOR_MINLENGTH
} from '../../shared/util/validators';
import './NewPlace.css';

const formReducer = (state, action) => {
  switch (action.type) {
    case 'INPUT_CHANGE':
      let formIsValid = true;
      for (const inputId in state.inputs) {
        if (inputId === action.inputId) {
          formIsValid = formIsValid && action.isValid;
        } else {
          formIsValid = formIsValid && state.inputs[inputId].isValid;
        }
      }
      return {
        ...state,
        inputs: {
          ...state.inputs,
          [action.inputId]: { value: action.value, isValid: action.isValid }
        },
        isValid: formIsValid
      };
    default:
      return state;
  }
};

const NewPlace = () => {
  const [formState, dispatch] = useReducer(formReducer, {
    inputs: {
      title: { value: '', isValid: false },
      description: { value: '', isValid: false }
    },
    isValid: false
  });

  const inputHandler = useCallback((id, value, isValid) => {
    dispatch({
      type: 'INPUT_CHANGE',
      value: value,
      isValid: isValid,
      inputId: id
    });
  }, []);

  return (
    <form className='place-form'>
      <Input
        id='title'
        element='input'
        type='text'
        label='Title'
        validators={[VALIDATOR_REQUIRE()]}
        errorText='Please enter a valid title.'
        onInput={inputHandler}
      />
      <Input
        id='description'
        element='textarea'
        label='Description'
        validators={[VALIDATOR_MINLENGTH(5)]}
        errorText='Please enter a valid description (at least 5 characters).'
        onInput={inputHandler}
      />
      <Button type='submit' disabled={!formState.isValid}>
        ADD PLACE
      </Button>
    </form>
  );
};

export default NewPlace;
```

### Finishing the "Add Place" Form

- In NewPlace.js, add address input field & submit handler:
```javascript
// src/places/pages/NewPlace.js
// in NewPlace:
  const placeSubmitHandler = event => {
    event.preventDefault();
    console.log(formState.inputs);
  };

  return (
    <form className='place-form' onSubmit={placeSubmitHandler}>
      <Input
        id='title'
        element='input'
        type='text'
        label='Title'
        validators={[VALIDATOR_REQUIRE()]}
        errorText='Please enter a valid title.'
        onInput={inputHandler}
      />
      <Input
        id='description'
        element='textarea'
        label='Description'
        validators={[VALIDATOR_MINLENGTH(5)]}
        errorText='Please enter a valid description (at least 5 characters).'
        onInput={inputHandler}
      />
      <Input
        id='address'
        element='input'
        label='Address'
        validators={[VALIDATOR_REQUIRE()]}
        errorText='Please enter a valid address.'
        onInput={inputHandler}
      />
      <Button type='submit' disabled={!formState.isValid}>
        ADD PLACE
      </Button>
    </form>
  );
```

### Starting Work on the "Update Place" Page

- UpdatePlace.js
```javascript
import React from 'react';
import { useParams } from 'react-router-dom';

import Input from '../../shared/components/FormElements/Input';
import Button from '../../shared/components/FormElements/Button';
import {
  VALIDATOR_REQUIRE,
  VALIDATOR_MINLENGTH
} from '../../shared/util/validators';

const DUMMY_PLACES = [
  // ...
];

const UpdatePlace = () => {
  const placeId = useParams().placeId;

  const identifiedPlace = DUMMY_PLACES.find(p => p.id === placeId);

  if (!identifiedPlace) {
    return (
      <div className='center'>
        <h2>Could not find place!</h2>
      </div>
    );
  }

  return (
    <form>
      <Input
        id='title'
        element='input'
        type='text'
        label='Title'
        validators={[VALIDATOR_REQUIRE]}
        errorText='Please enter a valid title.'
        onInput={() => {}}
        value={identifiedPlace.title}
        valid={true}
      />
      <Input
        id='description'
        element='textarea'
        label='Description'
        validators={[VALIDATOR_MINLENGTH(5)]}
        errorText='Please enter a valid description (min. 5 characters).'
        onInput={() => {}}
        value={identifiedPlace.title}
        valid={true}
      />
      <Button type='submit' disabled={true}>
        UPDATE PLACE
      </Button>
    </form>
  );
};

export default UpdatePlace;
```

- Also add new route in App.js:
    - `<Route path='/places/:placeId'>
        <UpdatePlace />
      </Route>`

### Adjusting the Input Component

- Update `useReducer()` in Input.js:
```javascript
const [inputState, dispatch] = useReducer(inputReducer, {
  value: props.value || '',
  isTouched: false,
  isValid: props.valid || false
});
```

- Rename NewPlace.css to PlaceForm.css, update imports and add class name (place-form) to form in UpdatePlace.js

### Creating a Custom Form Hook (useForm)

- Function that can share stateful logic.
- Starts with 'use' by convention (useEffect)
- Can use default hooks *inside* a custom hook
- Will re-render component if you change something that effects the state
- First, restructure NewPlace.js into form-hook.js, fix UpdatePlace.js

- NewPlace.js
```javascript
// src/places/pages/NewPlace.js
import React from 'react';

import Input from '../../shared/components/FormElements/Input';
import Button from '../../shared/components/FormElements/Button';
import {
  VALIDATOR_REQUIRE,
  VALIDATOR_MINLENGTH
} from '../../shared/util/validators';
import './PlaceForm.css';
import { useForm } from '../../shared/hooks/form-hook';

const NewPlace = () => {
  const [formState, inputHandler] = useForm(
    {
      title: {
        value: '',
        isValid: false
      },
      description: {
        value: '',
        isValid: false
      },
      address: {
        value: '',
        isValid: false
      }
    },
    false
  );

  const placeSubmitHandler = event => {
    event.preventDefault();
    console.log(formState.inputs);
  };

  return (
    <form className='place-form' onSubmit={placeSubmitHandler}>
      <Input
        id='title'
        element='input'
        type='text'
        label='Title'
        validators={[VALIDATOR_REQUIRE()]}
        errorText='Please enter a valid title.'
        onInput={inputHandler}
      />
      <Input
        id='description'
        element='textarea'
        label='Description'
        validators={[VALIDATOR_MINLENGTH(5)]}
        errorText='Please enter a valid description (at least 5 characters).'
        onInput={inputHandler}
      />
      <Input
        id='address'
        element='input'
        label='Address'
        validators={[VALIDATOR_REQUIRE()]}
        errorText='Please enter a valid address.'
        onInput={inputHandler}
      />
      <Button type='submit' disabled={!formState.isValid}>
        ADD PLACE
      </Button>
    </form>
  );
};

export default NewPlace;
```

- form-hook.js:
```javascript
// src/shared/hooks/form-hook.js
import { useCallback, useReducer } from 'react';

const formReducer = (state, action) => {
  switch (action.type) {
    case 'INPUT_CHANGE':
      let formIsValid = true;
      for (const inputId in state.inputs) {
        if (inputId === action.inputId) {
          formIsValid = formIsValid && action.isValid;
        } else {
          formIsValid = formIsValid && state.inputs[inputId].isValid;
        }
      }
      return {
        ...state,
        inputs: {
          ...state.inputs,
          [action.inputId]: { value: action.value, isValid: action.isValid }
        },
        isValid: formIsValid
      };
    default:
      return state;
  }
};

export const useForm = (initialInputs, initialFormValidity) => {
  const [formState, dispatch] = useReducer(formReducer, {
    inputs: initialInputs,
    isValid: initialFormValidity
  });

  const inputHandler = useCallback((id, value, isValid) => {
    dispatch({
      type: 'INPUT_CHANGE',
      value: value,
      isValid: isValid,
      inputId: id
    });
  }, []);

  return [formState, inputHandler];
};
```

- Also UpdatePlace.js:
```javascript
// src/places/pages/UpdatePlace.js
import React from 'react';
import { useParams } from 'react-router-dom';

import Input from '../../shared/components/FormElements/Input';
import Button from '../../shared/components/FormElements/Button';
import {
  VALIDATOR_REQUIRE,
  VALIDATOR_MINLENGTH
} from '../../shared/util/validators';
import { useForm } from '../../shared/hooks/form-hook';

import './PlaceForm.css';

const DUMMY_PLACES = [
  {
    id: 'p1',
    title: 'Empire State Building',
    description: 'One of the most famous sky scrapers in the world!',
    imageUrl:
      'https://upload.wikimedia.org/wikipedia/commons/1/10/Empire_State_Building_%28aerial_view%29.jpg',
    address: '20 West 34th Street, New York City, NY 10118-0114',
    location: {
      lat: 40.7484405,
      lng: -73.9878584
    },
    creator: 'u1'
  },
  {
    id: 'p2',
    title: 'Empire State Building',
    description: 'One of the most famous sky scrapers in the world!',
    imageUrl:
      'https://upload.wikimedia.org/wikipedia/commons/1/10/Empire_State_Building_%28aerial_view%29.jpg',
    address: '20 West 34th Street, New York City, NY 10118-0114',
    location: {
      lat: 40.7484405,
      lng: -73.9878584
    },
    creator: 'u2'
  }
];

const UpdatePlace = () => {
  const placeId = useParams().placeId;

  const identifiedPlace = DUMMY_PLACES.find(p => p.id === placeId);
  
  const [formState, inputHandler] = useForm({
    title: {
      value: identifiedPlace.title,
      isValid: true
    },
    description: {
      value: identifiedPlace.description,
      isValid: true
    }
  }, true);

  const placeUpdateSubmitHandler = event => {
    event.preventDefault();
    console.log(formState.inputs);
  };

  if (!identifiedPlace) {
    return (
      <div className='center'>
        <h2>Could not find place!</h2>
      </div>
    );
  }

  return (
    <form className='place-form' onSubmit={placeUpdateSubmitHandler}>
      <Input
        id='title'
        element='input'
        type='text'
        label='Title'
        validators={[VALIDATOR_REQUIRE()]}
        errorText='Please enter a valid title.'
        onInput={inputHandler}
        initialValue={formState.inputs.title.value}
        initialValid={formState.inputs.title.isValid}
      />
      <Input
        id='description'
        element='textarea'
        label='Description'
        validators={[VALIDATOR_MINLENGTH(5)]}
        errorText='Please enter a valid description (min. 5 characters).'
        onInput={inputHandler}
        initialValue={formState.inputs.description.value}
        initialValid={formState.inputs.description.isValid}
      />
      <Button type='submit' disabled={!formState.isValid}>
        UPDATE PLACE
      </Button>
    </form>
  );
};

export default UpdatePlace;
```

### Adjusting the Form Hook

- In `useForm()`
```javascript
// src/shared/hooks/form-hook.js
// ...
  const setFormData = useCallback((inputData, formValidity) => {
    dispatch({
      type: 'SET_DATA',
      inputs: inputData,
      formIsValid: formValidity
    });
  }, []);

  return [formState, inputHandler, setFormData];
```

- Use temporary fix in UpdatePlace.js:
```javascript
// src/places/pages/UpdatePlace.js
// imports...
const DUMMY_PLACES = [/* ... */];

const UpdatePlace = () => {
  const [isLoading, setIsLoading] = useState(true);
  const placeId = useParams().placeId;

  const [formState, inputHandler, setFormData] = useForm({
    title: {
      value: '',
      isValid: false
    },
    description: {
      value: '',
      isValid: false
    }
  }, true);
  
  const identifiedPlace = DUMMY_PLACES.find(p => p.id === placeId);

  useEffect(() => {
    setFormData({
      title: {
        value: identifiedPlace.title,
        isValid: true
      },
      description: {
        value: identifiedPlace.description,
        isValid: true
      }
    }, true);
    setIsLoading(false);
  }, [setFormData, identifiedPlace]);

  const placeUpdateSubmitHandler = event => {
    event.preventDefault();
    console.log(formState.inputs);
  };

  if (!identifiedPlace) {
    return (
      <div className='center'>
        <h2>Could not find place!</h2>
      </div>
    );
  }

  if (isLoading) {
    return (
      <div className='center'>
        <h2>Loading...</h2>
      </div>
    );
  }

  return (/* unchanged form */);
};

export default UpdatePlace;
```

### Fixing Minor Issues

- Fix up Share Button on PlaceList.js:
```javascript
// src/places/components/PlaceList.js
<Button to='/places/new'>Share Place</Button>
```

- Error on attempted navigate to place that doesn't exist
```javascript
// src/places/components/PlaceList.js
// putting setFormData in if check:
if (identifiedPlace) {
  setFormData({
    title: {
      value: identifiedPlace.title,
      isValid: true
    },
    description: {
      value: identifiedPlace.description,
      isValid: true
    }
  }, true);
}
```

- Wrap "Could not find place!" in Card for better styling:
```javascript
// src/places/pages/UpdatePlace.js
if (!identifiedPlace) {
  return (
    <div className='center'>
      <Card>
        <h2>Could not find place!</h2>
      </Card>
    </div>
  );
}
```

### Showing a Deletion Warning

- PlaceItem.js:
```javascript
// src/places/components/PlaceItem.js
import React, { useState } from 'react';

import Card from '../../shared/components/UIElements/Card';
import Button from '../../shared/components/FormElements/Button';
import Modal from '../../shared/components/UIElements/Modal';
import Map from '../../shared/components/UIElements/Map';
import './PlaceItem.css';

const PlaceItem = props => {
  const [showMap, setShowMap] = useState(false);
  const [showConfirmModal, setShowConfirmModal] = useState(false);

  const openMapHandler = () => setShowMap(true);

  const closeMapHandler = () => setShowMap(false);

  const showDeleteWarningHandler = () => {
    setShowConfirmModal(true);
  };

  const cancelDeleteHandler = () => {
    setShowConfirmModal(false);
  };

  const confirmDeleteHandler = () => {
    setShowConfirmModal(false);
    console.log('DELETING...');
  };

  return (
    <React.Fragment>
      <Modal
        show={showMap}
        onCancel={closeMapHandler}
        header={props.address}
        contentClass='place-item__modal-content'
        footerClass='place-item__modal-actions'
        footer={<Button onClick={closeMapHandler}>CLOSE</Button>}
      >
        <div className='map-container'>
          <Map center={props.coordinates} zoom={16} />
        </div>
      </Modal>
      <Modal
        show={showConfirmModal}
        onCancel={cancelDeleteHandler}
        header='Are you sure?'
        footerClass='place-item__modal-actions'
        footer={
          <React.Fragment>
            <Button inverse onClick={cancelDeleteHandler}>CANCEL</Button>
            <Button danger onClick={confirmDeleteHandler}>DELETE</Button>
          </React.Fragment>
        }
      >
        <p>
          Do you want to proceed and delete this place? Please note this action
          cannnot be undone.
        </p>
      </Modal>
      <li className='place-item'>
        <Card className='place-item__content'>
          <div className='place-item__image'>
            <img src={props.image} alt={props.title} />
          </div>
          <div className='place-item__info'>
            <h2>{props.title}</h2>
            <h3>{props.address}</h3>
            <p>{props.description}</p>
          </div>
          <div className='place-item__actions'>
            <Button inverse onClick={openMapHandler}>
              VIEW ON MAP
            </Button>
            <Button to={`/places/${props.id}`}>EDIT</Button>
            <Button danger onClick={showDeleteWarningHandler}>DELETE</Button>
          </div>
        </Card>
      </li>
    </React.Fragment>
  );
};

export default PlaceItem;
```

### Adding an "Auth" Page & Login Form

- Auth.js:
```javascript
import React from 'react';
import Button from '../../shared/components/FormElements/Button';
import Input from '../../shared/components/FormElements/Input';
import Card from '../../shared/components/UIElements/Card';
import {
  VALIDATOR_EMAIL,
  VALIDATOR_MINLENGTH
} from '../../shared/util/validators';
import { useForm } from '../../shared/hooks/form-hook';

import './Auth.css';

const Auth = () => {
  const [formState, inputHandler] = useForm(
    {
      email: {
        value: '',
        isValid: false
      },
      password: {
        value: '',
        isValid: false
      }
    },
    false
  );

  const authSubmitHandler = event => {
    event.preventDefault();
    console.log(formState.inputs);
  };

  return (
    <Card className='authentication'>
      <h2>Login Required</h2>
      <hr />
      <form onSubmit={authSubmitHandler}>
        <Input
          element='input'
          id='email'
          type='email'
          label='E-Mail'
          validators={[VALIDATOR_EMAIL()]}
          errorText='Please enter a valid email address.'
          onInput={inputHandler}
        />
        <Input
          element='input'
          id='password'
          type='password'
          label='Password'
          validators={[VALIDATOR_MINLENGTH(7)]}
          errorText='Please enter a valid password (min. 5 characters).'
          onInput={inputHandler}
        />
        <Button type='submit' disabled={!formState.isValid}>
          LOGIN
        </Button>
      </form>
    </Card>
  );
};

export default Auth;
```

### Adding Signup + "Switch Mode" Button

- Auth.js updated with "Switch Mode"
```javascript
// src/user/pages/Auth.js
// imports...
const Auth = () => {
  const [isLoginMode, setIsLoginMode] = useState(true);
  // need setFormData now
  const [formState, inputHandler, setFormData] = useForm(/* ... */);

  const switchModeHandler = () => {
    if (!isLoginMode) {
      setFormData(
        {
          ...formState.inputs,
          name: undefined
        },
        formState.inputs.email.isValid && formState.inputs.password.isValid
      );
    } else {
      setFormData(
        {
          ...formState.inputs,
          name: {
            value: '',
            isValid: false
          }
        },
        false
      );
    }
    setIsLoginMode(prevMode => !prevMode);
  };

  return (
    <Card className='authentication'>
      <h2>Login Required</h2>
      <hr />
      <form onSubmit={authSubmitHandler}>
        {!isLoginMode && (
          <Input
            element='input'
            id='name'
            type='type'
            label='Your Name'
            validators={[VALIDATOR_REQUIRE()]}
            errorText='Please enter a name.'
            onInput={inputHandler}
          />
        )}
        {/* email & password inputs*/}
        <Button type='submit' disabled={!formState.isValid}>
          {isLoginMode ? 'LOGIN' : 'SIGNUP'}
        </Button>
      </form>
      <Button inverse onClick={switchModeHandler}>
        SWITCH TO {isLoginMode ? 'SIGNUP' : 'LOGIN'}
      </Button>
    </Card>
}
```

- Add additional condition in form-hook.js `for` loop:
```javascript
// src/shared/hooks/form-hook.js/formReducer()
if (!state.inputs[inputId]) {
  continue;
}
```

### Adding Auth Context for App-wide State Management

- Context for passing data between components without having to use props
```javascript
// src/shared/context/auth-context.js
import { createContext } from 'react';

export const AuthContext = createContext({
  isLoggedIn: false,
  login: () => {},
  logout: () => {}
});
```

- Wrap where context should be available, in this case, the entire App:
```javascript
// src/App.js
// ...imports
import { AuthContext } from './shared/context/auth-context';

const App = () => {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  const login = useCallback(() => {
    setIsLoggedIn(true);
  }, []);

  const logout = useCallback(() => {
    setIsLoggedIn(false);
  }, []);

  return (
    <AuthContext.Provider
      value={{ isLoggedIn: isLoggedIn, login: login, logout: logout }}
    >
      <Router>
        <MainNavigation />
        <main>
          <Switch>
            <Route path='/' exact>
              <Users />
            </Route>
            <Route path='/auth' exact>
              <Auth />
            </Route>
            <Route path='/:userId/places'>
              <UserPlaces />
            </Route>
            <Route path='/places/new' exact>
              <NewPlace />
            </Route>
            <Route path='/places/:placeId'>
              <UpdatePlace />
            </Route>
            <Redirect to='/' />
          </Switch>
        </main>
      </Router>
    </AuthContext.Provider>
  );
};

export default App;
```

### Listening to Context Changes

- Use in NavLinks.js:
```javascript
// src/shared/Navvigation/NavLinks.js
import React, { useContext } from 'react';
import { NavLink } from 'react-router-dom';

import { AuthContext } from '../../context/auth-context';

import './NavLinks.css';

const NavLinks = props => {
  const auth = useContext(AuthContext);

  return (
    <ul className='nav-links'>
      <li>
        <NavLink to='/' exact>
          ALL USERS
        </NavLink>
      </li>
      {auth.isLoggedIn && (
        <li>
          <NavLink to='/u1/places'>MY PLACES</NavLink>
        </li>
      )}
      {auth.isLoggedIn && (
        <li>
          <NavLink to='/places/new'>ADD PLACE</NavLink>
        </li>
      )}
      {!auth.isLoggedIn && (
        <li>
          <NavLink to='/auth'>AUTHENTICATE</NavLink>
        </li>
      )}
    </ul>
  );
};

export default NavLinks;
```

### Adding Authenticated & Unauthenticated Routes

- In Auth.js:
```javascript
// src/user/pages/Auth.js
// ...imports
import { AuthContext } from '../../shared/context/auth-context';

const Auth = () => {
  const auth = useContext(AuthContext);
  // ...
  const authSubmitHandler = event => {
    event.preventDefault();
    console.log(formState.inputs);
    auth.login();
  };
};
```

- Refactor App.js & Routes
```javascript
// src/App.js
// ...imports
const App = () => {
  // ...
  let routes;

  if (isLoggedIn) {
    routes = (
      <Switch>
        <Route path='/' exact>
          <Users />
        </Route>
        <Route path='/:userId/places'>
          <UserPlaces />
        </Route>
        <Route path='/places/new' exact>
          <NewPlace />
        </Route>
        <Route path='/places/:placeId'>
          <UpdatePlace />
        </Route>
        <Redirect to='/' />
      </Switch>
    );
  } else {
    routes = (
      <Switch>
        <Route path='/' exact>
          <Users />
        </Route>
        <Route path='/:userId/places'>
          <UserPlaces />
        </Route>
        <Route path='/auth' exact>
          <Auth />
        </Route>
        <Redirect to='/auth' />
      </Switch>
    );
  }

  return (
    <AuthContext.Provider
      value={{ isLoggedIn: isLoggedIn, login: login, logout: logout }}
    >
      <Router>
        <MainNavigation />
        <main>{routes}</main>
      </Router>
    </AuthContext.Provider>
  );
};
```

### More Auth Context Usage

- Hide Edit & Delete buttons on PlaceItem:
```javascript
// src/places/components/PlaceItem.js
// after view on map place-item action
{auth.isLoggedIn && (
  <Button to={`/places/${props.id}`}>EDIT</Button>
)}
{auth.isLoggedIn && (
  <Button danger onClick={showDeleteWarningHandler}>
    DELETE
  </Button>
)}
```

- Add Logout button to NavLinks:
```javascript 
// src/shared/components/Navigation/NavLinks.js
// after auth button
{auth.isLoggedIn && (
  <li>
    <button onClick={auth.logout}>LOGOUT</button>
  </li>
)}
```

## 7. Node.js and Express.js - Building our REST API

### Intro

1. Set Up Node.js + Express App
2. Add Routes
3. Add Controllers & (Dummy) Logic
4. Add User Input Validation

### Setting up our Project

```
mkdir backend
cd ./backend
npm init
npm i --save body-parser express
npm i --save-dev nodemon
touch app.js
```

### Implementing Basic Routing

```
mkdir routes
cd ./routes
touch place-routes.js
touch users-routes.js
```

```javascript
// backend/routes/places-routes.js
const express = require('express');

const router = express.Router();

router.get('/', (req, res, next) => {
  console.log('GET Request in PLaces');
  res.json({ message: 'It works!' });
});

module.exports = router;
```

```javascript
// backend/app.js
const express = require('express');
const bodyParser = require('body-parser');

const placesRoutes = require('./routes/places-routes');

const app = express();

app.use(placesRoutes);

app.listen(5000);
```

### Adding Place-Specific Routes

- Limit filter to `placesRoutes`
    - `app.use('/api/places', placesRoutes); // => /api/places/ ...`
- Add in Dummy data to places-routes
    - `const DUMMY_PLACES + [ /*...*/]`
- Modify GET in places-routes:
```javascript
// backend/routes/places-routes.js
router.get('/:pid', (req, res, next) => {
  const placeId = req.params.pid // { pid: 'p1' }
  const place = DUMMY_PLACES.find(p => {
      return p.id === placeId;
  });
  res.json({ place });
});
```

### Getting a Place by User ID

- Order matters in these routes!
```javascript
// backend/routes/places-routes
router.get('/user/:uid', (req, res, next) => {
  const userId = req.params.uid;

  const place = DUMMY_PLACES.find(p => {
    return p.creator === userId;
  });
  res.json({ place });
});
```

### Handling Errors

- Catching in place-routes.js:
```javascript
// backend/routes/places-routes.js
router.get('/:pid', (req, res, next) => {
  const placeId = req.params.pid; // { pid: 'p1' }
  const place = DUMMY_PLACES.find(p => {
    return p.id === placeId;
  });

  if (!place) {
    const error = new Error('Could not find a place for the provided id.');
    error.code = 404;
    throw error; // throw in sync code
  }

  res.json({ place });
});

router.get('/user/:uid', (req, res, next) => {
  const userId = req.params.uid;

  const place = DUMMY_PLACES.find(p => {
    return p.creator === userId;
  });

  if (!place) {
    const error = new Error('Could not find a place for the provided id.');
    error.code = 404;
    return next(error); // next in async code. (its sync here, but as an example)
  }

  res.json({ place });
});
```

- Modifying app.js:
```javascript
// backend/app.js
// after /api/places route
app.use((error, req, res, next) => {
  if (res.headerSent) {
    return next(error);
  }
  res
    .status(error.code || 500)
    .json({ message: error.message || 'An unknown error ocurred!' });
});
```

### Adding our own Error Model

- HttpError class:
```javascript
// backend/models/http-error.js
class HttpError extends Error {
    constructor(message, errorCode) {
        super(message); // Add a "message" property
        this.code = errorCode; // Adds a "code" property
    }
}

module.exports = HttpError;
```

- Modify places-routes.js:
    - `const error = new HttpError('Could not find a place for the provided id.', 404);`

### Adding Controllers

- Model View Controller concept (without the views here)
    - `mkdir controllers`
- Refactor:

```javascript
// backend/controllers/places-controller/js
const HttpError = require('../models/http-error');

const DUMMY_PLACES = [
  // ...
];

const getPlaceById = (req, res, next) => {
  // ...
};

const getPlaceByUserId = (req, res, next) => {
  // ..
};

exports.getPlaceById = getPlaceById;
exports.getPlaceByUserId = getPlaceByUserId;
```

```javascript
// backend/routes/places-routes.js
const express = require('express');

const placesControllers = require('../controllers/places-controllers');

const router = express.Router();

router.get('/:pid', placesControllers.getPlaceById);

router.get('/user/:uid', placesControllers.getPlaceByUserId);

module.exports = router;
```

### Adding a POST Route and Using Postman

- Add new route in places-routes.js:
    - `router.post('/', placesControllers.createPlace);`
- Add in bodyParser to App.js:
    - `app.use(bodyParser.json());`
- createPlace route:
```javascript
const createPlace = (req, res, next) => {
  const { title, description, coordinates, address, creator } = req.body;

  const createdPlace = {
    title,
    description,
    location: coordinates,
    address,
    creator
  };

  DUMMY_PLACES.push(createdPlace); // unshift (createdPlaced)

  res.status(201).json({ place: createdPlace });
};

exports.createPlace = createPlace;
```

- Use Postman to test
- Use uuid package to generate unique ids
    - `npm i --save uuid`
    - import into controller
        - `const uuid = require('uuid/v4');`
        - use in id field on createPlace
            - `id: uuid(),`

### Handling Errors for Unsupported Routes

- Add a catch-all route *after* other routes to handle unwanted routes
```javascript
app.use((req, res, next) => {
  const error = new HttpError('Could not find this route.', 404);
  throw error;
});
```

### Adding Patch Routes to Update Places

- Add the new route for updating places:
    - `router.patch('/:pid', placesControllers.updatePlace);`
- Create the route in controllers:
```javascript
const updatePlace = (req, res, next) => {
  const { title, description } = req.body;
  const placeId = req.params.pid;

  const updatedPlace = { ...DUMMY_PLACES.find(p => p.id === placeId) };
  const placeIndex = DUMMY_PLACES.findIndex(p => p.id === placeId);
  updatedPlace.title = title;
  updatedPlace.description = description;

  DUMMY_PLACES[placeIndex] = updatedPlace;

  res.status(200).json({ place: updatedPlace });
};

exports.updatePlace = updatePlace;
```

### Deleting Places

- Add the new route for deleting places:
    - `router.delete('/:pid', placesControllers.deletePlace);`
- Create the route in controllers:
```javascript
const deletePlace = (req, res, next) => {
  const placeId = req.params.pid;
  // need to declare DUMMY_PLACES with let instead of const!
  DUMMY_PLACES = DUMMY_PLACES.filter(p => p.id !== placeId);
  res.status(200).json({message: 'Deleted place.'});
};

exports.deletePlace = deletePlace;
```

### Finalizing the "Get Places by User ID" Resource

- Fix getPlaceByUserId to get multiple places for user
- Change to use `filter` instead of `find`

### Setting Up the User Routes (Signup, Login, Get Users)

- User Controllers:
```javascript
// backend/controllers/users-controllers.js
const uuid = require('uuid/v4');

const HttpError = require('../models/http-error');

const DUMMY_USERS = [
  {
    id: 'u1',
    name: 'John',
    email: 'john@test.com',
    password: 'password'
  }
];

const getUsers = (req, res, next) => {
  res.json({ users: DUMMY_USERS });
};

const signup = (req, res, next) => {
  const { name, email, password } = req.body;

  const hasUser = DUMMY_USERS.find(u => u.email === email);
  if (hasUser) {
    throw new HttpError('Could not create user, email already exists!', 422);
  }

  const createdUser = {
    id: uuid(),
    name,
    email,
    password
  };

  DUMMY_USERS.push(createdUser);

  res.status(201).json({ user: createdUser });
};

const login = (req, res, next) => {
  const { email, password } = req.body;

  const identifiedUser = DUMMY_USERS.find(u => u.email === email);

  if (!identifiedUser || identifiedUser.password !== password) {
    throw new HttpError(
      'Could not identify user, credentials seem to be wrong.',
      401
    );
  }

  res.json({ message: 'Logged In!' });
};

exports.getUsers = getUsers;
exports.signup = signup;
exports.login = login;
```

- Users Routes:
```javascript
// backend/routes/users-routes.js
const express = require('express');

const usersControllers = require('../controllers/users-controllers');

const router = express.Router();

router.get('/', usersControllers.getUsers);

router.post('/signup', usersControllers.signup);

router.post('/login', usersControllers.login);

module.exports = router;
```

- Add to app.js:
    - `app.use('/api/users', usersRoutes); // => /api/users/ ...` 

### Validating API Input (Request Bodies)

- Install validator package:
`npm i --save express-validator`

- Can use multiple middlewares on routes

- For Create Place *route*:
```javascript
// ...imports
const { check } = require('express-validator');
// ...
router.post(
  '/',
  [
    check('title')
      .not()
      .isEmpty(),
    check('description').isLength({ min: 5 }),
    check('address')
      .not()
      .isEmpty()
  ],
  placesControllers.createPlace
);
// ...
```

- For Create Place *controller*:
```javascript
// ...imports
const { validationResult } = require('express-validator');
// ...
const createPlace = (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    console.log(errors);
    throw new HttpError('Invalid inputs passed, please check your data.', 422);
  }
  // ...
}
// ...
```

### Validating Patch Requests and User Routes

- For Update Place *route*:
```javascript
router.patch(
  '/:pid',
  [
    check('title')
      .not()
      .isEmpty(),
    check('description').isLength({ min: 5 })
  ],
  placesControllers.updatePlace
);
```

- For Update Place *controller*, can use same logic as in createPlace above

- For User Signup *route*
```javascript
// ...imports
const { check } = require('express-validator');
// ...
router.post(
  '/signup',
  [
    check('name')
      .not()
      .isEmpty(),
    check('email')
      .normalizeEmail() // Test@test.com => test@test.com
      .isEmail(),
    check('password').isLength({ min: 6 })
  ],
  usersControllers.signup
);
// ...
```
- For User Signup *controller*, use similar logic as update and create place

### Using Google's Geocoding API to Convert an Address Into Coordinates

- Use third party package to send request to google
    - `npm i --save axios`
- location.js:
```javascript
// backend/util/location.js
const axios = require('axios');

const HttpError = require('../models/http-error');

const API_KEY = 'AIzaSyC7ap01wqRWATA7guCbFBvIgpbYZwKQIEo';

const getCoordsforAddress = async address => {
  const response = await axios.get(
    `https://maps.googleapis.com/maps/api/geocode/json?address=${encodeURIComponent(
      address
    )}&key=${API_KEY}`
  );

  const data = response.data;

  if (!data || data.status === 'ZERO_RESULTS') {
    const error = new HttpError(
      'Could not find location for the specified address.',
      422
    );
    throw error;
  }

  const coordinates = data.results[0].geometry.location;

  return coordinates;
};

module.exports = getCoordsforAddress;
```

- Modify create Place in controller:
```javascript
// backend/controllers/places-controllers.js
// ...
const createPlace = async (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return next(
      new HttpError('Invalid inputs passed, please check your data.', 422)
    );
  }

  const { title, description, address, creator } = req.body;

  let coordinates;
  try {
    coordinates = await getCoordsForAddress(address);
  } catch (error) {
    return next(error);
  }
  // ...
}
// ...
```

## 9. Connecting the Backend to the Database MongoDB and Mongoose

1. Defining our Mongoose Models
2. CRUD Operations for Place and User
3. Connecting Places and Users

### Installing Mongoose and Connecting our Backend to MongoDB

- `npm i --save mongoose`
```javascript
// backend/app.js
// ...
mongoose
  .connect('mongodb://localhost:27017/react-training')
  .then(() => {
    console.log('Connected to database!');
    app.listen(5000);
  })
  .catch(err => {
    console.log(err);
  });
```

### Creating the Place Schema and Model

- place.js:
```javascript
// backend/models/place.js
const mongoose = require('mongoose');

const Schema = mongoose.Schema;

const placeSchema = new Schema({
    title: { type: String, required: true },
    description: { type: String, required: true },
    image: { type: String, required: true },
    address: { type: String, required: true },
    location: {
        lat: { type: Number, required: true },
        lng: { type: Number, required: true },
    },
    creator: { type: String, required: true },
});

module.exports = mongoose.model('Place', placeSchema);
```

### Creating and Storing Documents in the Database

- Modifying create place controller
```javascript
// backend/controllers/places-controllers
const createPlace = async (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return next(
      new HttpError('Invalid inputs passed, please check your data.', 422)
    );
  }

  const { title, description, address, creator } = req.body;

  let coordinates;
  try {
    coordinates = await getCoordsForAddress(address);
  } catch (error) {
    return next(error);
  }

  const createdPlace = new Place({
    title,
    description,
    address,
    location: coordinates,
    image:
      'https://upload.wikimedia.org/wikipedia/commons/1/10/Empire_State_Building_%28aerial_view%29.jpg',
    creator
  });

  try {
    await createdPlace.save();
  } catch (err) {
    const error = new HttpError(
      'Creating Place failed, please try again.',
      500
    );
    return next(error);
  }

  res.status(201).json({ place: createdPlace });
};
```

### Getting Places by the PlaceID

- Get place by ID controller:
```javascript
// backend/controllers/places-controllers.js
// ...
const getPlaceById = async (req, res, next) => {
  const placeId = req.params.pid; // { pid: 'p1' }

  let place;
  try {
    place = await Place.findById(placeId);
  } catch (err) {
    const error = new HttpError(
      'Something went wrong, could not find a place',
      500
    );
    return next(error);
  }

  if (!place) {
    const error = new HttpError(
      'Could not find a place for the provided id.',
      404
    );
    return next(error);
  }

  res.json({ place: place.toObject({ getters: true }) });
};
// ...
```

### Getting Places by the UserID

- Get place by User ID controller:
```javascript
// backend/controllers/places-controllers.js
// ...
const getPlacesByUserId = async (req, res, next) => {
  const userId = req.params.uid;

  let places;
  try {
    places = await Place.find({ creator: userId });
  } catch (err) {
    const error = new HttpError(
      'Fetching places failed, please try again later.',
      500
    );
    return next(error);
  }

  if (!places || places.length === 0) {
    const error = new HttpError(
      'Could not find a places for the provided user id.',
      404
    );
    return next(error);
  }

  res.json({ places: places.map(place => place.toObject({ getters: true })) });
};
// ...
```

### Updating Places

- Update place controller:
```javascript
// backend/controllers/places-controllers.js
// ...
const updatePlace = async (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    throw new HttpError('Invalid inputs passed, please check your data.', 422);
  }

  const { title, description } = req.body;
  const placeId = req.params.pid;

  let place;
  try {
    place = await Place.findById(placeId);
  } catch (err) {
    const error = new HttpError(
      'Something went wrong, could not update place',
      500
    );
    return next(error);
  }

  place.title = title;
  place.description = description;

  try {
    await place.save();
  } catch (err) {
    const error = new HttpError(
      'Something went wrong, could not update place.',
      500
    );
    return next(error);
  }

  res.status(200).json({ place: place.toObject({ getters: true }) });
};
// ...
```

### Deleting Places

- Delete Places controller:
```javascript
// backend/controllers/places-controllers
// ...
const deletePlace = async (req, res, next) => {
  const placeId = req.params.pid;

  let place;
  try {
    place = await Place.findById(placeId);
  } catch (err) {
    const error = new HttpError(
      'Something went wrong, could not delete place.',
      500
    );
    return next(error);
  }

  try {
    place.remove();
  } catch (err) {
    const error = new HttpError(
      'Something went wrong, could not delete place.',
      500
    );
    return next(error);
  }

  res.status(200).json({ message: 'Deleted place.' });
};
// ...
```

### How Users and Places are Related

- One place belongs to exactly one user
- One user can create multiple places

### Creating the User Model

- Will need mongoose unique validator:
    - `npm i --save mongoose-unique-validator`
    - Since unique property doesn't make sure another already exists, instead just makes easily to reference with an index
```javascript
// backend/models/user.js
const mongoose = require('mongoose');
const uniqueValidator = require('mongoose-unique-validator');

const Schema = mongoose.Schema;

const userSchema = new Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true, minlength: 6 },
  image: { type: String, required: true },
  places: { type: String, required: true }
});

userSchema.plugin(uniqueValidator);

module.exports = mongoose.model('User', userSchema);
```

### Using the User Model for Signup

- Modifying user controllers:
```javascript
const signup = async (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return next(
      new HttpError('Invalid inputs passed, please check your data.', 422)
    );
  }

  const { name, email, password, places } = req.body;

  let existingUser;
  try {
    existingUser = await User.findOne({ email: email });
  } catch (err) {
    const error = new HttpError(
      'Signing up failed, please try again later.',
      500
    );
    return next(error);
  }

  if (existingUser) {
    const error = new HttpError(
      'User exists already, please login instead.',
      422
    );
    return next(error);
  }

  const createdUser = new User({
    name,
    email,
    image:
      'https://www.publicdomainpictures.net/pictures/10000/velka/brown-eyed-susan-flower-closeup-23441281369772mHdG.jpg',
    password,
    places
  });

  try {
    await createdUser.save();
  } catch (err) {
    console.log(err.message);
    const error = new HttpError(
      'Saving User failed, please try again.',
      500
    );
    return next(error);
  }

  res.status(201).json({ user: createdUser.toObject({ getters: true }) });
};
```

### Adding the User Login

- Modifying users controller
```javascript
const login = async (req, res, next) => {
  const { email, password } = req.body;

  let existingUser;

  try {
    existingUser = await User.findOne({ email });
  } catch (err) {
    const error = new HttpError(
      'Logging in failed, please try again later.',
      500
    );
    return next(error);
  }

  // dummy logic currently for authentication:
  if (!existingUser || existingUser.password !== password) {
    const error = new HttpError(
      'Invalid credentials, could not log you in.', 401
    );

    return next(error);
  }

  res.json({ message: 'Logged In!' });
};
```

### Getting Users

- Modifying users controller
```javascript
const getUsers = async (req, res, next) => {
  let users;
  try {
    users = await User.find({}, '-password');
  } catch (err) {
    const error = new HttpError(
      'Fetching users failed, please try again later.',
      500
    );
    return next(error);
  }
  res.json({ users: users.map(user => user.toObject({ getters: true })) });
};
```

### Adding the Relation between Places and Users

- *One user can create multiple places* **AND** *One place belongs to exactly one user.*

- Modify Place Schema:
`{ type: mongoose.Types.ObjectId, required: true, ref: 'User' }`

- Modify User schema:
`places: [{ type: mongoose.Types.ObjectId, required: true, ref: 'Place' }]`

- Modify users controller
```javascript
// ...
const signup = async (req, res, next) => {
  // ...
  const { name, email, password } = req.body;
  // ...
  const createdUser = new User({
    name,
    email,
    image:
      'https://www.publicdomainpictures.net/pictures/10000/velka/brown-eyed-susan-flower-closeup-23441281369772mHdG.jpg',
    password,
    places: []
  });
  // ...
}
// ...
```

### Creating Places and Adding it to a User

- Using session and transaction
- Modifying Places controller:
```javascript
const createPlace = async (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return next(
      new HttpError('Invalid inputs passed, please check your data.', 422)
    );
  }

  const { title, description, address, creator } = req.body;

  let coordinates;
  try {
    coordinates = await getCoordsForAddress(address);
  } catch (error) {
    return next(error);
  }

  const createdPlace = new Place({
    title,
    description,
    address,
    location: coordinates,
    image:
      'https://upload.wikimedia.org/wikipedia/commons/1/10/Empire_State_Building_%28aerial_view%29.jpg',
    creator
  });

  let user;

  try {
    user = await User.findById(creator);
  } catch (err) {
    const error = new HttpError(
      // 'Creating place failed, please try again.',
      'Could not find user for provided id',
      500
    );
    return next(error);
  }

  if (!user) {
    const error = new HttpError('Could not find user for provided id', 404);
    return next(error);
  }

  console.log(user);

  try {
    // const sess = await mongoose.startSession();
    // sess.startTransaction();
    await createdPlace.save(/* { sessions: sess } */);
    user.places.push(createdPlace);
    await user.save(/* { sessions: sess } */);
    // await sess.commitTransaction();
  } catch (err) {
    console.log(err.message);
    const error = new HttpError(
      'Creating place failed, please try again.',
      500
    );
    return next(error);
  }

  res.status(201).json({ place: createdPlace });
};
```

### Deleting Places & Removing it from the User

- Modifying delete place in places controllers:
- Using `populate()`
```javascript
const deletePlace = async (req, res, next) => {
  const placeId = req.params.pid;

  let place;
  try {
    place = await Place.findById(placeId).populate('creator');
  } catch (err) {
    const error = new HttpError(
      'Something went wrong, could not delete place.',
      500
    );
    return next(error);
  }

  if (!place) {
    const error = new HttpError('Could not find place for this id.', 404);
    return next(error);
  }

  try {
    // const session = await mongoose.startSession();
    // session.startTransaction();
    await place.remove(/* { session: session } */);
    place.creator.places.pull(place);
    await place.creator.save(/* { session: session } */);
    // await session.commitTransaction();
  } catch (err) {
    const error = new HttpError(
      'Something went wrong, could not delete place.',
      500
    );
    return next(error);
  }

  res.status(200).json({ message: 'Deleted place.' });
};
```

### Getting Places - An Alternative

- Modifying get place by user id in places controllers:
```javascript
const getPlacesByUserId = async (req, res, next) => {
  const userId = req.params.uid;

  // let places;
  let userWithPlaces;
  try {
    userWithPlaces = await User.findById(userId).populate('places');
  } catch (err) {
    console.log(err.message)
    const error = new HttpError(
      'Fetching places failed, please try again later.',
      500
    );
    return next(error);
  }

  if (!userWithPlaces || userWithPlaces.length === 0) {
    const error = new HttpError(
      'Could not find places for the provided user id.',
      404
    );
    return next(error);
  }

  res.json({ places: userWithPlaces.places.map(place => place.toObject({ getters: true })) });
};
```

## 10. Connecting the React.js Frontend to the Backend

### Introduction

1. Send HTTP Requests & Use Response Data
2. Handle Errors & Loading State
3. Update User Interface

### Sending a POST Request to the Backend

- Modifying Auth.js on Frontend
```javascript
// src/user/pages/Auth.js
// ...
const Auth = () => {
// ...
  const authSubmitHandler = async event => {
    event.preventDefault();

    if (isLoginMode) {
    } else {
      try {
        const response = await fetch('http://localhost:5000/api/users/signup', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            name: formState.inputs.name.value,
            email: formState.inputs.email.value,
            password: formState.inputs.password.value
          })
        });

        const responseData = await response.json();
        console.log(responseData);
      } catch (err) {
        console.log(err);
      }
    }

    auth.login();
  };
// ...
}
// ...
```

### Handling CORS Errors

- Cross Origin Resource Sharing (CORS)
    - Enforced by browser
    - Issue here because we are trying to send request to localhost:3000 from localhost:5000
    - Need to attach certain headers
- Modifying app.js in backend:
```javascript
// backend/app.js
// ...
app.use(bodyParser.json());

app.use((req, res, next) => {
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader(
    'Access-Control-Allow-Headers',
    'Origin, X-Requested-With, Content-Type, Accept, Authorization'
  );
  res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PATCH, DELETE');
  next();
});
// ...
```

### Getting Started with Error Handling

- Modify Auth.js:
```javascript
// src/user/pages/Auth.js
// ...
// these components are provided in the course
import ErrorModal from '../../shared/components/UIElements/ErrorModal';
import LoadingSpinner from '../../shared/components/UIElements/LoadingSpinner';
// ...
const Auth = () => {
// ...
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState();
// ...
  const authSubmitHandler = async event => {
    event.preventDefault();

    if (isLoginMode) {
    } else {
      try {
        setIsLoading(true);
        const response = await fetch('http://localhost:5000/api/users/signup', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            name: formState.inputs.name.value,
            email: formState.inputs.email.value,
            password: formState.inputs.password.value
          })
        });

        const responseData = await response.json();
        console.log(responseData);
        setIsLoading(false);
        auth.login();
      } catch (err) {
        console.log(err);
        setIsLoading(false);
        setError(err.message || 'Something went wrong, please try again.');
      }
    }
    setIsLoading(false);

  };

    return (
      <Card className='authentication'>
        {isLoading && <LoadingSpinner asOverlay />}
        { /* ... */ }
      </Card>
    );
};

export default Auth;
```

### Proper Error Handling in the Frontend

- Technically no error in `fetch()` when getting back a 400ish or 500ish error code.
- Modifying Auth.js
```javascript
// src/user/pages/Auth.js
// ...
const Auth = () => {
// ...
  const authSubmitHandler = async event => {
    event.preventDefault();

    if (isLoginMode) {
    } else {
      try {
        setIsLoading(true);
        const response = await fetch('http://localhost:5000/api/users/signup', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            name: formState.inputs.name.value,
            email: formState.inputs.email.value,
            password: formState.inputs.password.value
          })
        });

        const responseData = await response.json();
        if (!response.ok) {
          throw new Error(responseData.message);
        }
        console.log(responseData);
        setIsLoading(false);
        auth.login();
      } catch (err) {
        console.log(err);
        setIsLoading(false);
        setError(err.message || 'Something went wrong, please try again.');
      }
    }
    setIsLoading(false);
  };

  const errorHandler = () => {
    setError(null);
  };

  return (
    <React.Fragment>
      <ErrorModal error={error} onClear={errorHandler} />
      <Card className='authentication'>
        {isLoading && <LoadingSpinner asOverlay />}
        { /* ... */ }
      </Card>
    </React.Fragment>
  );
};

export default Auth;
```

### Sending a Login Request

- Duplicating signup but with small changes, in Auth.js:
```javascript
// src/user/pages/Auth.js
// ...
const Auth = () => {
// ...
  const authSubmitHandler = async event => {
  event.preventDefault();
  setIsLoading(true);

  if (isLoginMode) {
    try {
      const response = await fetch('http://localhost:5000/api/users/login', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          email: formState.inputs.email.value,
          password: formState.inputs.password.value
        })
      });

      const responseData = await response.json();
      if (!response.ok) {
        throw new Error(responseData.message);
      }
      setIsLoading(false);
      auth.login();
    } catch (err) {
      console.log(err);
      setIsLoading(false);
      setError(err.message || 'Something went wrong, please try again.');
    }
  } else { 
    // ...
  }
  // ...
};

export default Auth;
```

### Gettig Users with a GET Request

- Modify Users.js:
```javascript
// src/user/pages/User.js
import React, { useEffect, useState } from 'react';

import UsersList from '../components/UsersList';
import ErrorModal from '../../shared/components/UIElements/ErrorModal';
import LoadingSpinner from '../../shared/components/UIElements/LoadingSpinner';

const Users = () => {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState();
  const [loadedUsers, setLoadedUsers] = useState();

  useEffect(() => {
    const sendRequest = async () => {
      setIsLoading(true);
      try {
        const response = await fetch('http://localhost:5000/api/users'); // defaults to get

        const responseData = await response.json();

        if (!response.ok) {
          throw new Error(responseData.message);
        }

        setLoadedUsers(responseData.users);
      } catch (err) {
        setError(err.message);
      }
      setIsLoading(false);
    };
    sendRequest();
  }, []);

  const errorHandler = () => {
    setError(null);
  };

  return (
    <React.Fragment>
      <ErrorModal error={error} onClear={errorHandler} />
      {isLoading && (
        <div className='center'>
          <LoadingSpinner />
        </div>
      )}
      {!isLoading && loadedUsers && <UsersList items={loadedUsers} />}
    </React.Fragment>
  );
};

export default Users;
```

- And line in UsersList.js
    - `placeCount={user.places.length}`

### Creating a Custom HTTP Hook

- http-hook.js so far:
```javascript
// src/shared/hooks/http-hook.js
import { useState, useCallback, useRef, useEffect } from 'react';

export const useHttpClient = () => {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState();

  const activeHttpRequests = useRef([]);

  const sendRequest = useCallback(
    async (url, method = 'GET', body = null, headers = {}) => {
      setIsLoading(true);
      const httpAbortCtrll = new AbortController();
      activeHttpRequests.current.push(httpAbortCtrll);

      try {
        const response = await fetch(url, {
          method,
          body,
          headers,
          signal: httpAbortCtrll.signal
        });

        const responseData = await response.json();

        if (!response.ok) {
          throw new Error(responseData.message);
        }

        return responseData;
      } catch (err) {
        setError(err.message);
      }
      setIsLoading(false);
    },
    []
  );

  const clearError = () => {
    setError(null);
  };

  useEffect(() => {
    return () => {
      activeHttpRequests.current.forEach(abortCtrl => abortCtrl.abort());
    };
  }, []);

  return { isLoading, error, sendRequest, clearError };
};
```

### Improving the Custom HTTP Hook

- Use in Auth.js:
```javascript
// ...
import { useHttpClient } from '../../shared/hooks/http-hook';
// ...
const Auth = () => {
  const authSubmitHandler = async event => {
    event.preventDefault();

    if (isLoginMode) {
      try {
        await sendRequest(
          'http://localhost:5000/api/users/login',
          'POST',
          JSON.stringify({
            email: formState.inputs.email.value,
            password: formState.inputs.password.value
          }),
          {
            'Content-Type': 'application/json'
          }
        );
        auth.login();
      } catch (err) {}
    } else {
      try {
        await sendRequest(
          'http://localhost:5000/api/users/signup',
          'POST',
          JSON.stringify({
            name: formState.inputs.name.value,
            email: formState.inputs.email.value,
            password: formState.inputs.password.value
          }),
          {
            'Content-Type': 'application/json'
          }
        );

        auth.login();
      } catch (err) {}
    }
  };
};

export default Auth;
```

- Also small changes to http-hook.js:
```javascript
// end of try block:
setIsLoading(false);
  return responseData;
} catch (err) {
  setError(err.message);
  setIsLoading(false);
  throw err;
}
```

### Using the HTTP Hook to GET Users

- Modify Users.js
    - useCallback in custom hook very important here, preventing infinite loops
```javascript
// src/user/pages/Users.js
import React, { useEffect, useState } from 'react';

import UsersList from '../components/UsersList';
import ErrorModal from '../../shared/components/UIElements/ErrorModal';
import LoadingSpinner from '../../shared/components/UIElements/LoadingSpinner';
import { useHttpClient } from '../../shared/hooks/http-hook';

const Users = () => {
  const { isLoading, error, sendRequest, clearError } = useHttpClient();
  const [loadedUsers, setLoadedUsers] = useState();

  useEffect(() => {
    const fetchUsers = async () => {
      try {
        const responseData = await sendRequest(
          'http://localhost:5000/api/users'
        ); // defaults to get

        setLoadedUsers(responseData.users);
      } catch (err) {}
    };
    fetchUsers();
  }, [sendRequest]);

  return (
    <React.Fragment>
      <ErrorModal error={error} onClear={clearError} />
      {isLoading && (
        <div className='center'>
          <LoadingSpinner />
        </div>
      )}
      {!isLoading && loadedUsers && <UsersList items={loadedUsers} />}
    </React.Fragment>
  );
};

export default Users;
```

### Adding Places (POST)

- Modifying NewPlace.js
```javascript
// src/places/pages/NewPlace.js
import React, { useContext } from 'react';
import { useHistory } from 'react-router-dom';
// ...
import { useHttpClient } from '../../shared/hooks/http-hook';
import { AuthContext } from '../../shared/context/auth-context';
// ...

const NewPlace = () => {
  const auth = useContext(AuthContext);
  const { isLoading, error, sendRequest, clearError } = useHttpClient();
  const [formState, inputHandler] = useForm(
    {
      title: {
        value: '',
        isValid: false
      },
      description: {
        value: '',
        isValid: false
      },
      address: {
        value: '',
        isValid: false
      }
    },
    false
  );

  const history = useHistory();

  const placeSubmitHandler = async event => {
    event.preventDefault();
    try {
      await sendRequest(
        'http://192.168.2.2:5000/api/places',
        'POST',
        JSON.stringify({
          title: formState.inputs.title.value,
          description: formState.inputs.description.value,
          address: formState.inputs.address.value,
          creator: auth.userId
        }),
        { 'Content-Type': 'application/json' }
      );
      history.push('/');
      // Redirect the user to a different page
    } catch (err) {}
  };

  return (
    <React.Fragment>
      <ErrorModal error={error} onClear={clearError} />
      <form className='place-form' onSubmit={placeSubmitHandler}>
        {isLoading && <LoadingSpinner asOverlay />}
        <Input
          id='title'
          element='input'
          type='text'
          label='Title'
          validators={[VALIDATOR_REQUIRE()]}
          errorText='Please enter a valid title.'
          onInput={inputHandler}
        />
        <Input
          id='description'
          element='textarea'
          label='Description'
          validators={[VALIDATOR_MINLENGTH(5)]}
          errorText='Please enter a valid description (at least 5 characters).'
          onInput={inputHandler}
        />
        <Input
          id='address'
          element='input'
          label='Address'
          validators={[VALIDATOR_REQUIRE()]}
          errorText='Please enter a valid address.'
          onInput={inputHandler}
        />
        <Button type='submit' disabled={!formState.isValid}>
          ADD PLACE
        </Button>
      </form>
    </React.Fragment>
  );
};

export default NewPlace;
```

- Modifying users-controllers.js, auth-context.js, Auth.js, and App.js to supply a userId
```javascript
// backend/controllers/users-controllers.js
// ...
const login = async (req, res, next) => {
  // ...

  res.json({
    message: 'Logged In!',
    user: existingUser.toObject({ getters: true })
  });
};

```
```javascript
// src/shared/context/auth-content.js
export const AuthContext = createContext({
  isLoggedIn: false,
  userId: null,
  login: () => {},
  logout: () => {}
});

```
```javascript
// src/user/pages/Auth.js
// ...
import { AuthContext } from '../../shared/context/auth-context';
// ...
const Auth = () => {
  const auth = useContext(AuthContext);
  // ...
  const authSubmitHandler = async event => {
    event.preventDefault();

    if (isLoginMode) {
      try {
        // ...
        auth.login(responseData.user.id);
      } catch (err) { }
    } else {
      try {
        // ...
        auth.login(responseData.user.id);
      } catch (err) { }
    }
  };
  // ...
};

```
```javascript
// src/App.js
// ...
const App = () => {
  // ...
  const [userId, setUserId] = useState(false);
  const login = useCallback(uid => {
    setIsLoggedIn(true);
    setUserId(uid);
  }, []);

  const logout = useCallback(() => {
    setIsLoggedIn(false);
    setUserId(null);
  }, []);
  // ...
  return (
    <AuthContext.Provider
      value={{
        isLoggedIn: isLoggedIn,
        userId: userId,
        login: login,
        logout: logout
      }}
    >
    {/* ... */}
    </AuthContext.Provider>
  );
};
// ...
```

### Loading Places by User ID

- Modify UserPlaces.js
```javascript
// src/places/pages/UserPlaces.js
import React, { useEffect, useState } from 'react';
import { useParams } from 'react-router-dom';

import PlaceList from '../components/PlaceList';
import ErrorModal from '../../shared/components/UIElements/ErrorModal';
import LoadingSpinner from '../../shared/components/UIElements/LoadingSpinner';
import { useHttpClient } from '../../shared/hooks/http-hook';

const UserPlaces = () => {
  const [loadedPlaces, setLoadedPlaces] = useState();
  const { isLoading, error, sendRequest, clearError } = useHttpClient();

  const userId = useParams().userId;

  useEffect(() => {
    const fetchPlaces = async () => {
      try {
        const responseData = await sendRequest(
          `http://192.168.2.2:5000/api/places/user/${userId}`
        );
        setLoadedPlaces(responseData.places);
      } catch (err) {}
    };
    fetchPlaces();
  }, [sendRequest, userId]);

  return (
    <React.Fragment>
      <ErrorModal error={error} onClear={clearError} />
      {isLoading && (
        <div className='center'>
          <LoadingSpinner />{' '}
        </div>
      )}
      {!isLoading && loadedPlaces && <PlaceList items={loadedPlaces} />}
    </React.Fragment>
  );
};

export default UserPlaces;

```

- Modify PlaceList.js
```javascript
// src/places/components/PlaceList.js
// ...
    <PlaceItem
          image={place.image}
          // ...
        />
// ...
```

### Updating Places

- Modifying UpdatePlace.js
```javascript
// src/places/pages/UpdatePlace.js
import React, { useEffect, useState, useContext } from 'react';
import { useParams, useHistory } from 'react-router-dom';

import Input from '../../shared/components/FormElements/Input';
import Button from '../../shared/components/FormElements/Button';
import Card from '../../shared/components/UIElements/Card';
import LoadingSpinner from '../../shared/components/UIElements/LoadingSpinner';
import ErrorModal from '../../shared/components/UIElements/ErrorModal';
import {
  VALIDATOR_REQUIRE,
  VALIDATOR_MINLENGTH
} from '../../shared/util/validators';
import { useForm } from '../../shared/hooks/form-hook';
import { useHttpClient } from '../../shared/hooks/http-hook';
import { AuthContext } from '../../shared/context/auth-context';

import './PlaceForm.css';

const UpdatePlace = () => {
  const { isLoading, error, sendRequest, clearError } = useHttpClient();
  const [loadedPlace, setLoadedPlace] = useState();
  const placeId = useParams().placeId;
  const history = useHistory();
  const auth = useContext(AuthContext);

  const [formState, inputHandler, setFormData] = useForm(
    {
      title: {
        value: '',
        isValid: false
      },
      description: {
        value: '',
        isValid: false
      }
    },
    false
  );

  useEffect(() => {
    const fetchPlace = async () => {
      try {
        const responseData = await sendRequest(
          `http://192.168.2.2:5000/api/places/${placeId}`
        );
        setLoadedPlace(responseData.place);
        setFormData(
          {
            title: {
              value: responseData.place.title,
              isValid: true
            },
            description: {
              value: responseData.place.description,
              isValid: true
            }
          },
          true
        );
      } catch (err) {}
    };
    fetchPlace();
  }, [sendRequest, placeId, setFormData]);

  const placeUpdateSubmitHandler = async event => {
    event.preventDefault();
    try {
      await sendRequest(
        `http://192.168.2.2:5000/api/places/${placeId}`,
        'PATCH',
        JSON.stringify({
          title: formState.inputs.title.value,
          description: formState.inputs.description.value
        }),
        { 'Content-Type': 'application/json' }
      );
      history.push('/' + auth.userId + '/places');
    } catch (err) {}
  };

  if (isLoading) {
    return (
      <div className='center'>
        <LoadingSpinner />
      </div>
    );
  }

  if (!loadedPlace && !error) {
    return (
      <div className='center'>
        <Card>
          <h2>Could not find place!</h2>
        </Card>
      </div>
    );
  }

  return (
    <React.Fragment>
      <ErrorModal error={error} onClear={clearError} />
      {!isLoading && loadedPlace && (
        <form className='place-form' onSubmit={placeUpdateSubmitHandler}>
          <Input
            id='title'
            element='input'
            type='text'
            label='Title'
            validators={[VALIDATOR_REQUIRE()]}
            errorText='Please enter a valid title.'
            onInput={inputHandler}
            initialValue={loadedPlace.title}
            initialValid={true}
          />
          <Input
            id='description'
            element='textarea'
            label='Description'
            validators={[VALIDATOR_MINLENGTH(5)]}
            errorText='Please enter a valid description (min. 5 characters).'
            onInput={inputHandler}
            initialValue={loadedPlace.description}
            initialValid={true}
          />
          <Button type='submit' disabled={!formState.isValid}>
            UPDATE PLACE
          </Button>
        </form>
      )}
    </React.Fragment>
  );
};

export default UpdatePlace;
```

### Deleting Places

- Modifying PlaceList.js, PlaceItem.js & UserPlaces.js
```javascript
// src/places/components/PlaceList.js
// ...
        <PlaceItem
          // ...
          onDelete={props.onDeletePlace}
        />
```
```javascript
// src/places/components/PlaceItem.js
// ...
import { useHttpClient } from '../../shared/hooks/http-hook';
import './PlaceItem.css';

const PlaceItem = props => {
  const { isLoading, error, sendRequest, clearError } = useHttpClient();
  // ...

  const confirmDeleteHandler = async () => {
    setShowConfirmModal(false);
    try {
      await sendRequest(
        `http://192.168.2.2:5000/api/places/${props.id}`,
        'DELETE'
      );
      props.onDelete(props.id);
    } catch (err) {}
  };

  return (
    <React.Fragment>
      <ErrorModal error={error} onClear={clearError} />
      { /* ... */ }
      <li className='place-item'>
        <Card className='place-item__content'>
          {isLoading && <LoadingSpinner asOverlay />}
          </Card>
      </li>
    </React.Fragment>
  );
};
// ...
```
```javascript
// src/places/pages/UserPlaces.js
// ...
  const placeDeletedHandler = deletedPlaceId => {
    setLoadedPlaces(prevPlaces =>
      prevPlaces.filter(place => place.id !== deletedPlaceId)
    );
  };
  // ...
        <PlaceList items={loadedPlaces} onDeletePlace={placeDeletedHandler} />
  // ...
```

### Fixing NavLinks and "My Places"

- Modify NavLinks.js
```javascript
          <NavLink to={`/${auth.userId}/places`}>MY PLACES</NavLink>
```

### Final Adjustments

- Modify Auth.js minimum password length to 6 to be consistent with backend
- Modify PlaceItem.js to not show buttons for places not belonging to that user.
```auth.userId === props.creatorId // as opposed to auth.isLoggedIn```

## 11. Adding File Upload

### Introduction

1. Accept & Handle Files (Images) on Backend
2. Add File Picker on Frontend + Send File to Backend

### Building an ImageUpload Component

- ImageUpload.js so far:
```javascript
import React, { useRef } from 'react';

import './ImageUpload.css';
import Button from './Button';

const ImageUpload = props => {
  const filePickerRef = useRef();

  const pickedHandler = event => {
    console.log(event.target);
  };

  const pickImageHandler = () => {
    filePickerRef.current.click();
  };

  return (
    <div className='form-control'>
      <input
        id={props.id}
        ref={filePickerRef}
        style={{ display: 'none' }}
        type='file'
        accept='.jpg,.png,.jpeg'
        onChange={pickedHandler}
      />
      <div className={`image-upload ${props.center && 'center'}`}>
        <div className='image-upload__preview'>
          <img src='' alt='Preview' />
        </div>
        <Button type='button' onClick={pickImageHandler}>
          PICK IMAGE
        </Button>
      </div>
    </div>
  );
};

export default ImageUpload;
```

- Add field to Auth.js, just after name:
    - `{!isLoginMode && <ImageUpload center id='image' />}` 

### Finishing an Using the ImageUpload Component (in a Form)

- Modifying Auth.js
```javascript
// ...
if (!isLoginMode) {
  setFormData(
    {
      ...formState.inputs,
      name: undefined,
      image: undefined
    },
    formState.inputs.email.isValid && formState.inputs.password.isValid
  );
} else {
  setFormData(
    {
      ...formState.inputs,
      name: {
        value: '',
        isValid: false
      },
      image: {
        value: null,
        isValid: false
      }
    }, false
  );
}
// ...
{!isLoginMode && (
  <ImageUpload center id='image' onInput={inputHandler} />
)}
// ...
```

### On the Backend: Using Multer to Save Files

`npm i --save multer`

- Starting on file-upload middleware
- Modifying users routes:
```javascript
// backend/routes/users-routes.js
  '/signup',
  fileUpload.single('image'),
```

### Filtering Files on the Backend (Images Only!)

- Can't rely exclusively on frontend validations!
    - Can be changed by users!

- File Upload middleware final
```javascript
// backend/middleware/file-upload.js
const multer = require('multer');
const uuid = require('uuid');

const MIME_TYPE_MAP = {
  'image/png': 'png',
  'image/jpeg': 'jpeg',
  'image/jpg': 'jpg'
};

const fileUpload = multer({
  limits: 500000,
  storage: multer.diskStorage({
    destination: (req, file, cb) => {
      cb(null, 'uploads/images');
    },
    filename: (req, file, cb) => {
      const ext = MIME_TYPE_MAP[file.mimetype];
      cb(null, uuid() + '.' + ext);
    }
  }),
  fileFilter: (req, file, cb) => {
    const isValid = !!MIME_TYPE_MAP[file.mimetype];
    let error = isValid ? null : new Error('Invalid mime type!');
    cb(error, isValid);
  }
});

module.exports = fileUpload;
```

### Wiring Frontend and Backend up

- Modify Auth.js
```javascript
// src/user/pages/Auth.js
const authSubmitHandler = async event => {
  event.preventDefault();

  if (isLoginMode) {
    // ... doesnt apply here
  } else {
    try {
      const formData = new FormData();
      formData.append('email', formState.inputs.email.value);
      formData.append('name', formState.inputs.name.value);
      formData.append('password', formState.inputs.password.value);
      formData.append('image', formState.inputs.image.value)
      const responseData = await sendRequest(
        'http://192.168.2.2:5000/api/users/signup',
        'POST',
        formData
        // headers set automatically when using form data
      );
      auth.login(responseData.user.id);
    } catch (err) {}
  }
};
```

### Connecting Users to Images

- Modifying app.js for rollback of image upload on error
```javascript
// backend/app.js
app.use((error, req, res, next) => {
  if (req.file) {
    fs.unlink(req.file.path, err => {
      console.log(err);
    });
  }
  // ...
});
```

- Link to User
```javascript
// backend/controllers/users-controllers.js
// ...
  const createdUser = new User({
    name,
    email,
    image:
      // old static url to external image,
      req.file.path,
    password,
    places: []
  });
// ...
```

### Serving Images Statically

- Modifying app.js in backend
```javascript
// backend/app.js
// ...
app.use(bodyParser.json());

app.use('/uploads/images', express.static(path.join('uploads', 'images')));
// ...
```

### Uploading Images for New Places

- Modify NewPlace.js
```javascript
// ...
import ImageUpload from '../../shared/components/FormElements/ImageUpload';
// ...

const NewPlace = () => {
  // ...
// add image to useForm data:
  const [formState, inputHandler] = useForm(
    {
      // ...
      image: {
        value: null,
        isValid: false
      }
    },
    false
  );
  // ...
  const placeSubmitHandler = async event => {
    event.preventDefault();
    try {
      const formData = new FormData();
      formData.append('title', formState.inputs.title.value);
      formData.append('description', formState.inputs.description.value);
      formData.append('address', formState.inputs.address.value);
      formData.append('creator', auth.userId);
      formData.append('image', formState.inputs.image.value);
      await sendRequest('http://192.168.2.2:5000/api/places', 'POST', formData);
      history.push('/');
      // Redirect the user to a different page
    } catch (err) {}
  };

  return (
    <React.Fragment>
        {/* ... */}
        <ImageUpload
          id='image'
          onInput={inputHandler}
          errorText='Please provide an image.'
        />
        <Button type='submit' disabled={!formState.isValid}>
          ADD PLACE
        </Button>
      </form>
    </React.Fragment>
  );
};
```

- Modifying places routes
```javascript
// backend/routes/places-routes.js
// ...
const fileUpload = require('../middleware/file-upload');
// ...
router.post(
  '/',
  fileUpload.single('image'),
  // ...
)
```

- Modify places controllers
```javascript
// backend/controllers/places-controllers.js
  const createdPlace = new Place({
    title,
    description,
    address,
    location: coordinates,
    image:
      // 'https://upload.wikimedia.org/wikipedia/commons/1/10/Empire_State_Building_%28aerial_view%29.jpg',
      req.file.path,
    creator
  });
```

- Modify PlaceItem.js:
```javascript
<div className='place-item__image'>
  <img src={`http://192.168.2.2:5000/${props.image}`} alt={props.title} />
</div>
```

### Deleting Images When Places Get Deleted

- Modifying places controllers
```javascript
// backend/controllers/places.controllers.js
const fs = require('fs')
// ...
const deletePlace = async (req, res, next) => {
  // ...
  const imagePath = place.image;

  try {
    // ...
  }

  fs.unlink(imagePath, err => {
    console.log(err);
  });
};
```

## 12. Adding Authentication

### Introduction

1. Add Authentication Logic on both Ends

### How Authentication Works (in aa MERN App)

![SPA Authentication](img/mern-auth.jpg "mern-auth")

### Hashing the User Password

***Never store passwords in plain text!***

- Using bcrypt.js
    - `npm i --save bcrypt.js`

- Utilize in signup user controller
```javascript
// backend/controllers/users-controllers/signup()
const bcrypt = require('bcryptjs');
// ...
let hashedPassword;
try {
  hashedPassword = await bcrypt.hash(password, 12);
} catch (err) {
  const error = new HttpError(
    'Could not create user, please try again.',
    500
  );
  return next(error);
}

const createdUser = new User({
  // ...
  password: hashedPassword,
  // ...
});
```

### Logging Users In (with Hashed Passwords)

- Utilize in login user controller
```javascript
// backend/controllers/users-controllers/login()
let isValidPassword = false;
try {
  isValidPassword = await bcrypt.compare(password, existingUser.password);
} catch (err) {
  const error = new HttpError(
    'Could not log you in, please check your credentials and try again.',
    500
  );
  return next(error);
}

if (!isValidPassword) {
  const error = new HttpError(
    'Invalid credentials, could not log you in.',
    401
  );
  return next(error);
}

res.json({ /* ... */})
```

### Generating Tokens (JWT) on the Backend

- With jsonwebtoken
    - `npm i --save jsonwebtoken`
- Utilize in login user controller
```javascript
// backend/controllers/users-controllers.js
const jwt = require('jsonwebtoken');
// ...
const signup = async (req, res, next) => {
let token;
  try {
    token = jwt.sign(
      { userId: createdUser.id, email: createdUser.email },
      'supersecret_dont_share',
      { expiresIn: '1h' }
    );
  } catch (err) {
    console.log(err.message);
    const error = new HttpError(
      'Signing up failed, please try again later.',
      500
    );
    return next(error);
  }

  res
    .status(201)
    .json({ userId: createdUser.id, email: createdUser.email, token: token });
};

const login = async (req, res, next) => {
  // ...

  let token;
  try {
    token = jwt.sign(
      { userId: existingUser.id, email: existingUser.email },
      'supersecret_dont_share',
      { expiresIn: '1h' }
    );
  } catch (err) {
    console.log(err.message);
    const error = new HttpError('Login failed, please try again later.', 500);
    return next(error);
  }

  res.json({ userId: existingUser.id, email: existingUser.email, token: token });
};
```

### Backend Route Protection with Auth Middleware

- Use in header, through middleware:
```javascript
// backend/middleware/check-auth.js
const jwt = require('jsonwebtoken');

const HttpError = require('../models/http-error');

module.exports = (req, res, next) => {
  try {
    const token = req.headers.authorization.split(' ')[1]; // Authorization: 'Bearer TOKEN'
    if (!token) {
      throw new Error('Authentication failed!');
    }
    const decodedToken = jwt.verify(token, 'supersecret_dont_share');
    req.userData = { userId: decodedToken.userId };
    next();
  } catch (err) {
    const error = new HttpError('Authentication failed!', 401);
    return next(error);
  }
};
```

- Remember, order is important with routes, top to bottom!
```javascript
// backend/routes/places-routes.js
// ...
const checkAuth = require('../middleware/check-auth');
// ...
router.get('/user/:uid', placesControllers.getPlacesByUserId);

router.use(checkAuth);
```

### Using and Attaching JWT (Tokens) in React

- Modify auth login in Auth.js
```javascript
// src/user/pages/Auth.js
// in BOTH login & signup modes:
auth.login(responseData.userId, responseData.token);
```

- Modify in App.js
```javascript
// src/App.js
const App = () => {
  const [token, setToken] = useState(false);
  // ...
  const login = useCallback((uid, token) => {
    setToken(token);
    setUserId(uid);
  }, []);

  const logout = useCallback(() => {
    setToken(null);
    setUserId(null);
  }, []);

  let routes;

  if (token) { /**/ }
  // ...

  return(
    <AuthContext.Provider
      value={{
        isLoggedIn: !!token,
        token: token,
        // ...
      }}
    >
    { /**/ }
    </ AuthContext.Provider>
  );
```

- Also modify the auth context
```javascript
// src/shared/context/auth-context.js
import { createContext } from 'react';

export const AuthContext = createContext({
  isLoggedIn: false,
  userId: null,
  token: null,
  login: () => {},
  logout: () => {}
});
```

- Add token to header on new place request
```javascript
// src/places/pages/NewPlace.js
await sendRequest('http://192.168.2.2:5000/api/places', 'POST', formData, {
  Authorization: 'Bearer ' + auth.token
});
```

- Handling OPTIONS Request
```javascript
// backend/middleware/check-auth.js
module.exports = (req, res, next) => {
  if (req.method === 'OPTIONS') {
    return next();
  }
  // ...
};
```

### Using Tokens to Update and Delete Places

- Modifying UpdatePlace.js
```javascript
// src/places/pages/UpdatePlace.js
// add auth header to update(patch) request:
{ 'Content-Type': 'application/json', Authorization: 'Bearer ' + auth.token }
```

- Set up for delete in PlaceItem.js:
```javascript
// src/places/pages/UpdatePlace.js
// in confirm delete handler:
'DELETE',
null,
{ Authorization: 'Bearer ' + auth.token }
```

### Adding Authorization

- On the backend, updating places:
```javascript
// backend/controllers/places-controllers.js
// in updatePlace():
if (place.creator.toString() !== req.userData.userId) {
  const error = new HttpError('You are not allowed to edit this place.', 401);
  return next(error);
}

place.title = title;
// ...
```

### Authorization on "Delete Places"

- On the backend, deleting places:
```javascript
// backend/controllers/places-controllers.js
// in deletePlace():
if (place.creator.id !== req.userData.userId) {
  const error = new HttpError('You are not allowed to edit this place.', 401);
  return next(error);
}

const imagePath = place.image;
```

### Storing the Token in the Browser Storage

- Have Token survive page reloads by storing in browser's local storage
- Modify App.js on frontend:
```javascript
// src/App.js
const login = useCallback((uid, token) => {
  setToken(token);
  localStorage.setItem(
    'userData',
    JSON.stringify({
      userId: uid,
      token: token
    })
  );
  setUserId(uid);
}, []);
```

### Adding Auto-Login (Basic Version)

- By utilizing useEffect in App.js:
```javascript
// src/App.js
const logout = useCallback(() => {
  setToken(null);
  setUserId(null);
  localStorage.removeItem('userData');
}, []);

useEffect(() => {
  const storedData = JSON.parse(localStorage.getItem('userData'));
  if (storedData && storedData.token) {
    login(storedData.userId, storedData.token);
  }
}, [login]);
```

### Managing the Token Expiration Date

- Modify App.js:
```javascript
const login = useCallback((uid, token, expirationDate) => {
  setToken(token);
  setUserId(uid);
  const tokenExpirationDate =
    expirationDate || new Date(new Date().getTime() + 1000 * 60 * 60);
  localStorage.setItem(
    'userData',
    JSON.stringify({
      userId: uid,
      token: token,
      expiration: tokenExpirationDate.toISOString()
    })
  );
}, []);
// ...
// also make sure to pass in useEffect:
useEffect(() => {
  const storedData = JSON.parse(localStorage.getItem('userData'));
  if (
    storedData &&
    storedData.token &&
    new Date(storedData.expiration) > new Date()
  ) {
    login(
      storedData.userId,
      storedData.token,
      new Date(storedData.expiration)
    );
  }
}, [login]);
```

### Finished Auto-Login & Auto Logout

- Set up logout timer in App.js with another useEffect
```javascript
// src/App.js
// ...
let logoutTimer;

const App = () => {
  const [tokenExpirationDate, setTokenExpirationDate] = useState();
  // ...
  const logout = useCallback(() => {
    setToken(null);
    setTokenExpirationDate(null);
    setUserId(null);
    localStorage.removeItem('userData');
  }, []);

  useEffect(() => {
    if (token && tokenExpirationDate) {
      const remainingTime = tokenExpirationDate.getTime() - new Date().getTime();
      logoutTimer = setTimeout(logout, remainingTime);
    } else {
      clearTimeout(logoutTimer);
    }
  }, [token, logout, tokenExpirationDate]);
};
```

### Creating a Custom Authentication Hook

- Move auth logic from App.js to custom hook
```javascript
// src/shared/hooks/auth-hook.js
import { useState, useCallback, useEffect } from 'react';

let logoutTimer;

export const useAuth = () => {
  const [token, setToken] = useState(false);
  const [tokenExpirationDate, setTokenExpirationDate] = useState();
  const [userId, setUserId] = useState(false);

  const login = useCallback((uid, token, expirationDate) => {
    setToken(token);
    setUserId(uid);
    const tokenExpirationDate =
      expirationDate || new Date(new Date().getTime() + 1000 * 60 * 60);
    setTokenExpirationDate(tokenExpirationDate);
    localStorage.setItem(
      'userData',
      JSON.stringify({
        userId: uid,
        token: token,
        expiration: tokenExpirationDate.toISOString()
      })
    );
  }, []);

  const logout = useCallback(() => {
    setToken(null);
    setTokenExpirationDate(null);
    setUserId(null);
    localStorage.removeItem('userData');
  }, []);

  useEffect(() => {
    if (token && tokenExpirationDate) {
      const remainingTime =
        tokenExpirationDate.getTime() - new Date().getTime();
      const minutesLeft = remainingTime / 60000;
      console.log(minutesLeft + ' minutes left.');
      logoutTimer = setTimeout(logout, remainingTime);
    } else {
      clearTimeout(logoutTimer);
    }
  }, [token, logout, tokenExpirationDate]);

  useEffect(() => {
    const storedData = JSON.parse(localStorage.getItem('userData'));
    if (
      storedData &&
      storedData.token &&
      new Date(storedData.expiration) > new Date()
    ) {
      login(
        storedData.userId,
        storedData.token,
        new Date(storedData.expiration)
      );
    }
  }, [login]);

  return { token, login, logout, userId };
};
```
- App.js final
```javascript
import React from 'react';
import {
  BrowserRouter as Router,
  Route,
  Redirect,
  Switch
} from 'react-router-dom';

import Users from './user/pages/Users';
import NewPlace from './places/pages/NewPlace';
import UserPlaces from './places/pages/UserPlaces';
import UpdatePlace from './places/pages/UpdatePlace';
import Auth from './user/pages/Auth';
import MainNavigation from './shared/components/Navigation/MainNavigation';
import { AuthContext } from './shared/context/auth-context';
import { useAuth } from './shared/hooks/auth-hook';

const App = () => {
  const { token, login, logout, userId } = useAuth();
  let routes;

  if (token) {
    routes = (
      <Switch>
        <Route path='/' exact>
          <Users />
        </Route>
        <Route path='/:userId/places'>
          <UserPlaces />
        </Route>
        <Route path='/places/new' exact>
          <NewPlace />
        </Route>
        <Route path='/places/:placeId'>
          <UpdatePlace />
        </Route>
        <Redirect to='/' />
      </Switch>
    );
  } else {
    routes = (
      <Switch>
        <Route path='/' exact>
          <Users />
        </Route>
        <Route path='/:userId/places'>
          <UserPlaces />
        </Route>
        <Route path='/auth' exact>
          <Auth />
        </Route>
        <Redirect to='/auth' />
      </Switch>
    );
  }

  return (
    <AuthContext.Provider
      value={{
        isLoggedIn: !!token,
        token: token,
        userId: userId,
        login: login,
        logout: logout
      }}
    >
      <Router>
        <MainNavigation />
        <main>{routes}</main>
      </Router>
    </AuthContext.Provider>
  );
};

export default App;
```

### General App Improvements

- Modify places controllers to get creator from userData instead of req body
```javascript
const createdPlace = new Place({
  // ...
  creator: req.userData.userId
});
```

## 13. Application Deployment

### Introduction

1. Deployment Preparation
2. Different Ways of Deploying MERN Apps

### Deployment Steps Overview

1. Develop & Test Your Code
2. Swap URLs & Credentials for Production-ready Ones
      - Consider using Environment Variables
3. Build & Deploy
4. Test Deployed Application, Monitor & Adjust

### Using Environment Variables

- Utilizing nodemon
```json
// backend/nodemon.json
{
  "env": {
    "DB_URL": "mongodb://localhost:27017/react-training",
    "PORT": 5000,
    "GOOGLE_API_KEY": "AIzaSyC7ap01wqRWATA7guCbFBvIgpbYZwKQIEo",
    "JWT_KEY": "supersecret_dont_share"
  }
}
```

### Environment Variables in React Apps

- Achieved with `.env` file in root folder
    - *Everyone can see because it is running in browser!!*
```
REACT_APP_GOOGLE_API_KEY=AIzaSyC7ap01wqRWATA7guCbFBvIgpbYZwKQIEo
REACT_APP_BACKEND_URL=http://192.168.2.2:5000/api
REACT_APP_ASSET_URL=http://192.168.2.2:5000
```
```html
<!-- how to replace in index.html for google api key: -->
<script
  src="https://maps.googleapis.com/maps/api/js?key=%REACT_APP_GOOGLE_API_KEY%"
  async
  defer
></script>
```

- Replace all instances of localhost, 192.168.2.2, etc with React env variables
    - `${process.env.REACT_APP_BACKEND_URL}`

### Preparing API Keys and Credentials

- Application & Key restrictions on Google API keys
- Configuring whitelist for (cloud) mongodb database

### Building the React App

- To build: `npm run build`
- Using production environment variables:
    - Using `.env.production` file
        - Automatically picked up by `npm run build`
        - Swap API keys, etc
        - Will revisit URLs (i.e. localhost)

### Adding Code Splitting to React

- Prevent loading code that is not needed yet (lazy loading)
- Modify App.js
```javascript
// src/App.js
import React, { Suspense } from 'react';

// import Users from './user/pages/Users';
// import NewPlace from './places/pages/NewPlace';
// import UserPlaces from './places/pages/UserPlaces';
// import UpdatePlace from './places/pages/UpdatePlace';
// import Auth from './user/pages/Auth';

const Users = React.lazy(() => import('./user/pages/Users'));
const NewPlace = React.lazy(() => import('./places/pages/NewPlace'));
const UserPlaces = React.lazy(() => import('./places/pages/UserPlaces'));
const UpdatePlace = React.lazy(() => import('./places/pages/UpdatePlace'));
const Auth = React.lazy(() => import('./user/pages/Auth'));

// also need to wrap routes with Suspense
<Router>
  <MainNavigation />
  <main>
    <Suspense
      fallback={
        <div className='center'>
          <LoadingSpinner />
        </div>
      }
    >
      {routes}
    </Suspense>
  </main>
</Router>
```

### Understanding Possible Deployment Alternatives

- Two main options
    - Deploy Two Separated Apps
        - React App
            - Static Host (only serve HTML, JS, and CSS)
            - AWS S3, Firebase Hosting, etc...
        - Node REST API
            - Node.js Host
            - AWS EC2 / EB, Heroku, etc...
        - ***CORS Headers absolutely required!***
    - Deploy One Combined App
        - Node REST API which renders the React App, too
        - Node.js Host
        - AWS EC2 / EB, Heroku, etc...
        - ***CORS Headers are NOT required!***

### Deploying a Standalone REST API (Example: Heroku)

- Hosting the NodeJS application
- Use Heroku
    - Create account
    - Initialize git on project
    - Install and setup heroku CLI
    - Setup deployment
        - Settings > Config Vars (for environment variables)
            - Make sure to set up port (heroku gives with `process.env.PORT`)
        - Whitelist network connection for MongoDB <--> Heroku
        - Push code to Heroku

### Deploying a Standalone React SPA (Example: Firebase Hosting)

- Replace React production environment variables for backend/asset URLs
- Can use `npm i -g serve` to serve static content to test the React web app build before deployment
- Use Firebase Hosting for hosting static React app
    - Install firebase tools
        1. `firebase login`
        2. `firebase init` (in project folder)
        3. Public directory? `build`
        4. Single-page-app? `yes`
        5. Don't overwrite index.html
        6. `firebase deploy`

### Fixing Style Issues

- Styling issue related to ordering of styles
    - use `!important` or specify with `.card.user-item__content`

### Deploying a Combined App

- Grab build folder and copy to backend
    - React environment variables may change (urls)
- Copy build folder into a backend/public/ folder
- After routes in app.js, serve public folder statically (comment out error for not found routes)
```javascript
// ...
app.use('/api/users', usersRoutes); // => /api/users/ ...

app.use(express.static(path.join('public')));

// app.use((req, res, next) => {
//   const error = new HttpError('Could not find this route.', 404);
//   throw error;
// });

// ...
```

- Then move to top and create catch-all route:
```javascript
app.use('/uploads/images', express.static(path.join('uploads', 'images')));
app.use(express.static(path.join('public')));

// ...

app.use('/api/users', usersRoutes); // => /api/users/ ...

app.use((req, res, next) => {
  res.sendFile(path.resolve(__dirname, 'public', 'index.html'));
});
```

- Now we can deploy it (heroku)

### Storing Files/Uploads

- Files aren't persistently stored and are lost when server restarts
    - Host statically elsewhere? (Heroku specific solutions)


## The End