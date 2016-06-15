## Arrow functions

With ES6, apart from the regular function expression came a new function expression, which can be used to decalre a function, known as the arrow function expression. Apart, from the syntax being different,, an arrow function behaves pretty differently as compared to the traditional functions :

1. The `this` is lexically binded, which means they are always binded to the closest context they are declared in.
2. These are always anonymous functions ie., they can never have a name.
3. They don't have the [[Construct]] function, hence, they cannot be treated as constructor function, and these functions cannot be called using the `new` operator.
4. They do not have any prototype property.
5. Arrow functions doesnot provide with an `arguments` object of its own, so which means to access extra values passed to the arrow function, we need to rely on the `rest` parameters.
6. Their cant be any duplicate name in the parameters, during the declaration of the arrow function.
7. The value of `this` cannot be changed, during the execution of the arrow function or within the code.

### Syntax

The syntax to declare an arrow function is list of variables on the left hand side with or without parantheses `()`, followed by the arrow sign `=>` followed by the body of the function with or without curly braces `{}`. There can't be any line break in between parameter declaration and the arrow sign. Let's cover each decalration step by step, and how it can look in prior to ES6.

> parameters `=>` body of function

##### Parameter decalration

During, the parameter declaration for arrow functions, there can be 0 or more variables. If there are more two parameter in function declaration, then the variables need to be enclosed within parantheses `()`, if there is a single parameter then the parantheses `()` can be omitted, and if no variables are decalared, then an empty parantheses need to be used `()`.

Let's look at the different ways of decalring parameter of arrow function.

````javascript
var foo1 = () => {return 1;}      // without any parameters

var foo2 = a => {return a*2;}     // with single parameter

var foo3 = (a,b) => {return a+b;} // with muliplte parameter
````

To get a view of how the above code might look in ES5, or in terms of traditional function decalration can be viewed below.

````javascript
var foo1 = function() {
    return 1;
}

var foo2 = function(a) {
    return a*2;
}

var foo3 = function(a,b) {
    return a+b;
}
````

##### Body declaration

The body of the arrow needs to *enclosed in a curly brace if there more than one line of code execution*, and if the there is only a single line of code execution, then the curly braces can be omitted. Let's take the funciton `foo2`from previous examples and modify it.

````javascript
var foo2 = a => {return a*2;}       // braces for single line

var foo4 = a => return a*2;         // braces is optional for single line code

var foo5 = a => {                   // braces is must for multiline
    var x = 1;
    return a = x+1;
}

var foo6 = a                        // Syntax error
=> a * 2;

var foo7 = a                        // Syntax error
=> { return a * 2}

var foo8 = a =>                     // Ok
{
    return a + 4;
}
````

##### Return in arrow function

You must have noticed that a lot of `return` are statements are being used in arrow functions examples. Now, saying that it doesnot mean that writing the `return` is neccessary, however, in the propsoal of ES6 for arrow function, it is mentioned that

> `return` is implicit under some circumstances

An implict `return` will only be used if there is no block, which means if curly braces `{}` is used declaring an arrow functions body, then the JS will not use an implicit `return`, and will only depend on the implicit `return` statement, mentioned in the block, however, if body is not containing a block, and the body doesnot return anything, then an implicit `return` will used by JS. Let's have a look at the example at what it means.

````javascript
// returns:2
// because there is no block statment, hence JS implicit `return`
(a => a*2)(1);          

// returns:2
// because there is no block statment,but we are explicitly returning
(a => return a*2)(1);

// returns:undefined
// because there is a block statment,and JS depends on explicit `return`
(a => {a*2})(1);

// returns:2
// because there is a block statment,and we are explicitly returning
(a => {return a*2})(1);

// returns:undefined
// because there is a block statment,and JS depends on explicit `return`
(a => {})();

// returns:undefined
// because even we may represent as object, the body considers it as
// a block statement, and depends on explicit `return`
// [read `labels` in JS](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label)
(a => {id:1})();

// returns:{id:1}
// because there is no curly braces, but parantheses, which is considered as
// expression, hence it implicitly `return` a the object
(a => ({id:1}))();
````

> If you are executing these codes in **dev tool console**, you might be wondering, why `undefined` are being returned for arrow function body where block exists, that is because `function` implicitly returns `undefined`, this is beacuse, the dev tools will print the result of whatever we type, which means it will try to evaluate whatever we write, 
an assingment never `return` any value, because of which the evaluation returns `undefined`, however, if we assign a value to variable already decalred, the value assinged to the variable is used as the return value, and for the same reason `function` also `return` `undefined`. 

**Note** : Now while evaluation, there can be some gotchas, to know more detail you can look into the following [github issue](https://github.com/kentcdodds/ama/issues/145), and [stackoverflow question](http://stackoverflow.com/questions/22844840/why-does-javascript-variable-declaration-at-console-results-in-undefined-being?lq=1).

##### How to call an arrow function

There are a couple of methods by which we call these function, since, the arrow function are anonymous functions, to call them 
* We can either assign the arrow function to a named variable, which can be later used to invoke.
````javascript
var sum = (a,b) => a+b;
sum(1,2);               // 3
````
* We can also use it as IIFE (Immediately Invoked Function Expression), which will is executing right when it declared, and can be referred ot as IIAF (Immediately Invoked Arrow Function)
````javascript
((a,b) => a+b)(1,2);    // 3
````
* We can also pass it as a parameter to the function, which later on goes to invoke it, when required.
````javascript
setTimeout(() => {console.log(1)},100);
````
* There are three other ways which can used to call the arrow functions which is the `call`, `apply` and `bind`. But, to dig into that we need to first swim through 🏊🏼 the ocean of `this`, and understand how `this` behaves and how different it is in the arrow functions from the regular functions.
