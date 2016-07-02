
# New literals

In ES6, two new literals where introduced for `string` namely the 

* Template literal
* Tagged template literal

## Template Literal
Template literal is a new kind of string literal which is introduced to provide a better support for multi line string support and provide string interpolation. Other benefits which is offered by template literal :

* You can interpolate variables in them
* You can actually interpolate using any kind of expression, not just variables
* They can be multi-line.
* You can construct raw templates that don’t interpret backslashes.
Let's look at the syntax for template literals.

#### Syntax
The syntax to use template literals is including your string within backticks `` ` ` ``. For string interpolation, the syntax is `${}`, where we can include any expression to be interpolated within the curly braces `{}`. Let's look at an example on how to do it.

`````javascript
var msg = `Hello Anirudh, Good morning`;

console.log(msg);
// Hello Anirudh, Good morning
`````

The `typeof` a template literal is a `string`.

#### Multiline Strings
Prior to ES6, is we wanted to achieve multiline text, it was a bit of pain, and ugly, too many string concatenation, and too many `/n` which creates nothing but a mess.

````javascript
var multiline = "\nHello Anirudh,\n"+
"This is a multline message\n"+
"May you have a good morning\n"+
"Regards,\n"+
"Ugly ES5";

console.log(multiline);
//Hello Anirudh,
//This is a multline message
//May you have a good morning
//Regards,
//Ugly ES5
````

The ES6 way!!! 😎

````javascript
var multiline=`
Hello Anirudh,
This is a multline message
May you have a good morning.
Regards,
Cool ES6
`
console.log(multiline);

//Hello Anirudh,
//This is a multline message
//May you have a good morning.
//Regards,
//Cool ES6
````

In template literals, every space we put becomes the part of the string, especially when go multiline, then a precaution need to kept in mind, while giving extra spaces before a new line, it will effect the length of the string. A new line also consumes a single character

````javascript
var msg = `1
2`;

console.log(msg.length);        //3 [2 characters 1 newline]

var msg = `
1
2
`;
console.log(msg.length);    // 5 [2 charactes 3 new line]
````

````javascript
var msg = `
1
 2
  3
   4`;
console.log(msg);           //
                            //1
                            // 2 
                            //  3 
                            //   4

console.log(msg.length);    //14 [4 characters 10 whitespace before 'String'
````

Because of the fact, that spaces become part of the string, template literals becomes very handy in creating html string, with proper indentations.

````javascript
var htmlElem = `
<div>
    <span>Hello</span>
    <span>Anirudh</span>
</div>
`;

console.log(htmlElem);      //<div>
                            //    <span>Hello</span>
                            //    <span>Anirudh</span>
                            //</div>
````

#### Escaping characters

If we wish to include a backtick `` ` `` within the template string, we can simply escape the character

````javascript

var escapedMsg = `Hello, This \` is a backtick`;

console.log(escapedMsg);
// Hello, This ` is a backtick
````

There is no need to escape single quotes `'` or double quotes `"` within a template literal.

````javascript
var singleAndDouble = `Hello "Anirudh", 'this' is not me!`;

console.log(singleAndDouble);
// Hello "Anirudh", 'this' is not me!
````

#### String interpolation
At this point, you may think that the template literals are a fancier and more sugar syntactic version of existing string literals, but, the real difference lies in the fact that template literal allow substitutions, a substitution which is capable of not just placing a variable but any valid javascript [expression]() will be evaluated and put as part of the string. Let's look at how string interpolation can be done.

Prior to ES6
````javascript
var firstName = 'Anirudh';
var lastName = 'Modi';

var msg = "Hello "+firstName+", should I call you Mr. "+lastName+"?";   

console.log(msg);
// Hello Anirudh, should I call you Mr. Modi?
````

Even though their is nothing wrong with this syntax, however a lot times, when the string, get lengthy, if we miss a single `+` it is bound to cause a syntax  error. This could be much simplified in ES6:

````javascript
var firstName = 'Anirudh';
var lastName = 'Modi';

var msg = `Hello ${firstName}, should I call you Mr. ${lastName}?`;

console.log(msg);
// Hello Anirudh, should I call you Mr. Modi?
````

String interpolation, is not just limited to replacing a variable, it can have any valid expression, be it a function call, inline function expression calls, and even creating an interpolated template string within it!

**Expression calculation**
````javascript
var firstName = 'Anirudh';
var items = 2;
var cost = 4;

var msg = `Hello ${firstName}, total amount is ${items * cost}`;

console.log(msg);
// Hello Anirudh, total amount is 8
````

**Nested Interpolated template string**
````javascript
var name = 'Anirudh';
var name2 = 'Bob'

var msg = `Hello ${name}, friends in ${`${name}'s`} list is 4`;

console.log(msg)
//Hello Anirudh,friends in Bob's list is 4
````

**Function call**
````javascript
function lowerCase(st)
{
    return st.toLowerCase();
}

var name = 'Anirudh';
var name2 = `BoB`;

var msg1 = `Hello ${name}, friends in ${lowerCase(name2)} list is 4`
var msg2 = `Hello ${name},friends in ${lowerCase(`${name2}'S`)} list is 4`;

console.log(msg1)
//Hello Anirudh, friends in bob list is 4

console.log(msg2)
//Hello Anirudh, friends in bob's list is 4
````

You must be wondering what happens if the function does not `returns` anything! 
In that case it will print `undefined` as an implicit `return` will be made by function which is `undefined`. [More on why `undefined`](https://github.com/anirudh-modi/JS-essentials/blob/master/ES2015/Functions/Arrow%20functions.md#why-undefined).

````javascript
function lowerCase(st)
{
    st.toLowerCase();
}

var name = 'Anirudh';
var name2 = `BoB`;

var msg1 = `Hello ${name}, friends in ${lowerCase(name2)} list is 4`;

console.log(msg1);
//Hello Anirudh, friends in undefined list is 4

````

**Immediately Invoked Function Expression (IIFE)**

Within the interpolation expression, we can use IIFE or IIAF (Immediately Invoked Arrow Functions) also, to `return` a value which can be a part of the string.

````javascript
var msg=`Hello, this is a ${(()=>'sunny')()} morning`;

console.log(msg);
// Hello, this is a sunny morning.
````

## Tagged Template literal

We have seen how template literals help us reduce the effort of performing string concatenation and create multi-line string effortlessly, but, ES6 also offers us the verbosity of creating a totally different string of our own, using the template literals along with a tag, this feature is known as the *tagged template literal*. A tag is `function` which will receive a `strings array` and list of **evaluated values** of `interpolated expression` which were used in a template literal, and using it whatever `string` returned by the tag function, will be used as the output. Let's look at the syntax.

#### Syntax

As mentioned, a tag is nothing but a function, only when we use it front of a template literal it gets called.

````javascript
function tag(strings,...values) {
    console.log(strings);
    console.log(values);
}

tag`hello, from template literal`;
//[hello, from template literal]
//[]
````

Since, in the above example no *interpolated expression* were used the second parameter is empty.

````javascript
function tag(strings,...values) {
    console.log(strings);
    console.log(values);
}

var name = 'Anirudh';

tag`hello, ${name}!`;
//["hello, ","!"]
//["Anirudh"]
````

You must be confused! what is that! how can you use a function like that!!! Well, this is a new kind of function call which does not need a parentheses `()` after it, here the function `tag` gets called when used in front of a template literal, the string which is a part of the template literal is parsed and passed as a value to the function `tag`.

When the `tag` function is called, the first parameter which is passed to the function, is the `array` of `strings` which are used within template literals, this `array` will not include the values of *interpolated expression*, however it will have `strings` around them, which is why in the second example we get an `array` of two elements `"hello ,"` and `!`.

And the rest of the parameters which passed to the `tag` function are the **evaluated values** of the *interpolated expression* used within the template literals. Since, there can be `n` number of values, it is better to represent them using a `rest parameter`, which will return an `array` of *values* for all the expressions use in the template literal.

The `strings` in the array are the splitted strings in order, as an expression comes up in the original literal, so the first string was `hello ,` after which came the expression, and after the expression came the `!` , hence two strings and the order of the string is also maintained and the order of the expression value is also maintained.

````javascript
function tag(strings,...values) {
    console.log(strings);
    console.log(values);
}

var name = 'Anirudh';

tag`hello: ${name}! ${`Modi`}`;
//["hello: " , "! " , ""]
//["Anirudh" , "Modi"]
````

You can see that this literal now has two expressions! One after the string `hello: ` and the other after the string `! `, so if we look up the string array, first element is the `hello: `, second element is `! `, and there is a third element which signifies the string after the last epxression, and since there is nothing after the last expression `${`Modi`}`, an empty string is inserted, and this will be the default behavior, we can also see that the `values` array is also in order, the first element is `Anirudh`, which comes after the `hello: `, the second element is `Modi` which comes after the `! `.

> So, it can be noted that the order of the `string` array and the values of the *interpolated expression* within the literal will always be the same as the literal, and even if a *interpolated expression* is the last item in the literal, the length of the `string` array will always be 1 greater than the length of `values` array.

````javascript
tag`Hello`;     // Syntax error, undefined is not a function
                // because the tag function is never defined
````

Naming of a tag function is not restricted to `tag` any name can be used to represent a tag function, because in the end it is function.

````javascript
function fooMe(strings,...values) {
    console.log(strings);
    console.log(values);
    console.log(arguments);         // arguments object will return a single
                                    // array with first param as array of
                                    // strings and the values of expression
                                    // will be scattered

}

var name = 'Anirudh';

fooMe`hello, ${name}!`;
//["hello, ","!"]
//["Anirudh"]
// [["hello, ","!"],"Anirudh"]
````

This also means that function can also be a `arrow functions`, or an `anonymous/named function` can be returned within a function which can be used as a tag function.

````javascript
var fooArrow = (strings,...values) => {
    console.log(strings);
    console.log(values);
};

fooArrow`hello, ${name}!`;
//["hello, ","!"]
//["Anirudh"]

function returnTagFunc() {
    return function(strings,...values) {
        console.log(strings);
        console.log(values);
    }
}

returnTagFunc()`hello, ${name}!`;
//["hello, ","!"]
//["Anirudh"]
````

It was mentioned that whatever the tag function `return` will be treated as and used as new `string`, which means we can return a totally different `string` and if nothing is returned then function implicitly `returns` `undefined` which will be the value of the new `string`.

````javascript
function fooMe(strings,...values) {
    console.log(values);
}

var name = 'Anirudh';

var taggedText = fooMe`hello, ${name}!`;
console.log(taggedText);
// undefined


var fooArrow = (strings,...values) => 'foo is me';

var taggedArrow = fooArrow`hello, ${name}!`;
console.log(taggedArrow);
// foo is me
````

Now, that we understood the syntax for the tagged template literals, let look how can we use to parameters to build up a new string.

````javascript
function fooMe(strings,...values) {
    let result = ;

    // You can run a loop on the strings also, but 
    // then while adding we need to keep in mind that the 
    // len of string is 1 greater than values,
    // which means there is no element in 
    // values[strings.length-1]
    for(let i=0;i<values.length;i++) {
        result = strings[i]+values[i];
    }

    // Since the length of values will always be one less than string, 
    // we need to include the last element of the string
    result = result + strings[strings.length - 1];

    return result;
}

var name = 'Anirudh';

var taggedText = fooMe`hello, ${name}!`;

console.log(taggedText);
// hello, Anirudh!
````

We can also use template literals within the function to create new string, but we need to be careful about the `whitespaces` as they will also get included as a part of the new `string`.

````javascript
function fooMe(strings,...values) {
    let result = ``;

    // You can run a loop on the strings also, but 
    // then while adding we need to keep in mind that the 
    // len of string is 1 greater than values,
    // which means there is no element in 
    // values[strings.length-1]
    for(let i=0;i<values.length;i++) {
        result = `${result}${strings[i]}${values[i]}`;
    }

    // Since the length of values will always be one less than string, 
    // we need to include the last element of the string
    result = `${result}${strings[strings.length - 1]}`;

    return result;
}

var name = 'Anirudh';

var taggedText = fooMe`hello, ${name}!`;

console.log(taggedText);
// hello, Anirudh!
````

With ES6, every string has two distinct version :

* Cooked version : Where the backlashes are interpreted, which means that `\n` will be interpreted as a new line
* Raw version : where the backlashes are not interpreted, which means that a `\n` will not be interpreted as a new line but left as it is.

````javascript
var str = String.raw`Hello\nWorld`;

console.log(str);
console.log(str.length);
// Hello/nWorld
// 12 this also means that the \n will be treated as characters of str
````

We can notice how we used `String.raw(.. )` as a tagged template function, well it a built in tagged template within the `String` wrapper object. The `String.raw(..)` can be useful to generate `regex` or other places where keeping the `/` would be useful.

#### Further Reads on tagged template literal

For more uses of tagged template literal you go through the links below : 

* http://jaysoo.ca/2014/03/20/i18n-with-es6-template-strings/
* http://wiki.ecmascript.org/doku.php?id=harmony:quasis
* https://gist.github.com/dherman/6165867
* https://gist.github.com/slevithan/4222600

****

**Next chapter : [New methods in String Wrapper](https://github.com/anirudh-modi/JS-essentials/blob/master/ES2015/Strings/stringMethods.md)**

