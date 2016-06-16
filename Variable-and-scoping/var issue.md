## The problem with `var`

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

The way variables are hoisted in JS also leads to an expected output, lets look at some more examples of how hoisting is done and how it can effect your code!

````javascript
var a = 20;

for(var i=0;i<5;i++)
{
    var a = 10;
}
console.log(a);         // 10
````

With the above code, many might be expecting that the value of `a` will be 20, thinking it should remained, unchanged, however it prints `10` 😳. Let's understand the why behind it.

Their is two reason behind the value being altered to `10`,

1. variable hoisting
2. lack of block scope

But what does it means? 😫

##### Hoisting 

This can be considered as the look up mechanism adopted by JS, where the JS will analyze our entire code, and look for variable declarations within our code, once it finds a declaration, its job is to declare the variable, in it's nearest `function scope`, but, before declaring the variable blindly, it will first find its nearest `function scope` and then it will try to search if that variable is declared within that scope or not, if it doesn't find the variable declared it will declare the variable, else it will discard the duplicate variable decalration.

##### Lack of block scope

In JS prior to ES6, there was only `function scope`/ `global scope`, which means any variable declared or modifed within a `block scope` will be declared or is modifying the variable in the nearest `function scope` or `global scope`. In layman terms, during variable decalration it will first find the nearest scope, check if the variable is declared is or not, and if it finds then it will discard the duplicate declaration, and during modification of value, it will first, look up the varaible in the nearest scope, if it find the variable declaration, then it directly modifys the variable else it decalres the variable within that scope and then assign the value! Horray!!🎉

Now, lets look whathow the above two things give the output as `10`!! The above code could be broken into like this
````javascript
var a;
a = 20;

for(var i=0;i<5;i++)
{
    // var a = 10;
    // The above line was broken into two parts,
    // 1. variable decalration var a;
    // 2. assignment of 10 to a = 1
    // As we mentioned that there is no block scope, which means the nearest scope is?
    // global scope! and during the decalration it already finds that a is declared,
    // in the global which is why the declaration part was ommited.
    
    a = 10;
    
    // and during the assiginment, it again searches for the variable within the?
    // global scope! and hence, the global a is modified!
}
console.log(a);
````

Ok, now that must be relief!! 😌. But, wait!! 😐, you must be thinking I forgot to mention `function scope`!! 😣
Let's look into `function scope`.

##### Function Scope

Consider the code below.

````javascript
var a = 'global scope';

function foo()
{
    var a  = 'function scope';
}
foo();

console.log(a);         // ????? 'Global scope'!
````

You must be thinking, wooaaaa! 😱 What just happened their! Well, `function scope` happened! When we decalred a new function `foo` a new set of scope was created around that `function`, meaning, variables declared within the `foo` will be within the function foo itself, because remember the hoisting rule? It declares a variable in the nearest `function scope`/`global scope`, well, the `foo` just created its own scope and the variable `a`, was decalred within the scope of `foo`. Let's zoom into the function a little to know what I mean, and the code below can clear some 👻!

````javascript
var a = 'global scope';

function foo()
{
    console.log(a);                     // undefined
    var a  = 'function scope';
    console.log(a);                     // function scope
}
foo();

console.log(a);         // ????? 'Global scope'!
````

As, I mentioned, the `foo` creates its own scope, because of which the `var a = 'function scope'` was broken into two part

1. `var a;` where the value of `a` was `undefined` within the function scope, hence when we try to print `a` it gave `undefined`.

2. `a = 'function scope'` where the `a` lying within the scope of `foo` was assigned the value `function scope` and thus the second line printed `function scope` and the global `a` was never touched!. 

With this giving an introduction on hoisting and scoping, lets look into another issue, which `var` seems to impose!

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

* * *

**Next Chapter : [`let` declaration](https://github.com/anirudh-modi/JS-essentials/blob/master/Variable-and-scoping/let.md)**
