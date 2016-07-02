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

###### Why `undefined`?
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
 

#####  How `this` is different for arrow functions

> Note : An update for `this` will come soon, till then before proceeding I request you to learn `this` and return, for better understanding. You can go through these link by Kyle Simpson where he have in depth explained about `this` 
* [`this` Or That?](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20&%20object%20prototypes/ch1.md)
* [`this` All Makes Sense Now!](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20&%20object%20prototypes/ch2.md)

For arrow functions `this` does not exist, which means **unlike other regular function, the arrow functions does not have their own `this` binding**, instead, it starts looking up the scope of itself to find a `this`, and wherever the scope has a `this` binding of its own, that `this` is used as context when `this` is used within an arrow function.

> Note : In javascript only functions and global scope have a `this`.

So, now let's understand what happens when we start using `this` in arrow functions. Consider, the code snippet below.

````javascript
function foo() {
    console.log(this.a);        // global
}

foo.a = 'func';

var a = 'global';

foo();
````

The reason why above gave a console of `global`, is because we mentioned earlier that `this` context for a function, is not decided by where it is declared, but, on how and where it is called! Here, the `foo` was called from global context, and during its execution, `this` will be binded to the `global object` [`Default binding rule`], and since, regular functions and global object can only have `this` context, `this` context at the time of execution was pointing to the variable `a` declared in global scope, and hence the value was `a` was `global` and not `func`.

The above issue, could be solved using the `call` method on the `foo` function, where, we hard bind `this` of the function `foo` to the function itself, and not the `global scope`. Let's see how this can be done.

````javascript
function foo() {
    console.log(this.a);        // func, now since `this` is hard binded to
                                // the function itself, it now refers to the 
                                // variable a within the function.
}

foo.a = 'func';

var a = 'global';

foo.call(foo);
````

Let's look at another example, where `this` can be misleading.👣

````javascript
function foo() {
    setTimeout(function(){
            console.log(this.a);        // global
        },100);
}

foo.a = 'func';

var a = 'global';

foo.call(foo);
````

Again, the same output!! *This* was because of the fact, that even though the `setTimeout` was called from the `foo` function, and `this` in the `foo` function is hard binded to itself, because of the `call` method, the call-site of the callback function which we have provided was at `global level`, and hence, the variable at global scope was referred. How was the call-site of the callback function `global`? The callback function is called from the `setTimeout` function, and the `setTimeout` function is defined in the `global scope`, which means, that the callback function is called from the `global scope`.

To solve this problem, we can hard bind `this` of the callback function to `this` of the `foo` function, itself, which could be done using the `bind` method. 

````javascript
function foo() {
    setTimeout((function(){
            console.log(this.a);        // func
        }).bind(this),100);
}

foo.a = 'func';

var a = 'global';

foo.call(foo);
````

The, same problem could be solved using the arrow function, which offers a lexical scope look-up mechanism to find a reference to `this`. Let's see what it means and how it is done.

````javascript
function foo() {
    setTimeout(()=>{
            console.log(this.a);        // func
        },100);
}

foo.a = 'func';

var a = 'global';

foo.call(foo);
````

You must be wondering how did the above code worked, and we didn't even hard binded `this`  for the arrow function to `this` of `foo` function!!!! 👺

Well *this* is because the arrow functions does not have their own `this`, so how come did it printed `func` 😱, *this* is because of the fact that, for arrow functions `this` is used using lexical scope lookup, which means that, when a reference to `this` is made within any arrow function, the engine, will start looking up the scope of arrow function to find a `this` binding, default being the `global scope` `this`, so, when the above code is executed, during the callback, the engine, will first look for `this` within the scope of arrow function, which it fails to find, then it traverses up the scope of the arrow function, which is the scope of the function `foo`, and since it finds a `this` in the scope of the `foo`, that same `this` is used as future reference within the arrow function, and since `this` of `foo` is hard binded to itself, we get the value of `this.a` as `func`.

Let's modify the above example a little to make you understand, the `this` for arrow functions.

````javascript
function foo() {
    setTimeout(()=>{
            console.log(this.a);        
        },100);
}

foo.a = 'func';

var a = 'global';

foo();
````

If you guessed that the value of `this.a` will be `global`, then you have started to understand the `this`!!! Hooray!!! 🎉🎊.

However, if you guessed `func` 😔, then don't worry, their is a reason why I am here to explain 😃.

Let's rewind a little to the very first example of this section, can you recall anything???? Yes!! The reason why it console `global` was because, `foo` was called in global context, `this` of the `foo` function is binded to the `global object`, which means when the engine will encounter `this` for arrow function it will start looking up the scope, it will refer to the `this` of `foo` which is not itself but to the `global object`!!!! 😃

Let's walk you through another example, which will surely clear up the air around the `this` for arrow function.

````javascript
var obj = {
    a:'object???',
    foo:()=>{console.log(this.a)}
};

var a = 'global!!!';

obj.foo();              // global!!!
````

What the hell, just happened!! How come `global!!!` was printed, JS must have gone crazy!!😡 Why can't the JS just print the damn `object???`, It must be driving you crazy!! Well, let's make it clear..one thing is for certain, JS is not crazy, and I won't let you crazy! 😃

Again, let's rewind! Recall the sentence said at the very beginning of this section.

> In javascript only functions and global scope have a `this`.

Which means? that while engine started to look up the scope, it first bumped into the arrow function `foo` scope, then it looked up the scope and bumped into the scope of `obj` object, and since, only `regular functions` and `global scope` have `this`, the engine could not find, `this` in object also!
So, it again went looked the scope of the `obj`, and bumped into the `global scope`, where it found `this` and hence, we found our `this.a` as `global!!!` and not `object!!!`. Horrraayyy!!! 🎉

This could be solved by wrapping a the arrow function within a regular function, see the code snippet below:

````javascript
var obj = {
    a:'object???',
    foo:function(){
        return (()=>{
            console.log(this.a)
            })();
    }
};

var a = 'global!!!';

obj.foo();              // object????
````

Why did, the above worked? Firstly, the arrow function is wrapped within a regular function `foo`, which means whenever `this` is referred within the arrow function, engine will look up its scope and it will first land into the function `foo`, where it will find a `this`. Secondly, because of the implicit binding rule, when we call `obj.foo`, `this` for the `foo` will be binded to the `obj` object, and hence during execution we got `object???` as our value.

Let's twist the above example a little more, to see how things can be effected if `this` of the function `foo` is changed!

````javascript
var obj = {
    a:'object1???',
    foo:function(){
        return (()=>{
            console.log(this.a)
            })();
    }
};

var obj2 = {
    a:'ugh!!object2!!'
}

var a = 'global!!!';

obj.foo.call(obj2);              // ugh!!object2!!
````

How did this printed `ugh!!object2!!`😨 , *this* is  because `this` of the `foo` function is now hard binded to the `obj2` when we used the `call`, because of which when `this` of arrow function points to the variable `a` of `obj2`.

Consider the code snippet below, just to be sure, no smelly air is around the `this` of arrow functions!!!

````javascript
function foo()
{
    return (() => {
        return (() => {
            return (() => {
                    console.log(this.a);
                })();
            })();       
        })();
}

foo.a = 'func';

foo.call(foo);
````

So, Ask yourself two questions!! What should be the output???? And how many `this` are there??

The output will be `func`, and there is only one `this` which is the `this` of the function `foo` which is binded to itself, and for every arrow function, whenever a `this` is referred, it will look up the scope and it will finally land into `this` of function `foo`.

##### Does `call`, `apply` and `bind` effect arrow function's this?

Well *this* is kind of a useless question, because all the three methods hard binds `this` value of a function to the context specified as the parameter to those method, but, arrow functions does not have their own `this`, so what will these methods bind the context to????
Nothing!! 😛, which means calling arrow functions using `call` ,`apply` or `bind` can produce no effect. Let's look at the previous example, on how indifferent it could be.

````javascript
var obj = {
    a:'object???',
    foo:()=>{console.log(this.a)}
};

var a = 'global!!!';

obj.foo.call(obj);              // global!!!
````

##### Other lexically binded variable for arrow functions

Apart from `this`, variables like `arguments`, `super` and `new.target` are all lexically binded which means that they will look up their scope to determine find the variable value.

````javascript
function foo(foo1,foo2,foo3)
{
    return (arrow1,arrow2)=>{
        console.log(arguments);         // [1,2,3]
    }
}

foo(1,2,3)(4,5);
````

Even though, we the arrow function is accepting parameters, yet, when `arguments` are console, it gives the `arguments` passed to the `foo`, this is because arrow function does not have their own `arguments` object, it lexically lookup to find an `arguments` object. So you must be wondering, how can we handle the extra parameter which are being sent to the arrow function. We can use `rest` parameter!!!

````javascript
function foo(foo1,foo2,foo3,...restOfFoos)
{
    console.log(restOfFoos);                // [0]

    return (arrow1,arrow2,...restOfArrows)=>{
        console.log(restOfArrows);         // [6,7]
    }
}

foo(1,2,3,0)(4,5,6,7);
````

Since, arrow functions does not have a `[[Construct]]` and `prototype` property of their own, it is not possible to use the `new` operator in front of an arrow function.

````javascript
var a = () => {console.log('Hey!!')}
var b = new a();                        //Type error
````

****
**Previous Chapter: [Spread operators](https://github.com/anirudh-modi/JS-essentials/blob/master/ES2015/Functions/Spread%20operator.md)**

**[Strings](https://github.com/anirudh-modi/JS-essentials/blob/master/ES2015/Strings/main.md)**
