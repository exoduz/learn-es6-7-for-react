# Learn ES6/7 for React.js #
##### http://courses.reactjsprogram.com/courses/es6forreact #####

## Section 1 - Babel. const. let. Modules. Destructuring. ##

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
	if (num >= - {
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
} //will thros a "User is already defined" error, trying to re-assign actual reference
```

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

### Object Destructuring ###

Convenient way to extract values from data stored in objects and arrays.

```javascript
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

```javascript
//ugly!
var ReactRouter = require('react-router');
var Route = ReactRouter.Route;
var Link = ReactRouter.Link;
var Router = ReactRouter.Router;

//better!
var { Route, Link, Router } = require('react-router');
```

## Section 2 - Concise Object Methods. Arrow Functions. ##

### Concise Object Methods ###
