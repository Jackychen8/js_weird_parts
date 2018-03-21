# Understanding Javascript: The Weird Parts

## Syntax Parser: 
A program that reads your code and determines 
what it does and if its grammar is valid

## Lexical Environment:
Where something sits phsyically in the code you write
(where you write something is important)

## Execution Context:
A wrapper to help manage the code that is running.
Execution context helps manage which lexical environment is running

## Global Execution Context
When JS is executed, it creates global object and 'this' (which will be global object)

-----------------------------
'hoisting' (Execution Context Phases: Creation and Execution Phase)
-----------------------------

OG file
-----------------------------
```javascript
b();
console.log(a);
var a = 'Hello World!';
function b() {
	console.log('Called b!');
}
```
-----------------------------
gives
-----------------------------
```javascript
'Called b!'
undefined
```
-----------------------------

This happens because of the creation phase which sets up memory space for variables and functions (Hoisting)

During Creation Phase:
1. function is placed into memory entirely
2. variable gets a placeholder (undefined)


undefined is not 'ReferenceError undefined' (which is that it is not prepped in memory)
-------------------------------------------
```javascript
var a;
if (a === undefined) {
	console.log('a is undefined!');
} else {
	console.log('a is defined!');
}
```

## Execution Phase 
------------------
Runs code line by line


Single Threaded: One command at a time
Synchronous: One line at a time in the order that it appears

!!!Every function creates a new execution context and gets placed on a stack.!!!!!
When done, function/execution context is popped off stack

Order of execution determines Executioin Context stack
Lexical environment determines variable environment stack (Scope Chain)
(where a function searches for variables not found in current context)


Scope (let vs var)
1. let provides block scoping
	* c cannot be called before it is declared (will error instead of being undefined)

Asynchronous Calls
1. Uses 'another stack' (Event Queue)
	* Only runs when Synchronous Execution Context Stack is empty

Types
1. Dynamic Typing (js figures out the type of your variable)
2. Primitive Type: a type of data that represents a single value
	* undefined
	* null
	* true/false
	* number (always floating point, no int)
	* string
	* symbol (ES6)

Operators: a special function that is syntactically (written) differently 
1. Precedence/Order
	* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence
2. Associativity
	* left-to-right, right-to-left


Coercion: converting a value from one type to another
1. Example: var a = 1 + '2';// become '12'
2. 3<2<1 yields True because Number(false) < 1 is True
```javascript
	Number('3') // 3
	Number(false) //0
	Number(undefined) //NaN
	Number(null) // 0
```
3. Equals (==) (!=)
```javascript
	false == 0 is true
	null == 0 is false
	'' == 0 is true
	'' == false is true
```
4. Strict Equals (===) (!==)
	A. checks type, no coercion
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness

js tags in HTML
1. all of the javascript moved into one file and run in order
```javascript
var libararyName = window.libraryName || 'liberary1';
```

## Objects and Functions (Everything in JS is a primitive or object(object is a primitive))
1. Objects can have 
	A. properties
		i. primitives
		ii. objects
	B. methods
		i. Functions
2. [] is 'computed member access operator' (left-to-right)
3. . is 'member access operator' (left-to-right)
4. dot operator prefered. Unless need access w/ dynamic string 
5. object literal '{}' vs new Object();


Difference between JS object and JSON
1. JSON.stringify(objectLiteral) makes it JSON
2. strings as keys
3. can't have functions as values


## Functions are Objects 
1st Class Functions: everything you can do with other types you can do with functions
1. assign them to variables
2. pass them around
3. create them on the fly
4. properties
	1. Name (optional, can be anonymous)
	2. Code (the code of the function) 
		1. can't actually access it with .
		2. must be invoked ()

## Function statements and expressions (unit of code that results in/returns a value)
1. expressions examples
```javascript
	A. a = 3, 1+2, b = {a:1}
	B. var anonymousGreet = function() {}
```
2. statement examples
```javascript
	if (a === 3) {} 
	function greet(){}

# error
anonymousGreet();
var anonymousGreet = function() {}

# no error
var anonymousGreet = function() {}
anonymousGreet();
```

# By Value/Copy vs By Reference
1. Value
	* primitive copy values
2. Reference
	1. All Object including functions
		1. even as function parameters
	2. however assignment operator (=) sets up new memory space 


# Objects, Functions, and 'this'
1. method will return 'this' of method's object
	* 	However, if there is a function created and called in that method
		that function will access the global object as 'this'!
2. var self = this;// helps maintain what 'this' is
3. use let instead of var
```javascript
---------------------------------------
function a() { console.log(this); }
var b = function() { console.log(this) }
a();
b();//Both return global object as 'this'
---------------------------------------
var c = {
	log: function() { 
		console.log(this);
		var setName = function(newname) {
			this.name = newname;
		}
		setName('Updated again! The c object');// updates global object!
		console.log(this);//returns c object
	}
}
c.log();
// in the case that 'this' is used in the method of an object, it will return that object
```

arguments and SPREAD
1. hoisting works on arguments of a function as well
2. js now supports default paramters as well
3. console.log(arguments); // shows 'array-like' array of the arguments
	* arguments.length === 0

```javascript
function greet(firstname, lastname, language){
	console.log(firstname);
	console.log(lastname);
	console.log(language);
}
greet();//logs undfined for all 3

// now we have default values available
function greet(firstname, lastname, language='en'){...}

function greet(firstname, lastname, language, ...other){}
// all other args gets wrapped up in other
```

Function Overloading


# Syntax Parsers
1. goes through code char by char to decide if code is valid syntax

DANGEROUS ASIDE
Automatic Semicolon Insertion (always put your own semicolons)
--------------------------
```javascript
function getPerson() {
	return                     // semicolon is automatically added after return
	{ firstname: 'Tony' }
}
console.log(getPerson());// returns undefined
```
---------------------------

# White Space
1. These are both valid
2. Use whitespace to add good comments
```javascript
var firstname, lastname, language;

var 
	// first name of person
	firstname,

	// lastname of person
	lastname,

	// the language used, 'en' or 'es'
	language;
```

# Immediately Invoked Function Expressions
```javascript
// function statement
function greet(name) { console.log('Hello ' + name); }

// function expression
var greetFunc = function(name) { console.log('Hello ' + name); };

// IIFE
var greeting = function(name) { console.log('Hello ' + name); }();

function(name) { console.log('Hello ' + name); }// this line on its own will error, requires function name

(function(name) { console.log('Hello ' + name); });// runs fine
!!!!!js engine knows that anything inside of () is an expression!!!!!
(function(name) { console.log('Hello ' + name); }('Jacky'));// runs fine
(function(name) { console.log('Hello ' + name); })('Jacky');// runs fine
```

# Understanding Closures
1. Execution context has 'closed in' all the variables needed that were taken from the stack
   even when they were originally created in a different execution context.
2. The javascript engine creates the closure. We are simply using this feature of JS

```javascript
function greet(whattosay) {
	return function(name) {
		console.log(whattosay + ' ' + name);
	}
}
greet('Hi')('Jacky');
var sayHi = greet('Hi');
sayHi('Tony');

function buildFunctions() {
	var arr = [];
	for (var i = 0; i < 3; i++) {
		arr.push(
			function() {
				console.log(i);
			}
		)
	}
	return arr;
}
var fs = buildFunctions()/;
fs[0]();
fs[1]();
fs[2]();
// This will print out 3 3's
// Like asking three children how old their father is. They won't say what age he was when they were born. 
// All will give the same answer.

// Fix this by adding an execution context that stores the variable at function creation time.
	for (var i = 0; i < 3; i++) {
		arr.push(
			(function(j) {// j is stored here by closure!
				return function() {
					console.log(j);
				}
			}(i))
		)
	}
```

## Framework Aside: Function Factories (these use closures)
```javascript
function makeGreeting(lang) {
	return function(first, last) {
		if (lang === 'en') {
			console.log('Hello ' + first + ' ' + last);
		}
		if (lang === 'es') {
			console.log('Hola ' + first + ' ' + last);
		}
	}
}
// key is running makeGreeting twice, each with its own execution context
var greetEnglish = makeGreeting('en');
var greetSpanish = makeGreeting('es');

greetEnglish('John', 'Doe');
greetSpanish('John', 'Doe');
```

# Closures and Callbacks
-----------------------
```javascript
function sayHiLater() {
	var greeting = 'Hi!'; // This is kept in scope via closure
	setTimeout(function() {
		console.log(greeting);
	}, 3000);
}
sayHiLater();
```

1st class functions just means that you can use it like other types. As in passing them as args.
// jQuery uses function expressions and first-class functions
//$('button').click(function() {
//});

# Callback Function: a func you give another func, to be run when other func is finished
--------------------------------------------------------------------------------------
```javascript
function tellMeWhenDone(callback) {
	var a = 1000; 
	var b = 2000;
	callback();
}
tellMeWhenDone(function() { console.log('I am done!'); })
tellMeWhenDone(function() { console.log('All done...'); })
```

# call(), apply(), bind() 
-----------------------
All functions have these 3 methods, related to 'this'
var person = {
	firstname: 'John',
	lastname: 'Doe',
	getFullName: function() {
		var fullName = this.firstname + ' ' + this.lastname;
		return fullname;
	}
}
var logName = function(lang1, lang2) {
	console.log('Logged: ' + this.getFullName());
	console.log('Arguments: ' + lang1 + ' ' + lang2);
	console.log('---------------');
}// could bind here .bind(person);

var logPersonName = logName.bind(person);// sets 'this'
logName();
logPersonName()'en;

logName.call(person, 'en', 'es'); //Executes function with the first arg as 'this'
logName.apply(person, ['en', 'es']); // only difference between apply and call is that it takes an array

(function(lang1, lang2) {
	console.log('Logged: ' + this.getFullName());
	console.log('Arguments: ' + lang1 + ' ' + lang2);
	console.log('-----------');
}).apply(person, ['es', 'en']);


// Function Borrowing
var person2 = {
	firstname: 'Jane',
	lastname: 'Doe'
}

person.getFullName.apply(person2);// Jane Doe

// Function Currying: Creating a copy of a func but with some preset params
function multiply(a, b) {
	return a*b;
}
var multiplyByTwo = multiply.bind(this, 2);// permanently set 'a' to 2
console.log(multiplyByTwo(4));//8

var multiplyByTwo = multiply.bind(this, 2, 2);// permanently set 'a' and 'b' to 2
// even if you call multiplyByTwo with args


//Functional Programming
---------------------------------------------------
1st class functions: 

function mapForEach(arr, fn) {
	var newArr = [];
	for (var i=0; i<arr.length; i++) {
		newArr.push(
			fn(arr[i])
		)
	};
	return newArr;
}
var arr1 = [1,2,3];
console.log(arr1);
var arr2 = mapForEach(arr1, function(item) {
	return item*2;
});
console.log(arr2);
var arr3 = mapForEach(arr1, function(item) {
	return item>2;
});
console.log(arr3);

var checkPastLimit = function(limiter,item) {
	return item > limiter;
}
var arr4 = mapForEach(arr1, checkPastLimit.bind(this, 1));
console.log('arr4');

var checkPastLimitSimplified = function(limiter) {
	return function(limiter, item) {
		return item > limiter;
	}.bind(this, limiter);
};

var arr5 = mapForEach(arr1, checkPastLimitSimplified(1));
console.log(arr5);


# Underscore.js (annotated source code available) lodash is a optimized version
--------------------------------------------------------------------------------
```javascript
var arr6 = _.map(arr1, function(item) {return item*3});
console.log(arr6);
var arr7 = _.filter([2,3,4,5,6,7], function(item) { return item%2 === 0; });
console.log(arr7);
```
Classical vs Prototypal Inheritance
Classical: Verbose 
Prototypal: Simple

# Understanding the Prototype
All objects have a prototype prop (.__proto__) but don't ever set it
Prototype Chain
```javascript
var person = {
	firstname: 'Default',
	lastname: 'Default',
	getFullName: function() {
	return this.firstname + ' ' + this.lastname;
}
var john = {
	firstname: 'John',
	lastname: 'Doe'
}
john.__proto__ = person;//will now give john access to getFullName, this refers to original call object

Base Object/Prototype is Object{}
var a = {};
var b = function() {};
var c = [];
a.__proto__ // Object{}
b.__proto__ // function Empty() {}
c.__proto__ // []
c.__proto__.__proto__ // Object{}
```

# Reflection and Extend
----------------------------------------------------------------------------
An object can look at itself, listing and changing its props and methods. Can .extend()
```javascript
for (var prop in john) {
	// will avoid printing getFullName
	if (john.hasOwnProperty(prop)) {
		console.log(prop + ': ' + john[prop]);
	}
}

var jane = {
	address: '111 Main St.',
	getFormalFullName: function() {
		return this.lastname + ', ' + this.firstname;
	}
}

var jim = {
	getFirstName: function() {
		return firstname;
	}
}

\_.extend(john, jane, jim);

console.log(john);// now has all the keys/methods from jane and jim
```

# Building Objects
----------------------------------------------------------------------------
Function constructors, 'new', and the history of javascript
Function constructors: a normal function that is used to construct objects
					The 'this' var points to a new empty object and that 
					obj is returned from the func automatically

```javascript
function Person() {
	console.log(this);// will be empty object
	this.firstname = 'John';
	this.lastname = 'Doe';
	// returns 'this' object automatically

	//but
	return {greeting: 'I got in the way'};// john will now be this object instead
}

var john = new Person();// marketing, 'look. js is like java. you should use it hehe.'
console.log(john);
```

'.prototype': prop of function which is only used by the 'new' operator
----------------------------------------------------------------------------
.prototype is not the prototype of the func constructor, it is the prototype of the objs created by that func constructor

'new' creates an empty object and makes its prototype the Person.prototype
```javascript
function Person(firstname, lastname) {
	this.firstname = firstname;
	this.lastname = lastname;
}

Person.prototype.getFullName = function(){
	return this.firstname + ' ' + this.lastname;
}

var john = new Person('John', 'Doe');
console.log(john);
john.getFullName();// This works!

Person.prototype.getFormalFullName = function() {
	return this.lastname + ', ' + this.firstname;
}
john.getFormalFullName();// This works too!
```

Best Practice: Use capitalized name for function constructors
----------------------------------------------------------------------------
```javascript
'John'.length
	is the same as
a = new String('John')
a.length
```

Conceptual aside: Built-in function constructors
```javascript
var a = new Number(3);
// Number is the f c. Makes object. a.prototype => Number.prototype... 
var a = new String('John');
// Looks like you're creating a primitive but makes Object (String())
var a = new Date('3/1/2015');
// Adjusts all strings
String.prototype.isLengthGreaterThan = (limit) => {
	return this.length > limit;
}
console.log('John'.isLengthGreaterThan(3));
//immediately callable for all strings!

Number.prototype.isPositive = () => {
	return this > 0;
}
3.isPositive();// Errors because 3 is actually primitive
var a = new Number(3);
a.isPositive();// this works

var a = 3;
var b = new Number(3);// creating object
a == b;// true
a === b;// false

// Moment.js is great for date/time. Possibly replace Date()
var c = Number('3');// conversion
a === c;// true


Dangerous Aside: Arrays and for... in
Arrays are objects in js!

var arr = ['John', 'Jane', 'Jim'];// calling new Array()
for (var prop in arr) {
	console.log(prop + ': ' + arr[prop]);
}
Array.prototype.myCustomFeature = 'cool!';//above for loop will print out an additioinal object
```
//Log below
0: John
1: Jane
2: Jim
myCustomFeature: cool!


## Function Constructors and Object.create() end up doing basically same thing
Object.create and Pure Prototypal Inheritance (Kind of like a parent Class)
-------------------------------------------------------------------------------------
```javascript
var person = {
	firstname: 'Default',
	lastname: 'Default',
	greet: function() {
		return 'Hi ' + this.firstname;
	}
}
var john = Object.create(person);// empty object with prototype pointing at person defined above
console.log(john);
john.firstname = 'John';
john.lastname = 'Doe';

Polyfill: code that adds a feature which the engine may lack
if (!Object.create) {
	Object.create = (o) =>{
		function F() {}// Function constructor
		F.prototype = o;
		return new F();
	}
}
```

# ES6 and Classes
-------------------------------------------------------------------------------------
```javascript
class Person {
	constructor(firstname, lastname) {//like a func constructor
		this.firstname = firstname;
		this.lastname = lastname;
	}
	greet() {
		return 'Hi ' + firstname;
	}
}
var john = new Person('John','Doe');


class InformalPerson extends Person {
	constructor(firstname,lastname) {
		super(firstname, lastname);
	}
	greet() {
		return 'Yo ' + firstname;
	}
}
```


# Initialization (Hard coding like this is useful for starting out to code a project)
-------------------------------------------------------------------------------------
```javascript
var people = [
	{
		firstname: 'John',
		lastname: 'Doe',
		addresses: [
			'111 Main St.',
			'222 Third St.'
		]
	},
	{
		firstname: 'Jane',
		lastname: 'Doe',
		addresses: [
			'333 Main St.',
			'444 Third St.'
		],
		greet: () => 'Hello!'
	}
]


typeof, instanceof, and figuring out what something is
-------------------------------------------------------------------------------------
var d = [];
console.log(typeof d);// object
console.log(Object.prototype.toString.call(d));// array

function Person(name){
	this.name = name;
}

var e = new Person('Jane');
console.log(typeof e);
console.log(e instanceof Person);// true
console.log(typeof undefined);// undefined
console.log(typeof null);// object, a bug
z = () => {}
console.log(typeof z);//function
```

# Strict Mode (can put it in global/top of file and in function space/top of function)
-------------------------------------------------------------------------------------
```javascript
'use strict';// will force you to declare a variable before assigning it

var person;
persom = {};
console.log(persom);// will exist on the global object

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode
```


# Deep Dive into JQuery Source Code
-------------------------------------------------------------------------------------
```html
<html>
	<body>
		<div id="main" class="container">
			<h1>People</h1>
			<ul class="people">
				<li>John Doe</li>
				<li>Jane Doe</li>
				<li>Jim Doe</li>
			</ul>
		</div>
		<script src="jquery-1.11.2.js"></script>
		<script src="app.js"></script>
	</body>
</html>
```
```javascript
// app.js
var q = $("ul.people li");
console.log(q);//jQuery.fn.init[3] array like

var q = $("ul.people").addClass("newClass").removeClass("people");
console.log(q);

// METHOD CHAINING: Calling one method after another, and each method affects the parent object
obj.method1().method2() where both methods end up with a 'this' variable pointing at 'obj'.

jQuery methods return 'this'
'this' points at jQuery object, not the prototype
```
# Section 9: Building our own Framework/Library
Greetr

```html
//index.html
<html>
	<head></head>
	<body>
		<div id="main" class="container">
			<h1>People</h1>
			<ul class="people">
				<li>John Doe</li>
				<li>Jane Doe</li>
				<li>Jim Doe</li>
			</ul>
		</div>
		<div id="logindiv">
				<select id="lang">
						<option value='en'>English</option>
						<option value='es'>Spanish</option>
				</select>
				<input type="button" value="Login" id="login" />
		</div>
		<h1 id='greeting'></h1>						
		<script src="jquery-1.11.2.js"></script>
		<script src="Greetr.js"></script>
		<script src="app.js"></script>
	</body>
</html>
```
```javascript
//Greetr.js   //immediately invoked function
// Create function scope
// add extra ; to close off any library that was imported previously
;(function(global, $) {
	var Greetr = function(firstName, lastName, language) {// G$ points to here!
		return new Greetr.init(firstName, lastName, language);
	}

	var supportedLangs = ['en', 'es'];// only exists in this function scope 
	// but closure allows it be seen by Greetr
	// not exposed to outside world, only available to Greetr.init
	var greetings = {
		en: 'Hello',
		es: 'Hola'
	}

	var formalGreetings = {
		en: 'Greetings',
		es: 'Saludos'
	}

	var logMessages = {
		en: 'Logged in',
		es: 'Inicio sesion'
	}	

	Greetr.prototype = {
		// These will be available to outside
		fullName: function() {
			return this.firstName + ' ' + this.lastName;
		},
		validate: function() {
			if supportedLangs.indexOf(this.language) === -1 {
				throw 'Invalid language';
			}
		},
		greeting: function() {
			return greetings[this.language] + ' ' + this.firstName + '!';
		},
		formalGreeting: function() {
			return formalGreetings[this.language] + ' ' + this.fullName();
		},
		greet: function(formal) {
			var msg;
			msg = formal ? this.formalGreeting : this.greeting()
			if (console) {
				console.log(msg)// Internet Explorer doesn't have console object
			}

			// 'this' refers to the calling object at execution time
			// makes the method chainable
			return this;
		},
		log: function() {
			if (console) {
				console.log(logMessages[this.language] + ': ' + this.fullName());
			}
			return this;
		},
		setLang: function(lang) {
			this.language = lang;
			this.validate();
			return this;
		},
		HTMLGreeting: function(selector, formal) {
			if (!$) {
				throw 'jQuery not loaded';
			}
			if (!selector) {
				throw 'Missing jQuery selector';
			}
			var msg;
			msg = formal ? this.formalGreeting() : this.greeting();

			$(selector).html(msg);

			return this;
		}
	};
	
	// Function Constructor
	Greetr.init = function(firstName,lastName,language){
		var self = this;
		self.firstName = firstName || '';
		self.lastName = lastName || '';
		self.language = language || 'en';
		self.validate();
	}

	Greetr.init.prototype = Greetr.prototype;

	global.Greetr = global.G$ = Greetr;// Allows us to do G$('') to create Greetr object!

}(window, JQuery));


// app.js
// gets a new object (the architecture allows us to not have to use the 'new' keyword here)
var g = G$('John', 'Doe');

// use our chainable methods
g.greet().setLang('es').greet(true).log();

// let's use our object on the click of the login button
$('#login').click(function() {
	// create a new 'Greetr' obj (let's pretend we know the name from the login)
	var loginGrtr = G$('John', 'Doe');
	// hid the login on the screen
	$('#logindiv').hide();

	// fire off an HTML greeting, passing the '#greeting' as the selector 
	// and the chosen language, and log the welcome as well
	loginGrtr.setLang($('#lang').val()).HTMLGreeting('#greeting', true).log();
})
```


// Bonus Lectures: Transpiled Languages: Typescript, ES6 (when it was still new and not used yet)
// Tool: Traceur
Transpile: convert the syntax of one programming language to another

ES6 Features: https://github.com/lukehoban/es6features




























