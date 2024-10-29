# SWE 101 : Programming Terms Explained in simplest form

![banner](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pompixb3i6q6c39ac5gb.png)

What is idempotent again ?

Closure, Memoization, Idempotence : Decoding and understanding programming terms one by one in simplest definition

All code written in this javascript, but dont worry about the language, syntax is kept super simple. For practise, you can implement them in your favourite programming language.

Lets start with first class functions

# First Class Function

A programming language is said to have first class functions if it treat its functions as first class citizens

What are first class citizens : something that can be

- passed as an argument
- returned from a function
- assigned to a variable

Whatever satisfies the above 3 properties in you programming language can be called as first class citizen. Lets take a look at with examples

## Assigning Function to a variable
```js
function square(x) {
    return x * x
}

// Assigned to another variable
let f = square

console.log(square(5))
console.log(f(5))
```

## Passed as an Argument aka High order functions
```js
// sqr the the passed square function
function my_map(sqr, args) { 
   let result = [] 
    
   for (let i = 0; i < args.length; i++) { 
       // the passed function is used here
       result.push(sqr(args[i])) 
   } 
   return result; 
}

// square function is passed as argument
let squares = my_map(square, [1, 2, 3, 4, 5, 6, 7]) 
console.log(squares)
```

## Function as Return Type
```js

function logger(msg) { 
   function log_message() { 
    console.log("Log : " + msg) 
   } 
   //  this is a function, returning from parent functions
   return log_message 
} 
  
logHello = logger("hello") 
logHello()
```

Before moving on, please read the above and try to understand the concept, it would be helpfull

# Closure

_They are similar to functions returned from another function but captures the internal state of parent function at the time of invoked._

- A closure is a record storing a function together with an environment, a mapping associating each free variable of the function with the value of storage location to which the name was bound when then the closure was created. ( Kinda formal , read below and look at code snippet )
- A closure, unlike a plain function, allows the function to access those captured and closed variables when the function is invoked outside the scope.

```js
function outer_function() {
   message = "hello world"
   function inner_function() {
       console.log (message) // Look at point 2 from definition
   }
   return inner_function()
}

// invoked from outside
outer_function()
```

### Another Example of Closure

```js
function outer_function(msg) {
   message = msg
   function inner_function() {
       console.log (message)
   }
   return inner_function
}

let func = outer_function("Hello World")
func()
```

# Immutable and Mutable
```js
// strings in js are immutable 
// they cannot be changed once initialised
let name = "uday Yadav"
name[0] = "U";
// this makes not difference
console.log(name);
// still small case 'u'
console.log(name[0]); 

// array in js is mutable 
// they can be changed once created
let data = [0,2,3,4];
data[0] = 1;
console.log(data);
```

# Memoization

Some operations are expensive to preform, so we store the results of them in some form of temporary storage and when required to recalculate, first find them in the temporary storage.

```js
let cache = {}

function expensive_compute(data) {

   if (cache.hasOwnProperty(data)) {
       console.log("answer cache : "+cache[data])
       cache[data] = data*data
       return;
   }
   cache[data] = data*data
   console.log("answer : "+cache[data])
}

expensive_compute(4)
expensive_compute(10)
expensive_compute(4)
expensive_compute(16)
expensive_compute(10)
```

# Idempotence
The property of certain operations in mathematics and computer science, that can be applied multiple times without changing the result without initial application

A good example of understanding an idempotent operation might be locking a car with remote key.

```js
log(Car.state) // unlocked

Remote.lock();
log(Car.state) // locked

Remote.lock();
Remote.lock();
Remote.lock();
log(Car.state) // locked)
```

- `lock` is an idempotent operation. Even if there are some side effect each time you run lock, like blinking, the car is still in the same locked state, no matter how many times you run lock operation.

- **NON-IDEMPOTENT**: If an operation always causes a change in state, like POSTing the same message to a user over and over, resulting in a new message sent and stored in the database every time, we say that the operation is NON-IDEMPOTENT.

- **NULLIPOTENT**: If an operation has no side effects, like purely displaying information on a web page without any change in a database (in other words you are only reading the database), we say the operation is NULLIPOTENT. All GETs should be nullipotent.

To understand idempotence more, refer this stackoverflow thread : [what is idempotent operation](https://stackoverflow.com/questions/1077412/what-is-an-idempotent-operation)

# Ephemeral
**synonyms to temporary**

# Anonymous Functions
Function without a name, also known as lambda function in 
Python

```js
let arr = [1, 2, 3];
let mapped = arr.map(x => Math.pow(x, 2));
// x =>  is a function without a name
console.log(mapped);
```

# Predicate

Functions that return true or false depending on the input. They usually start with _is_
```js
class Animal {
   constructor(_type) {
       this.type = _type;
   }
}

function makeSound(animal) {
   if (isCat(animal)) {
       console.log(" MEOW ! ");
       return;
   }
   console.log(" NOT CAT ! ");
}

function isCat(animal) {
   return animal.type === 'Cat';
}

let newCat = new Animal('Cat');
makeSound(newCat);
```

# Parsing and Stringify
- Parsing : converting string to some object
- Stringify : converting some object to string

```js
let data = {
   "name": "Uday Yadav",
   "Gender": "Male"
}

let str = JSON.stringify(data)
console.log(str + "|" + typeof str)

let dataReturns = JSON.parse(str)
console.log(dataReturns + "|" + typeof dataReturns)
```

More about me : https://uday-yadav.web.app/