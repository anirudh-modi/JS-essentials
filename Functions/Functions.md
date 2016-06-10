# Functions
With ES6, function paramters declaration and passing values to the parameters of the functions have become more versatile than before. So lets got through the changes how the function behaviour have changed.

## Blocked scoped function declaration

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

## Default parameter values

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

It is worth noting that the way arguments object behave can be different in strict and non strict mode of ES5. In non-strict mode, the argument object reflect the changes in the value of the paramters which are being present, however in strict mode any changes to the value of the paramter will not be reflected. 

It also worth noting that in ES5, if any one the parameters are `undefined`, and we try to change the value of the parameter in both `strict-mode` or `non-strict mode` the arguments object remains unchanged, the reason for that is when a function is called then if values are passed to its named parameter, then in `non-strict mode` any changes to the named parameter, will be reflected in the `argument` object also, however, if during the function call, a value is not passed to any of the named parameter, then any change in the value of named parameter is not reflected in its `argument` object.

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

### Default Value Expressions

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
function foo(x,y) {

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

## Rest paramaeter

Prior to ES6, the way to access extra parameter or unamed parameter passed to a function, was through the `arguments` object, working with the `arguments` object was not an issue, you can loop through the argument object and extract the extra values being passed to a function, however, one problem with that approach was the `arguments` object always returned the entire set of parameters, even the named parameters, lets look what it meant.

````javascript
function foo(a,b)
{
    console.log(arguments); // [1,2,3,4]
}
foo(1,2,3,4);
````

We can see above the arguments object not only gave the unamed parameters but also the parameters which are already declared, in function declaration. So, to extract the unamed params, a `loop` from 2 untill the length of `arguments`need to be run to extract unamed params. So, just to solve this a `rest` parameter was introduced.

> The `arguments` is not a real array, it is an object.

We can declare a rest parameter by putting `...` before any named parameter, and the parameter will extract the extra values which are passed during the function call [excluding the values for the parameter which are already defined in function declaration], and the extra values are assigned to the rest parameter in an array. 

````javascript
function foo(a,b,...restOfParams)
{
    console.log(restOfParams);  // [3,4] 
    console.log(arguments);     // [1,2,3,4] 
}
foo(1,2,3,4);
````

In the above code, since `a` and `b` is already decalred as parametere during function declaration, the values `1` and `2` and assigned to `a` and `b` respectively, and the extra values `3,4` are extracted and assigned as `rest` parameter.

The rest parameter not only solves the problem with `arguments` object, it also leaves the `arguments` object unaltered, and return a real array. Declaring a  `rest` parameter doesn't change the length of a function (which returns the number of parameters declared in a function).

````javascript
function foo(a){};
foo.length()        // 1

function foo(a,...rest){};
foo.length()        // 1

function foo(...res){};
foo.length()        // 0
````

The `rest` parameter doesn't have any effect on the `arguments` object, which means any change in the value rest parameter, will not effect the values in `arguments` object and, vice-versa, because in `strict-mode` the `arguments` object will represent only the values send during the function call.

````javascript
function restFunc(a,...restParam) {

    console.log(arguments);     // 1,2,3
    console.log(restParam);     // 1,2,3
    
    restParam[0] = 4;       
    
    console.log(arguments);     // 1,2,3
    console.log(restParam);     // 1,4,3
}
restFunc(1,2,3);
````

**Restrictions of rest parameter**

One of the main restriction of `rest` parameter is that, if the `rest` parameter is declared as param for a `function` then, there can't be any other parameter declaration after it.

````javascript
function foo(a,...restParam,c) // Syntax syntax
{....}

function foo(a,...restParam,...resParam2) // Syntax error
{....}
````

## Spread operator

The syntax for the `spread` operator is similar to that of the `rest` parameter i.e `...` , however, it performs the job of spreading an iterable, it behaves a little differntly depending on where it is being used. Let's have a look at how it works.

**During function and method call**

When using a `spread` operator during function call, the `spread` operator does the job of spreading the values mentioned in an array as the values to the parameters defined in function declaration.
````javascript
function foo(a,b,...restParam)
{
    console.log(a);             // 1
    console.log(b);             // 2
    console.log(restParam);  // [3,4]
}

foo(...[1,2,3,4]);
````

Let's take an example, on how it will behave in a real life scenario. We all know `array.push` method, it accepts n number of parameters, and for each value it pushes the value in the corresponding array, So when we use the `spread` operator, during calling the `array.push` method, the operator will spread all the values as the arguments for the `array.push`, for smaller chunks of `push` you might not find `spread` operator useful, however for a larger one, it can be more useful to push all the values in an array and put the `...` in front of the array and pass it directly to the `array.push`.

````javascript
var arr = [1,2];

arr.push(...[3,4,5]);

console.log(arr);    // 1,2,3,4,5

var arr2 = [6,7];

arr.push(...arr2);

console.log(arr);   // 1,2,3,4,5,6,7
````

Let's look at another scenario, suppose we want to create a function sum, which can accept n number of arguments and return the sum of all arguments.

ES6 Prior
````javascript
function sum()
{
    var sumAll=0;
    if(arguments.length)
    {
        for(var i=0;i<arguments.length;i++)
        {
            sumAll = sumAll+arguments[i];
        }
    }

    return sumAll;
}
sum(1);         // 1
sum(1,2);       // 3
sum(12,2,3,4);  // 21
````
 
 This function decalration takes care of multiple params, however suppose we want to send an array of numbers, and we want to achieve the same result, without modifying the content of sum function, then the way we call the function sum needs to be modified, that is we need use `apply` on the sum function, and pass the array, so the `apply` will pass the individual values of the array as the `arguments` for the function `sum`.

 ````javascript
 sum.apply(sum,[1,2,3]);    // 6
 ````

Now, the same thing can be used using the `spread` operator and the `rest` parameter, in ES6, lets see how it can be done.

````javascript
function sum(...rest)
{
    var sumAll=0;

    if(rest.length)
    {
        rest.forEach(function(i)
        {
            sumAll = sumAll+rest
        });
    }

    return sumAll;
}
// we can call like this
sum(1,2,3,4,5,6);       // 21

// or if we want to use an array then we can use the spread operator
sum(...[1,2,3,4,5]);    //15
````

**In arrays**

The `spread` operator is not just retricted to spreading values of an iterable during function call, it can be used within array also, meaning reconstructing a new array with an existing array. 

Lets, have a look on this can be acheived in ES5.

````javascript
var arr1=[2,3,4];
var arr2=[1];
// create an array 2 with values 1,2,3,4,5 using arr1

for(var i=0,len<arr1.length;i<len;i++)
{
    arr2.push(arr1[i]);
}
arr2.push(5);

//Or
arr2.concat(arr1, [5]);
````

We have to run a `for..loop` over the `arr1` then push values for every `arr1` value, or use `concat`. So the `spread` operator can solve this very simply and elegant way.

````javascript
var arr1 = [2,3,4];

var arr2 = [1,...arr1,5];

console.log(arr2);  // [1,2,3,4,5]
````

Not only the code is reduced, it reduces the effort to create an array.

Another, advantage of having the `spread` operator can be to `destructure` any `iterables`. Let's have a look what it means and how it can help. Let's consider if there is an array `[1,2,3,4,5]` and we want to spilt to variable `a` holding first value, and another array `b` holding rest of the values in array.

````javascript
var arr1 = [1,2,3,4,5];

var a = arr1[0];

var restArray = arr1.slice(1);

// Or

var restArray = [];

for(var i=1,len<arr1.length;i<len;i++)
{
    restArray.push(arr1[i]);
}
````

Even though nothing is wrong with it, but the same thing can achieved using `spread` operator in a cleaner way.

````javascript
var arr1 = [1,2,3,4,5];

[a,...restParam]=arr1;

console.log(a);             // 1

console.log(restParam);     // [2,3,4,5]
````

The above trick is known as `destructing` which we will cover later in more depth. One common exmaple/ practical exmaple of `spread` operator along with `destructing` could be using the `arguments` object.

> Even though we previously went thru the syntax of `rest` parameter, it is just to show how `spread` can also be used to `destructure` something.

````javascript
function foo(a,b)
{
    [a,b,...restParam] = arguments;
    console.log(restParam);         // [3,4]
}

foo(12,2,3,4);
````

**What do you mean by iterables?**

Now, throught the explaination I mentioned that the `spread` operator can be used over an **iterable**. So, what does this iterable means?

`Iterables` are objects which have the property `Symbol.iterator` as a method, Built in `iterable` in Js are `Array`,`String`,`Set`,`TypedArray`,`arguments` and so on. We can create our `iterable` object.

Because of this reason, the `arguments` object can be in `spread` operator.

You may find some where that `node-list` can also be converted into `array` using `speard` operator, however, this depends on browser, in firefox the `node-list` are considered as `iterable`, in safari and chrome they are not `iterable`. 

````javascript
var text = 'blue sky';
var splittedChars = [...text];
console.log(splittedChars);     // ["b", "l", "u", "e", " ", "s", "k", "y"]
                                // this works because the string is an
                                // iterable
````
