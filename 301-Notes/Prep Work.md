# Arrow Functions

## Syntax 

```javascript
() => expression

param => expression

(param) => expression

(param1, paramN) => expression

() => {
  statements
}

param => {
  statements
}

(param1, paramN) => {
  statements
} 
```

## Step by Step Traditional Function => Arrow Function

 Traditional anonymous function
    (function (a) {
  return a + 100;
    });

 1. Remove the word "function" and place arrow between the argument and opening body brace
    
    (a) => {
  return a + 100;
    };

2. Remove the body braces and word "return" — the return is implied.

    (a) => a + 100;

 3. Remove the parameter parentheses

    a => a + 100;

Returning object literals using the expression body syntax (params) => { object: literal } does not work as expected.

- To fix this, wrap the object literal in parentheses:
  - const func = () => ({ foo: 1 });


## Arrow Function Behavior:

Arrow functions in JavaScript have a unique behavior regarding the "this" keyword.
Unlike regular functions, arrow functions do not have their own "this" context. Instead, they inherit the "this" value from their lexical (surrounding) scope.
Lexical Scoping:

Lexical scoping means that the value of "this" inside an arrow function is determined by where the arrow function is defined, not how or where it is called.
In other words, the arrow function "remembers" the value of "this" from the context in which it was created.
Global Context:

When the arrow function Student.prototype.scopeArrow is defined, it's done in a global context, meaning it's not part of any specific object or function.
In the global context, the default value of "this" is the global object (e.g., window in a browser environment).
Invocation of joe.scopeArrow():

When you later invoke joe.scopeArrow(), the arrow function does not get its own "this" context. Instead, it uses the "this" value from the global context where it was defined.
As a result, when joe.scopeArrow() is invoked, "this" inside the arrow function does not refer to the joe object (as might be expected for a method). Instead, it refers to the global object.
Conclusion:

Arrow functions are not suitable for methods in constructor functions or prototypes when you need the value of "this" to be the instance of the object calling the method.
They are more appropriate for situations where you want to preserve the "this" value from the surrounding context, often used in callbacks or concise functions.

Sources: ChatGPT, MDN docs.