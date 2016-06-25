
# New literals

In ES6, two new literals where introduced for `string` namely the 

* Template literal
* Tagged template literal

### Template Literal
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
