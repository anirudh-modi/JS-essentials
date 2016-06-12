
## Block scoped functions

Prior to ES6, the functions were always hoisted either in the nearest function scope or the global scope. However, in ES6 the declaration of function have changes a little.

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

****

**Next Chapter : [Default parameter](https://github.com/anirudh-modi/JS-essentials/blob/master/Functions/Default%20parameter.md)**
