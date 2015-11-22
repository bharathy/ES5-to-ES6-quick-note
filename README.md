# ES5-to-ES6-quick-note (WIP)

#####Fat Arrow :
Arrows are a function shorthand using the => syntax. They are syntactically similar to the related feature in C#, Java 8 and CoffeeScript. They support both expression and statement bodies.

```javascript
// ***** ES5 Function *****
var square = function (n) {
	return n * n;
};
var numbers = [ 1, 2, 3];
var squares = numbers.map(square);
console.log(squares);

// ***** ES6 Fat arrow *****
var square = (n) => n * n;
var numbers = [ 6, 7, 8 ];
var squares = numbers.map(square1);
console.log(squares);
```
#####Lexical "this" :
Unlike functions, arrows share the same lexical this as their surrounding code.
Following ES5 example returns "undefined skill" because this context is missing, otherwise say JS scope issue due to closure.
With fat arrow (=>) lexical scope exist.
```javascript
// ***** ES6 Lexical this *****
function ClassEmployee() {
	var employee_es5, employee_es6;
	// ES5 has scope this issue on closure
	employee_es5 = {
		name: 'Bharathy',
		skillset: [ 'JS', 'Java', 'CSS' ],
		printSkillset: function () {
			this.skillset.forEach(function (skill) {
				console.log('%s have %s skill', this.name, skill);
			});
		}
	};
	employee_es5.printSkillset();

	// ES6 lexical scope this can be used
	employee_es6 = {
		name: 'Bharathy',
		skillset: [ 'JS', 'Java', 'CSS' ],
		printSkillset: function () {
			this.skillset.forEach((skill) => {
				console.log('%s have %s skill', this.name, skill);
			}
			);
		}
	};
	employee_es6.printSkillset();
}
ClassEmployee();
```
####Let :
While var creates a variable scoped within its nearest parent function, let scopes the variable to the nearest block, this includes for loops, if statements, and others.
```javascript
// ***** ES6 LET & CONST*****
function foo() {
	// below line will execute just fine due to variable hoisting
	console.log(x);
	// log(y) call fails due to block scope of let
	console.log(y);

	var x = 1;

	if (x === 1) {
		let y = 2;
		// execute well & log 2 due to if blcok scope
		console.log(y);
	}

	// log(y) call fails due to block scope of let
	console.log(y);
}

foo();
// will produce a ReferenceError because x is only defined (scoped) inside the foo() function
console.log(x);
```
####Const:
In ES6 a const represents a constant reference to a value (the same is true in most languages, in fact). In other words, the pointer that the variable name is using cannot change in memory, but the thing the variable points to might change. const follows the same new scoping rules as let! 
```javascript
const names = [];
names.push('Jammy');
// throws error saying read-only
console.log(names);
```
####For...of:
The forEach(), it can’t break out of this loop using with break, continue, and return.
The for–in loop is for looping over object properties.
The values assigned to index in for-in are the strings "0", "1", "2" and so on, not actual numbers.
The for–of loop is for looping over data—like the values in an array.
```javascript
// ***** ES6 Iterator & For...of *****
// Old way of looping array.
var myArray = [ 1, 2, 3 ];
for (var index = 0; index < myArray.length; index++) {
	console.log(myArray[index]);
}

// ES5 way of looping array with forEach in-built method
// cons: can’t break out of this loop using a break statement or return statement
myArray.forEach(function (value) {
	console.log(value);
});

// ES5 way of looping the object key value properties
// don't actually use for-in to loop array.
var obj = { a:1, b:2 };
for (var key in obj) {
	console.log(obj[key]);
}

// ES6 For ...of to loop an Array
for (var value of myArray) {
	console.log(value);
}
```
####Modules:
Modules can export multiple objects, which can be either plain old variables or JavaScript functions.
Couple of ways to import modules
Using import keyword
ex: `import { employees, getEmployee } from './employees';`
Using module keyword
ex: `module employees from './employees';`
#####Import:
The reason for those extra braces {} is that this syntax lets you export multiple variables.
```javascript
/ ***** ES6 Modules *****
// Using import keyword
// in employees.js class
class employee {
	constructor(name, id) {
		this.name = name;
		this.id = id;
	}
	toString() {
		return `* ${this.name} ${this.id} `;
	}
	isMatch(name, id) {
		return this.name === name || this.id === id;
	}
}

// exporting only required object or function
export var employees = [
	new employee({ name: 'Julie', id: '1008' }),
	new employee({ name: 'Rishmi', id: '1007' }),
	new employee({ name: 'Zefan', id: '1004' })
];

export function getEmployee({ name, id }) {
	return employees.filter(emp => emp.isMatch(name, id))[0];
}
// in app.js
import { employees, getEmployee } from './employees';
console.log(employees);

var employee = getEmployee({ name:'Rishmi', id:'1007' });
console.log(`Employee found: ${employee} `);
```
#####Module:
Basically imports everything from a module. So we need to specify the module name in calls.
```javascript
// Using module keyword
// in employees.js class
export class Employees {
	constructor(name, id) {
		this.name = name;
		this.id = id;
	}
	toString() {
		return `* ${this.name} ${this.id} `;
	}
	isMatch(name, id) {
		return this.name === name || this.id === id;
	}
	static getAllEmployees() {
		var employees = [
			new employee({ name: 'Julie', id: '1008' }),
			new employee({ name: 'Rishmi', id: '1007' }),
			new employee({ name: 'Zefan', id: '1004' })
		];
		return employees;
	}
	static getEmployee() {
		return Employees.getAllEmployees().filter(emp => emp.isMatch(name, id))[0];
	}
}

// in app.js
module employees from './employees';

console.log(employees.getAllEmployees());

var employee = employees.getEmployee({ name:'Rishmi', id:'1007' });
console.log(`Employee found: ${employee} `);
```

####Template Strings:
Template strings are string literals allowing embedded expressions. You can use multi-line strings and string interpolation features with them.
remember to use "GRAVE CHARACTER" - ` to start string interpolation.
use ${} for string placeholder
```javascript
// ***** ES6 String templating *****
employee_es6 = {
	name: 'Bharathy',
	skillset: [ 'JS', 'Java', 'CSS' ]
};

// ES5 String templating
console.log('%s have following skill: \n\t - %s \n\t - %s \n\t - %s',
	employee_es6.name, employee_es6.skillset[0], employee_es6.skillset[1], employee_es6.skillset[2]);

// ES6 String templating
console.log(`${employee_es6.name} have following skill:
	- ${employee_es6.skillset[0]}
	- ${employee_es6.skillset[1]}
	- ${employee_es6.skillset[2]}`);
```
####Variable destructuring:
```javascript
// ***** ES6 variable destructuring *****
function getEmployeeList() {
	return [
		'Adam',
		'John'
	];
}

//normal way
employees = getEmployeeList();
console.log(`The first employee is ${employees[0]} and the second employee is ${employees[1]} `);

// variable destructuring
var [ firstEmployee, secondEmployee ] = getEmployeeList();
console.log(`The first employee is ${firstEmployee} and the second employee is ${secondEmployee} `);
```
####Object Destructuring:
```javascript
// ***** ES6 Object Destructuring *****
function getEmployeeData() {
	return {
		name: 'sam',
		id: '1001',
		title: 'Javascript Developer'
	};
}

// normal way
var employee1 = getEmployeeData();
console.log(`The Employee Details are
	Name - ${employee1.name},
	Id - ${employee1.id},
	Title - ${employee1.title}`);

// Object destructuring
var { name: Name, id: Id, title: Title } = getEmployeeData();
console.log(`The Employee Details are
	Name - ${Name},
	Id - ${Id},
	Title - ${Title}`
);
```
####Argument Destructuring:
```javascript
// ***** ES6 Argument Destructuring *****
// normal way
function searchEmployee1(args) {
	console.log(`Searching for employee with
		name - ${args.name},
		id - ${args.id}`);
}
searchEmployee1({ name: 'Zac', id: '1002' });

// Argument destructuring
function searchEmployee2({ name: theName, id: theId }) {
	console.log(`Searching for employee with
		name - ${theName},
		id - ${theId}`);
}
searchEmployee2({ name: 'Zac', id: '1002' });
```
####Enhanced Literals:
we don't need to do foo: foo, can say just foo
we don't need to say methodName: function() {}, instead just ass the method.
dynamic keys can be added directly in object literal definition.
```javascript
// ***** ES6 Enhanced Literals *****
var title = 'Java Developer';

// ES5 object literals
var employee = {
	name: 'Rhea',
	title: title,
	printMessage : function () {
		console.log('Welcome to development world!');
	}
};
employee['uniqueId'+ new Date().getTime()] = Math.floor(Math.random() * 10000);
employee.printMessage();
console.log(employee);

// ES6 Enhanced object literals
var employee2 = {
	name: 'Rhea',
	title,
	printMessage() {
		console.log('Welcome to development world!');
	},
	['uniqueId_'+ new Date().getTime()]: Math.floor(Math.random() * 10000)
};
employee2.printMessage();
console.log(employee2);
```
####Parameter Magic:
#####Default parameter:
 -passing default argument in function parameter is possible in ES6.
```javascript
// ***** ES6 DEFAULT ARGS *****
// ES5 default arg assignment;
function printEmployeeIdByName(name, id) {
	id = id || 'not yet assigned';
	console.log(`${name}'\'s employee id is' ${id}`);
}
printEmployeeIdByName('Kai', '1005');
printEmployeeIdByName('Zara');

// ES6 default arg assignment;
function printEmployeeIdByName(name, id='not yet assigned') {
	console.log(`${name}'\'s employee id is' ${id}`);
}
printEmployeeIdByName('Kai', '1005');
printEmployeeIdByName('Zara');
```
####Rest Parameter:
Rest parameters are indicated by three dots … preceding a parameter. Named parameter becomes an array which contain the rest of the parameters.
ES5: But it’s not obvious that the function is capable of handling any parameters. W have to scan a body of the function and find arguments object.
```javascript
// ***** ES6 REST Parameter *****
// ES5 argument handling;
function logEmployeeSkills() {
	var skills = Array.prototype.slice.call(arguments);
	skills.forEach(function (skill) {
		console.log(skill);
	});
}
logEmployeeSkills('Javascript', 'Java', 'CSS');

// ES6 REST parameter
function logEmployeeSkills(...skills) {
	skills.forEach(function (skill) {
		console.log(skill);
	});
}
logEmployeeSkills('Javascript', 'Java', 'CSS');
```
####Spread Operator:
It allows to split an array to single arguments which are passed to the function as separate arguments.
The spread operator allows an expression to be expanded in places where multiple arguments (for function calls) or multiple elements (for array literals) are expected.
In the case of function calls this allows us to avoid the common practice of using the apply method to spread the values of an array across the parameters of the function:
The spread construct allows us to avoid this use of apply (which is good, since the main use of apply is to invoke a function in a particular context)
```javascript
// ***** ES6 SPREAD Parameter *****
// ES5 way of passing argument using apply() method.
function printEmployeeData(name, id, title) {
	console.log(name, id, title); // 1, 2, 3
}
var args = [ 'Sam', '1006', 'Fullstack Developer' ];
printEmployeeData.apply(null, args);

// ES6 SPREAD for passing arguements
function printEmployeeData(name, id, title) {
	console.log(name, id, title); // 1, 2, 3
}
var args = [ 'Sam', '1006', 'Fullstack Developer' ];
printEmployeeData(...args);
```
####Classes:
classes are a simple sugar over the prototype-based OO pattern. 
Classes support prototype-based inheritance, super calls, instance and static methods and constructors.
```javascript
// ***** ES6 CLASS *****
// ES5 way of creating class.
function CreateEmployee(name, id) {
	var employee = {
		name: name,
		id: id
	};
	employee.getName = function () {
		return this.name;
	};
	employee.getId = function () {
		return this.id;
	};
	return employee;
}

// ES5 another way of creating class.
var CreateEmployee = (function () {
	function CreateEmployee(name, id) {
		this.name = name;
		this.id = id;
	}
	CreateEmployee.prototype = {
		getName: function getName() {
			return this.name;
		},
		getId: function getId() {
			return this.id;
		}
	};
	return CreateEmployee;
})();
var newEmployee = new CreateEmployee('Mark', '1007');
newEmployee.getName();

// ES6 Class
class CreateEmployee {
	constructor(name, id) {
		this.name = name;
		this.id = id;
	}
	getName() {
		return this.name;
	}
	getId() {
		return this.id;
	}
}
var newEmployee = new CreateEmployee('Mark', '1007');
newEmployee.getName();
```
####Class Inheritance:
In ES5, we use object.create to achieve prototypal inheritance. super — to achieve such functionality in Javascript required the use of call or apply.
```javascript
// ***** ES6 CLASS INHERITANCE *****
// ES5 way of inheritance
var CreateEmployee = (function () {
	function CreateEmployee(name, id) {
		this.name = name;
		this.id = id;
	}
	CreateEmployee.prototype = {
		getName: function getName() {
			return this.name;
		},
		getId: function getId() {
			return this.id;
		}
	};
	return CreateEmployee;
})();

CreateEmployeeSkillset.prototype = Object.create(CreateEmployee.prototype);
CreateEmployeeSkillset.prototype.constructor = CreateEmployeeSkillset;

var CreateEmployeeSkillset = (function () {
	function CreateEmployeeSkillset(name, id, skills) {
		CreateEmployee.call(this, name, id);
		this.skills = skills;
	}
	CreateEmployeeSkillset.prototype = {
		printSkills: function getSkills() {
			console.log(this.name + ' has following skills ' + this.skills[0] + ',' +this.skills[1] + '.');
		}
	};
	return CreateEmployeeSkillset;
})();

var employee = new CreateEmployeeSkillset('Ryan', '1009', ['java','js']);
employee.printSkills();

// ES6 way of inheritance
class CreateEmployee {
	constructor(name, id) {
		this.name = name;
		this.id = id;
	}
	getName() {
		return this.name;
	}
	getId() {
		return this.id;
	}
}

class CreateEmployeeSkillset extends CreateEmployee{
	constructor(name, id, skills) {
		super(name, id);
		this.skills = skills;
	}
	printSkills() {
		console.log(this.name + ' has following skills ' + this.skills[0] + ',' +this.skills[1] + '.');
	}
}

var employee = new CreateEmployeeSkillset('Ryan', '1009', ['java','js']);
employee.printSkills();
```
####Classes : Static Method
Using static keyword we can define static method in ES6. Static method can be invoked without creating an instance.
```javascript
// ***** ES6 STATIC METHOD*****
// ES5 way of static method
var CreateEmployee = (function () {
	function CreateEmployee(name) {
		this.name = name;
	}
	return CreateEmployee;
})();

CreateEmployee.sayWelcome = function () {
	console.log('Welcome to ABC!');
};

CreateEmployee.sayWelcome();

// ES6 way of static method
class CreateEmployee {
	constructor(name) {
		this.name = name;
	}
	static sayWelcome() {
		console.log('Welcome to ABC!');
	}
}

CreateEmployee.sayWelcome();
```
Yet to do following:
####Map + Set + WeakMap + WeakSet:
####symbols:
####Promises:
####Proxy:
####Generators:




