## 1. `let` and `const` decalration

The `let` and `const` declarations essentially included block scoping technique in javascript.

#### 1.1 The problem with `var`
Earlier in ES5 and prior, there was no block scoping and the varaibles declared with `var` always gets hoisted in the nearest function scope found. Blocks like `if`, `for`, `while`..etc never created a scope dedicated to themselves, leaving the variable declared within the nearest function scope it lies.

``````javascript
var functionScope = false;

if(functionScope)
{
    var blockScope = true;
}

console.log(blockScope); //undefined
``````

For those familar with block scope must be expecting that `console.log(blockScope)` must give an error, because the variable was never declared within the scope, and we are trying to access the variable.

However, because of the fact that, variable are hoisted in the nearest function scope in Javascript, the above code looks like this:

````javascript
var functionScope = false,
    blockScope; // Since the 'var' declares the varibale to the nearest
                // function scope, It never throws any error, however since we 
                // never entered if condition, the variable was never assigned
                // any value, and by default 'undefined' is assigned

if(functionScope)
{
    blockScope = true;
}

console.log(blockScope); //undefined 
````

The way variables are hoisted in JS also leads to an expected output, lets look at the piece of code below:

````javascript
var funcArray = [];

for(var i=0;i<5;i++)
{
    funcArray.push(function()
    {
        console.log(i);
    });
}

funcArray[1](); //Gives an output of 5
````

The reason above codes gives an output of **5** and not an output of _1_, is because of the fact that every the for loop increments the value of 'i' it increments the value of the i which lies in the function scope, and at the time of the execution the we are printing i, which points to the updated value, hene the output of 5, the above code transpiles to :

````javascript
var funcArray = [],i;

for(i=0;i<5;i++)
{
    funcArray.push(function()
    {
        console.log(i);  //When the function is exec it points to the i 
                         //declared outside the for loop
    });
}
funcArray[1](); 
````

To solve the above issue, we can use IIFE, which creates a closure around the each instance of i and stores each i along with the function.

````javascript
var funcArray = [],i;

for(i=0;i<5;i++)
{
    funcArray.push((function(val)
    {
        return function()
        {
            var j = val;
            console.log(j);
        }
    })(i));
}
funcArray[2](); //2
````


But, with ES6 we have `let` and `const` which enables us to create block scope. Lets look into both the variable.

#### 1.2 `let` decalration 

The `let` decalration allows only a single declartion of a variable with a particular name within a given scope, which means once a variable is defined irrespective of we are using `var`, `let`, `const`, in the same scope we cannot redecalre that same variable using `let` or `const`. But if the variable is declared using `var`, we can redecalre it only using `var`

The `let`syntax allows us to reassign a value to a variable, however it doesn't allow to redeclare a variable with same name, within the same scope. Let's See an example on what it means.

````javascript
var b;

let a = 1,c = 'outsideIf';

a = 2;

console.log(a); //2

var a = 3; // Syntax error, because redeclaration is not allowed

let b = 3; // Syntax error

if(a === 1) // the 'if' create a new scope around the if
{
    let a = 3, x = 10;  // this creates a new variable but 
                        //within the ````if```` scope

    console.log(a) // 3

    var c = 'insideIf'; // Syntax Error because ````var```` never creates
                        // a scope 
                        // hence, it tries to create the a variable in the 
                        // scope outside
                        // where 'c' is defined using ````let````, hence 
                        // syntax error
}

console.log(a); // 2

console.log(x); // Reference err: Because x is created in 'if'
                // scope & cant be access from outside
````

A new scope can be created around a `let` variable by putting a curly braces.

````javascript
var a = 1;

{
    let a = 2;

    console.log(a); //2
}

console.log(a); //1
````

**Using `let` in `for loops`**

The `let` can be used effieciently to solve the problem stated in 1.1, since it creates a new scope for ever block, hence each loop will have its own instance of a variable. lets look how `let` works in a `for` loop.

````javascript
var resultArr = [];

for(let i=0;i<5;i++)
{
    resultArr.push(function()
    {
        console.log(i);
    });
}

resultArr[2](); //2
````

The above internally transpiles to 

````javascript
var resultArr = [];

{
    let i=i+1; //incrementing i for each loop 

    {                       
        let j=i;
        resultArr.push(function()
        {
            console.log(j);
        });
    }
    // the above block will created five times hence each 
    //block will have its own scopic j
}

resultArr[2](); //2
````

The `let` also works the same way with `for..in` and `for..of` loops.


#### 1.3 `const` declaration

Variables declared using `const` are considered constants, meaning their values cannot be changed once set. because of which, it becomes important that while declaring a `const`, we need to initialize the variable too, signifying a `const` is a read only variable  `const` can used as below :

````javascript
const a = 1;

const b; //missing intialization error

a = 2; // "a" is read-only TypeError error
````


Constants are not a restriction on the value itself, but on the variable's assignment of that value. In other words, the value is not frozen or immutable because of `const`, just the assignment of it. If the value is complex, such as an object or array, the contents of the value can still be modified:

````javascript
const a =[];
a.push(1);
console.log(a); //[1]

a = 1; //Type error because the assignment of the variable is tried ot be changed
````

The a variable doesn't actually hold a constant array; rather, it holds a constant reference to the array. The array itself is freely mutable, hence we can easily changes the value within array/object.

**Using `const` in `for loops`**

A `const` can be used in `for..in` and `for..of` loops, because for every loop a new scope is created and within the scope a constant variable is defined, which is not governed by any outer scoped variable, which we occurs when we use `const` in the general `for loop`

````javascript
for (const x in obj) {
  // ..
}

//think of it like this: each loop iteration redeclares, like:

const x = obj[ .. ]
````

#### 1.4 Temperal Dead Zone

A variable declared with either `let` or `const` cannot be accessed until after the declaration. Attempting to do so results in a reference error.

````javascript
console.log(a);  // Reference error
console.log(b);  // Reference error

let a = 1;
const b = 2;
````

When a JavaScript engine looks through an upcoming block and finds a variable declaration, it either hoists the declaration to the top of the function or global scope (for `var`) or places the declaration in the TDZ (for `let` and `const`). Any attempt to access a variable in the TDZ results in a runtime error. That variable is only removed from the TDZ, and therefore safe to use, once execution flows to the variable declaration.

To see how TDZ works lets watch the below example:

````javascript
console.log(typeof a); // Reference error

let a = 1

console.log(typeof b); // undefined

if(true)
{
    console.log(typeof b); //Reference error

    let b = 1;
}
````

The reason why accessing the constant a gives a refernce error is because a lies within the global scope, and hence when engine looks up and declares a in TDZ, however, the variable b is not declared within the global scope and hence it doesnot lie in the TDZ, it is defined within the block scope and hence accessing the variable b outside the block, doesnot give a Reference error, but accessing the variable b within the block scope of `if` before its declaration will give a Reference error.