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

* * *

**Next Chapter : [`let` declaration](https://github.com/anirudh-modi/JS-essentials/blob/master/Variable-and-scoping/let.md)**
