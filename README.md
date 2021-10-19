# Overview

This project is a work-in-progress for my own education and experimentation. It represents my attempt to program under various influences:

## Douglas Crockford

I try to follow Crockford's encouragement to program in Javascript by studiously avoiding its "bad parts". Although his [JSLint](http://jslint.com/) tool is inflexible and opinionated, I write so that all code passes it successfully with the provisional exception of the spacing style Sanctuary adopts for function calls (see [below](#spacing)).

## Static Land

I am implementing a curried variation of the [Static Land](https://github.com/fantasyland/static-land) specification, which is attractive because it separates data from methods. (a practice Crockford has advocated.) In my experiment, each algebraic type constructor exports a factory function for creating a curried Static Land-compliant type module.

## Sanctuary

I learned about implementing different algebras from studying [Sanctuary](https://github.com/sanctuary-js/sanctuary).

## Bartosz Milewski

His series of [Category Theory lectures](https://www.youtube.com/user/DrBartosz) challenged me to understand Category Theory by programming algebraic data types in Javascript.

# Conventions

## Currying
All functions are manually curried and unary. The one exception is functions that are called with this form:
```javascript
f (value, err)        
```

This style of function call is technically unary because it either has a value or an err, but cannot have both. In fact, this form is essentially an Either algebraic data type.

## Spacing
Sanctuary has recently adopted a style of currying where (unary) functions are called as follows:
```javascript
f (x) (y)
```
This can improve code readability, especially when there are nested function calls. However, an argument against this style is that nested function calls should be made more readable by introducing line breaks as follows:
```javascript
f(
    other_function_call(a)(b)
)(
    compose(x)(y)
)
```
Although I initially adopted Sanctuary's style, I am lately preferring the use of more line breaks.

## Factory functions
Factory functions that return type modules always take at least one curried argument:
- Factories for primitive types take no arguments
- Factories for containers that contain a value of another type take an optional type module for the contents
- Factories for containers that contain multiple types (Either, Pair, etc.) take a curried argument for each type

## Additional methods

In addition to the Static Land methods, each type module contains:
* A .create() method which can be used to create an instance of the data type. For some applicatives this will be synonymous with the .of() method, but for others (such as pair) it may be a non-rule-compliant function. This method should not do any validation of the input.
* A .validate() method which returns a boolean reflecting whether a value is a valid instance of that type
* Types that satisfy Semigroup/Applicative should also have an .append() method

## Properties

Each type module should include spec, version, and type_name properties as follows:
```javascript
{
    spec: "curried-static-land",
    version: 1,
    type_name: "Array"
}
```

## Order of arguments
Methods that take two values of a type should follow the convention:
Method              | English             | Javascript
--------------------|---------------------|--------------
.equals (9) (4)     | equals 9, does 4    | 4 === 9
.lt (5) (1)         | lt 5 is 1           | 1 < 5
.concat ([a]) ([b]) | concat to [a], [b]  | [a].concat([b])
