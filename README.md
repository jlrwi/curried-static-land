# Overview

This project is a work-in-progress for my own education and experimentation. It represents my attempt to program under various influences:

## Douglas Crockford

I try to follow Crockford's encouragement to program in Javascript by studiously avoiding its "bad parts". Although his [JSLint](http://jslint.com/) tool is inflexible and opinionated, I write so that all code passes it successfully with the exception of the spacing style Sanctuary adtops for function calls (see below).

## Static Land

I'm attempting to implement a curried variation of the [Static Land](https://github.com/fantasyland/static-land) specification, which is attractive because it separates data from methods. (Something Crockford has advocated.) In my experiment, each algebraic type constructor exports a factory function for creating a Static Land-compliant type module.

The factory function may take multiple (curried) arguments, particularly if the type will contain typed contents. All returned modules are frozen.

## Sanctuary

I learned about implementing different algebras from studying [Sanctuary](https://github.com/sanctuary-js/sanctuary).
Sanctuary has recently adopted a style of currying where (unary) functions are called as follows:
```javascript
f (x) (y)
```

## Bartosz Milewski

His series of [Category Theory lectures](https://www.youtube.com/user/DrBartosz) challenged me to understand Category Theory by programming algebraic data types in Javascript.

# Conventions

All functions are manually curried and unary. The one exception is functions that are called with this form:
```javascript
f (value, err)        
```

This style of function call is technically unary because it either has a value or an err, but cannot have both. In fact, this form can easily be construed as an Either algebraic data type.

## Additional methods

In addition to the Static Land methods, each type module contains:
* A .create() method which can be used to create an instance of the data type. For some applicatives this will be synonymous with the .of() method, but for others (such as pair) it may be a non-rule-compliant function. This method should not do any validation of the input.
* A .validate() method which returns a boolean reflecting whether a value is a valid instance of that type
* A .toJSON() method
* Types that satisfy Semigroup/Applicative should also have an .append() method

## Properties

Each custom data type that stores a value in an object should include spec, version, and type_name properties as follows:
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
