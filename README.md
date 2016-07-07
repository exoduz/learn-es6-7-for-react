# Learn ES6/7 for React.js #
##### http://courses.reactjsprogram.com/courses/es6forreact #####

### Contents ###
1. [Babel. const. let. Modules. Destructuring.](#section-1)
	1. [New Variable Declarations with let and const](#section-1-1)
	2. [`import` and `export` with ES6 Modules](#section-1-2)
	3. [Object Destructuring](#section-1-3)
2. [Concise Object Methods. Arrow Functions.](#section-2)
 	1. [Concise Object Methods](#section-2-1)
 	2. [Arrow Functions](#section-2-2)
3. [Template Strings. Default Parameters. Concise Objects.](#section-3)
	1. [Template Strings (AKA Template Literals)](#section-3-1)
	2. [Default Parameters](#section-3-2)
	3. [Concise Objects](#section-3-3)
4. [Async/Await](#section-4)
5. [Classes](#section-5)

<a id="section-1"></a>
## Section 1 - Babel. const. let. Modules. Destructuring. ##

<a id="section-1-1"></a>
### New Variable Declarations with let and const ###

Javascript is **function scoped**. Only functions introduce new scopes.

```javascript
//Global Scope
var functionFunction = function(0 {
	//firstFunction's scope
	var seconeFunction = function() {
		//secondFunction's scope	
	};
};

//E.g.
function doThing() {
	var num = 1;
	if (num >= 0) {
		var secondNum = 2;
		console.log(num); // 1
		console.log(secondNum); // 2
	}
	console.log(num); // 1
	console.log(secondNum); // 2
}
doThing();
```

`let` allows you to have code which is block scoped.

```javascript
function doThing() {
	var num = 1;
	if (num >= 0) {
		let secondNum = 2;
		console.log(num); // 1
		console.log(secondNum); // 2
	}
	console.log(num); // 1
	console.log(secondNum); //Uncaught ReferenceError: secondNum is not defined
}
```

`const` is like `let` except that variable cannot be re-assigned a new reference.

```javascript
const user = {
	name: 'Tyler',
	age: 25
}

user.name = 'Joey'; //valid because it's just re-assigning a specific value

user = {
	name: 'Joey',
	age: 28
} //will throw a "User is already defined" error, trying to re-assign actual reference
```

<a id="section-1-2"></a>
### `import` and `export` with ES6 Modules ###

```javascript
//math.js
export function add(x,y) {
	return x + y;
}
export function multiply(x,y) {
	return x * y;
}
export function divide(x,y) {
	return x / y;
}


//main.js
import { add, multiple } from './math'; //named imports, only import a few properties
add(1, 2); //3
multiply(3, 4); //12

import * as math from './math';
add(1, 2); //3
multiply(3, 4); //12
divide(4, 4); //1
```

You can also have modules that only export single values using `default`. (Not using named imports). You can even mix **named exports** with `default` exports.

```javascript
//math.js
export function add(x, y) {
	return x + y;
}
export default function doAllTheMath(x, y, z) {
	return x + y + x * x * y * z / x / y / z;
}

//main.js
import doAllTheMath from './math'; //default export (not named export)
doAllTheMath(1, 2, 3); //4

import doAllTheMath, { add } from './math'; //mixed (both named and default export)
doAllTheMath(1, 2, 3); //4
add(1, 2); //3
```

<a id="section-1-3"></a>
### Object Destructuring ###

Convenient way to extract values from data stored in objects and arrays.

```jsx
function register(props) {
	//ugly!
	var onChangeEmail = props.onChangeEmail;
	var email = props.email;
	var onChangePassword = props.onChangePassword;
	var password = props.password;
	var submit = props.submit;
	
	//better!
	var { onChangeEmail, email, onChangePassword, password, submit }  = props;
	
	return (
		<div>
			<input type='text' onChange={ onChangeEmail } value={ email } />
			...
		</div>
	)
}
```

Another great example is **named imports**

```jsx
//ugly!
var ReactRouter = require('react-router');
var Route = ReactRouter.Route;
var Link = ReactRouter.Link;
var Router = ReactRouter.Router;

//better!
var { Route, Link, Router } = require('react-router');
```

<a id="section-2"></a>
## Section 2 - Concise Object Methods. Arrow Functions. ##

<a id="section-2-1"></a>
### Concise Object Methods ###

**Concise Object Methods** allows you to drop the `: function` part of a method.

```javascript
var actions = {
	sayName: function() { ... },
	takeStep: function() { ... }
}

//above can be written as...
var actions = {
	sayName() { ... },
	takeStep() { ... }
}
```

With `React` code

```jsx
var Register = React.createClass({
	getDefaultProps: function() { ... }.
	componentDidMount: function() { ... },
	shouldComponentUpdate: function() { ... },
	render: function() { ... },
})

//above can be written as
var Register = React.createClass({
	getDefaultProps() { ... }.
	componentDidMount() { ... },
	shouldComponentUpdate() { ... },
	render() { ... },
})
```

<a id="section-2-2"></a>
### Arrow Functions ###

Why were **Arrow Functions** added to the spec?  
More concise syntax and sharing lexical `this` with the parent scope.

```jsx
var FriendsList = React.createClass({
	getInitialState() {
		return {
			friends: [
				{ id: 0, name: 'Mikenzi' },
				{ id: 1, name: 'Ryan' },
				{ id: 2, name: 'Jake' }
			]
		}
	},
	onAddFriend(friend) {
		this.setState({
			friends: this.state.friends.concat([friend])
		})
	},
	render() {
		return (
			<ul>
				{this.state.friends.map(function (friend) {
					return <FriendItem key={ friend.id } handleAddFriend={ this.onAddFriend }>{ friend.name }</FriendItem>
          	})} />
          </ul>
		)
	}
});
```

`this` is not in the context of `onAddFriend` method. One fix is to `bind(this)` to the end of the `map` invocation. More elegant solution is to use the **arrow function**.

```jsx
render() {
	return (
		<ul>
			{this.state.friends.map((friend) => {
				return <FriendItem key={ friend.id } handleAddFriend={ this.onAddFriend }>{ friend.name }></FriendItem>
			})}
		</ul>
	)
}
```

If you have everything on one line, the **arrow function** will implicitly return whatever is on that line, without the need of a `return` statement.

```jsx
//above code rewritten with one line
render() {
	return (
		<ul>
			{ this.state.friends.map((friend) => <FriendItem key={ friend.id } handleAddFriend={ this.onAddFriend }>{ friend.name }</FriendItem>)}
		</ul>
	)
}
```

<a id="section-3"></a>
## Section 3 - Template Strings. Default Parameters. Concise Objects. ##

<a id="section-3-1"></a>
### Template Strings (AKA Template Literals) ###

Replacement for string concatenation without the `+`.  
Wrap the line in back ticks (\`), not quotations ('), each variable is wrapped in `${}`. Also supports multi-line strings.

```javascript
'Hello, ' + name + '. We\'ve emailed you at ' + email '. Your user id is ' + id + '.'; //old
`Hello, ${name}. We've emailed you at ${email}. Your user id is ${id}.` //using template strings

//also supports multi-line
var thing = `this is a multi
	line
	update and it is 
	valid.
`
```

<a id="section-3-2"></a>
### Default Parameters ###

Setting inital/default parameters if those arguments aren't specified.

```javascript
//without default parameters
function debounce(func, wait, immediate) {
	if (typeof wait === 'undefined') {
		wait = 1000
	}
}

//with default parameters
function debounce (func, wait = 1000, immediate) { ... }
```

<a id="section-3-3"></a>
### Concise Objects ###

```javascript
function getUser(username) {
	const email = getEmail(username);
	
	//without concise objects, mirroring occurs
	//username: username, email: email...
	return {
		username: username,
		email: email
	}
	
	//with concise objects
	return {
		username,
		email
	}
}
```

<a id="section-4"></a>
## Section 4 - Async/Await ##

Writing asynchronous code that appears to be synchronous.

**Every async function you write will return a promise, and every single thing you await will ordinarily be a promise**

Promises allow to pass a function to `.then` and that function will be called when our promise resolves (when the user object is ready).

```javascript
function get User() {
	return new Promise((resolve, reject) => {
		setTimeout(() => resolve({ name: 'Tyler' }), 2000); //run after 2 seconds
	})
}

function handleGetUser() {
	getUser()
	.then((user) => {
		console.log('The user is: ', user);
	})
	.catch((error) => {
		console.warn('Oh no there was an error: ', error);
	})
}

//will not work because results DNE
function handleGetUser() {
	var user = getUser();
	console.log(user);
}

//works
//remember "if I want to use await, I need to make sure that the function I'm in is an async function"
async function handleGetUser() {
	try {
		var user = await getUser() //await tells compiler to pause execution until the promise resolves
		console.log(user)
	} catch (error) {
		console.log('Error in handleGetUser', error)
	}
}
```

<a id="section-5"></a>
## Section 5 - Classes ##

```jsx
var TodoApp = React.createClass({
	propTypes: {
		title: PropTypes.string.isRequired
	},
	getInitialState() {
		return {
			items: []
		};
	},
  	updateItems(newItem) {
		var allItems = this.state.items.concat([newItem]);
		this.setState({items: allItems});
	},
	render() {
		return (
			<div>
				<TodoBanner title={this.props.title}/>
				<TodoList items={this.state.items}/>
				<TodoForm onFormSubmit={this.updateItems}/>
			</div>
		);
	}
});
```

In ES6

```jsx
class TodoApp extends React.Component {
	constructor() {
		super()
		this.state = {
			items: []
		}
	}
	updateItems(newItem) {
		var allItems = this.state.items.concat([newItem]);
		this.setState({items: allItems});
	}
	render() {
		return (
			<div>
				<TodoBanner title={this.props.title}/>
				<TodoList items={this.state.items}/>
				<TodoForm onFormSubmit={() => this.updateItems()}/>
			</div>
    );
  }
}

TodoApp.propTypes = {
	title: PropTypes.string.isRequired
}
```

#### Differences ####

1. Obviously we're no longer using createClass. Instead, we're creating a class that extends React.Component.
2. Classes have constructor functions which initialize the object which is being returned from the class. This essentially replaces our need to getInitialState since we can set a state instance on the component itself during the constructor phase. Also note before you set anything to the instance you need to call super() which calls the constructor function of the thing we're extending, in this case React.component.
3. We no longer have commas between our properties since we're not in an object anymore.
4. PropTypes are moved out of the class and added afterwards, just like we're used to doing with Stateless Functional Components.
5. When we use createClass, the 'this' keyword is autobound for us. Meaning, we don't have to worry about which context the function using 'this' will be called in since it will always be bound to the original context. This isn't the case for classes. We can use .bind to fix this problem when we're passing in a function to another component (context) or we can use our handy dandy arrow functions as we did passing updateItems to onFormSubmit.
