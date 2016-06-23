## Temperal Dead Zone

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

Let's look at another example of TDZ.

````javascript
let a = 1;
(function(){
    console.log(a);
    let a = 2;
})()
````

One might think the output must be `1` however, it will again throw a reference error, because the moment a function was defined a new scope was created, the engine looked up its variable declaration, and when it found `a` it has put the `a` in a TDZ and when we are trying to access the `a` within the function scope, the variable `a` was not intialized, because of which a reference error was thrown.

You might be wondering then why does not `var` declarations throw error, that is because, when variable with `var` are declared, it assigns a value if `undefined` while creating a list of variables within the scope, and for (`let` and `const`) value is not assigned until it the control flow, reaches the code, where that variable is declared. And for `let` if no value is assigned during its decalration `let a;` then, a default value of `undefined` is assinged.
- - -
**Previous Chapter : [`const` declaration](https://github.com/anirudh-modi/JS-essentials/blob/master/ES2015/Variable-and-scoping/const.md)**

**[Functions](https://github.com/anirudh-modi/JS-essentials/blob/master/ES2015/Functions/Functions.md)**
