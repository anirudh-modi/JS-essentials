## 2. Functions
With ES6, function paramters declaration and passing values to the parameters of the functions have become more versatile than before. So lets got through the changes how the function behaviour have changed.

#### 2.1 Blocked scoped function declaration

Prior to ES6, the functions were always hosited either in the nearest function scope or the global scope. However, in ES6 the decalation of function have changes a little.

````javascript
{
    function foo(){
        console.log(1);
    }
}

foo(); // 1 the foo function hosts itself to the global scope and the block ignored,hence the foo function is available outside the scope.
````

However, with introduction of the block scoping in ES6, the function foo only exists within the block scope, and it wont be accessible outside the scope and give a reference error, because the function foo is available only within the scope.

````javascript
if(1)
{
    function fooFinal1()
    {
        console.log(1);
    }
}
else
{
    function fooFinal2()
    {
        console.log(2);
    }
}
fooFinal1();    // 1 (Prior ES6) 
                // fooFinal1 is not a function (ES6+)

fooFinal2();// Currently in both ES6+ and ES6- it gives an error because 
            // it under continuous change, due to which the different browsers 
            // behave differently.
````

The reason why, above codes fails to execute in ES6 and above is because of the fact scope exist and both the functions fooFinal1, and fooFinal2 can only be accessed within the `if...else` scope.

#### 2.2 Default parameter values

This enables us to provide default values to a function's parameter. Consider the code below :

````javascript
function add(firstValue,secondValue)
{
    console.log(firstValue + secondValue);
}
add(1,2); // 3
add(1); // gives a NaN
````

Now often scenarios arise where you might forget to a second value to the function, in which the function will start retruning `NaN` if the second value is not passed or the first parameter is not passed or `undefined` is passed as a value. 

> TODO : Show how to skip the first parameter while calling a function using `apply` 

````javascript
function add(firstValue,secondValue)
{
    if(!firstValue)
    {
        firstValue = 1;
    }

    if(!secondValue){
        secondValue = 3;
    }

    console.log(firstValue + secondValue);
}
````

However, if you try sending a value of 0, the `if` condition will return a falsy value, because of which a default value of 300 will be taken. Also, if the value being passed is `null` then also the timeout tasken will be 300, which might not be the desired case. So, we end up making a slight modification:

````javascript
function add(firstValue,secondValue)
{
    if(typeof firstValue ==="undefined")
    {
        firstValue = 1;
    }

    if(typeof secondValue==="undefined"){
        secondValue = 3;
    }

    console.log(firstValue + secondValue);
}
````

There is nothing wrong with the above code, or per se writing extra validation, which will take care of the parameter values if not passed, but it not only requires an axtra precaution, but also leads to the issue mentioned with null and 0, which can be very easily neglected unless we bum into the situation. So, with ES6, we get to assign default values to the parameter which are passed to a function.

````javascript
function add(firstValue = 1,secondValue = 3)
{
    console.log(firstValue + secondValue);
}
add(1);                     // 4
add(undefined,1);           // 2
add(null,1);                // 1 null is coereced to 0
add(1,0);                   // 1
add(undefined,undefined);   // 4 takes the default value
````

##### **Default pramater effecting arguments object**
The default paramters changes how the arguments object behave, one might think that if a paramter is not passed to the function and with the default value, the arguments object will give the default value, however, this is not true,
the argument object represents the set of values which where passed to a function, which doesn't get overridden by the default value if no value was passed.

````javascript
function add(a=1,b=2)
{
    console.log(a,b);                       //3,2
    console.log(arguments[0],arguments[1]); // 3,undefined
}

add(3);
````

It is worth noting that the way arguments object behave can be different in strict and non strict mode of ES5. In non-strict mode, the argument object reflect the changes in the value of the paramters which are being present, however in strict mode any changes to the value of the paramter will not be reflected. It also worth noting that in ES5, if any one the parameters are `undefined`, and we try to change the value of the parameter in both `strict-mode` or `non-strict mode` the arguments object remains unchanged

**Non-strict mode**
````javascript
function add(a,b)
{
    console.log(arguments[0],arguments[1]); // 1,2
    a = 5,b =10;
    console.log(arguments[0],arguments[1]); // 5,10, in non-strict mode
                                            // the arguments object reflect
                                            // the changes occuring in the
                                            // value of the parameters
}
add(1,2);
````

**Strict mode**
````javascript
function add(a,b)
{
    'use strict';
    console.log(arguments[0],arguments[1]); // 1,2
    a = 5,b =10;
    console.log(arguments[0],arguments[1]); // 1,2, in strict mode
                                            // the arguments object reflect
                                            // the initial value of the
                                            // parameter passed, hence 
                                            // these changes aren't reflected
}
add(1,2);
````

**Strict mode/ Non-strict mode**
````javascript
function add(a,b)
{
    'use strict';
    console.log(arguments[0],arguments[1]); // undefined,undefined 
                                            // 1,undefined
    a = 5,b =10;
    console.log(arguments[0],arguments[1]); // undefined,undefined
                                            // 1, undefined
}
add();
add(1)
````

And when we use the default parameter in ES6, it behaves in the same way as in `strict-mode` of ES6, and hence the changes in the value of the paramter are never relfected in arguments object.


>The default paramter can be argued as a syntatic sugar, but it certainly reduces the fear of no paramter being passed, that too in a clean way.

#### 2.3 Default Value Expressions

The default paramters are not restricted just till the assigning number or string, a function can also be set as the default value `callback = function(){}`. It can also a previously declared parameter as a default value, we can perform inline operation before assigning default value and even a function call, lets have look into the other variations.

````javascript
function multiply(x){
    return x*2;
}
function add(a = 1, b = a, c = b + a,d = multiply(c))
{
    console.log((a + b + c),d);
}

add(1);     // 4,4
add(3);     // 12,12
add(2,7);   // 18,18
add(1,2,5); // 8,10
add(1,2,5,10); // 8,20
````

**Condition paramter scope**

In ES6, whenever default parameters are used in the functions declaration, a scope is created around the function parameters, whenever the function is executed, which enables us to assign values of previous parameters to the upcoming parameters.

> The way scoping works is if it doesn't find a variable in the current scope, it looks up to the scope immediately above it, and it keeps on looking up untill it reaches the global scope.

> **The reason for introducing param scope is: the variables in the body with the same name should not affect captured in closures bindings with the same name.**

Lets try to look what it means

````javascript
function add(a = b, b=2){...}
````

The reason above code throws an error, is because when the function is started to execute a scope is created around the function parameters, and the paramters of the function are put in the TDZ, like the `let` behaves. Lets see what and how the above code fails:

````javascript
function add(a = b, b = 2){...}

// When we call the add function with add(1,1), 
// virtually a scope around the parameter is created,
// where the variables are declared;
{
    let a = 1;
    let b = 1;
}

// When we call the add function with add(undefined,2),
// virtually a scope is created, where the variables are declared;
{
    let a = b;  // now as we know that the let decalred varible lie in TDZ,
                // and while this lines get executed the JS engine tries to
                // look in scope whether a variable b is present or not, but 
                // since the variable b is not decalred and when JS engine 
                // tries to look in the scope it throws the reference error.
                // and hence forward lookup are not allowed.
    let b = 2;
}
````

The parameter scoping, effectively overshadows the variable defined in outer scope, when the function is executed, which means if we look at the code below, we can see that when the function foo is executed, the value of `x` is 'param scope', however the value of the outer `x` still remains the same, which is because a scope is created around the function params, and when no default value is found, a variable `x` is declared and initialzed witht the value of 'param scope', and when the function is executed, it first searches for `x` within the function scope and since no `x` is found it then looks up the param scope where it find the value of `x` as 'param scope'.

````javascript
var x = 'outer scope';

function foo(x = 'param scope')
{
    console.log(x); // param scope
}

foo();

console.log(x);     // outer scope
````

````javascript
var x = 'outer scope';

function foo(y = function(){ x=1;return x;})
{
    console.log(y());       // 1
}
foo();
console.log(x);             // 1
````

The reason why the global `x` is changes because scope look-up mechanism, it could not find the variable x within the parameter scope(immediate scope), hence looks-up at the function scope outside the foo function, and hence, it changes the value their, and changes the global scope value.

Let's look at another example on how parameter scope effects the inner scope of a function.

````javascript
var x = 'outer scope';
function foo(x,y = function(){x = 'param scope';})
{
    var x = 'function scope';
    y();
    console.log(x); // 'function scope'
}

console.log(x);     // 'outer scope'

foo();
````

Now, since it was mentioned earlier that, in ES6, whenever a default parameter is used in the function declaration, a scope is created around the params during the exec, this leaves us with 3 different scope when code is executed.

````javascript
x = 'outer scope' // this is global/function scope;
{x, y = function(){ x = 'param scope'}} // the scope created around the param
x = 'inner scope' //the scope within the func foo
````

because of the three scopes, when we execute the `function y` it searches for `x` in its nearest scope environment, ie the parameter scope environment, and because of which the value of `x` is only changed within the parameter scope,and the inner function scoped `x` is never touched becuase it lies within after the parameter scope and the call never looks up to the global `x`, becuase it has already `x` within its scope.

If the try to do the same code without using default parameter values, it will not create a parameter scope, lets how the above code makes a difference
````javascript
var x  = 'outer';
function foo(x,y){
    if(typeof y =='undefined')
    {
        y = function()
        {
            x = 'param';
        }
    }

    var x = 'function';
    y();
    console.log(x); //param scope
    console.log(arguments[0]); // undefined
    console.log(arguments[1]); // undefined
}
foo();
````

This is because the moment we remove the default parameter scope, only two scope exist, the global scope and the foo scope, and becuase of variable hoisting, the variable `x` declared within the foo function is hoisted at the top, of the function foo scope with its value `undefined`, and a function `y` is also hoisted at scope of the foo, and when the function `y` is executed, it is executed within the foo scope environment and it changes the variable within the foo scope, and this also clarifies the reason why arguments[0] is never changed.