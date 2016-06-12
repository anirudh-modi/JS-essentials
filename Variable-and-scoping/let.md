## `let` decalration 

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

* * *

**Previous Chapter    : [The problem with `var`](https://github.com/anirudh-modi/JS-essentials/blob/master/Variable-and-scoping/var%20issue.md)**

**Next Chapter        : [`const` declaration](https://github.com/anirudh-modi/JS-essentials/blob/master/Variable-and-scoping/const.md)**
