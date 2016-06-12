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
