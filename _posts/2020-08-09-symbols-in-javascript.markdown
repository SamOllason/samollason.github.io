---
layout: post
title:  "Symbols in JavaScript"
date:   2020-12-18 14:00:57 +0100
categories: 
---

Recently I was upgrading a library and found that it broke some of our unit tests. The reason it was failing was 
because the test infrastructure was looking for keys in the mocked request that weren’t being found anymore. 
It turns out that the new keys were in fact **symbolic keys**.
That investigation lead to me learning more about JavaScript symbols. These were the
notes I wish I had last week! Hopefully they help you too.

## What are they
JavaScript has two categories of types: primitives and objects. 
Technically, `null` is both a primitive and an object, 
so the more accurate definition is of a primitive is: *If it isn’t an object and has 
no methods then it's a primitive* (see [here](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)).

A few years ago the Symbol primitive was added to the language. I read about it at the time and remember getting the impression that 
it's a bit of a niche corner of the language that most people won’t
need to bother with most of the time.
I think that’s still correct, but I happened to run into a situation recently where
I needed to know what they were and how they worked in more detail.

# Why use them?
They provide a way of generating unique identifiers.

# Example of why they are useful

They seem to be used mainly for ‘hidden’ fields on objects. 
Although it is still possible to access them, the point is that their implementation
makes it difficult to access. They aren't found using `for...in` loops or
by accessing `Object.keys()`.

This is useful if you want to set flags that you don’t want developers to accidentally set. 
The example that comes up a lot online is to indicate whether a list is sorted or not. 
This is useful metadata that you don’t want someone to accidentally overrite.

Read down below to lern about how to access symbolic keys.

## How to use them

# What they feel like
As mentioned above, they are a primitive in the JavaScript language, but the way they are written
and used makes them feel kind of like an object, but with some noticeable differences.

# Creating new ones
For instance, you can't need to use the `new` operator.

```
let sym1 = Symbol() // correct
let sym1 = new Symbol()  // TypeError
```

You can wrap them up in an object if you really want to, but it's not straightforward:

```
let sym = Symbol('foo')
typeof sym      // "symbol"
let symObj = Object(sym)
typeof symObj   // "object
```

And I'm not sure yet why you would want to do this.

# Uniqueness
Something else that is interesting that's worth mentioning here is that each 'symbol' is unique:

```
// each of these is a different symbol
let sym1 = Symbol()
let sym2 = Symbol('foo')
let sym3 = Symbol('foo')

Symbol('foo') === Symbol('foo')  // false

```

# Global keys
You can create symbols in such a way that they are globally available throughout your code base using `Symbol.for()`,
which could be used really well or abused really well!

# Accessing symbolic keys - indexing

This was where my original problem was. I was trying to access the value of the nested 'path' field
in this object:

```
Request {
    size: 0,
    timeout: 0,
    follow: 20,
    compress: true,
    counter: 0,
    agent: undefined,
    [Symbol(Body internals)]: { body: null, disturbed: false, error: null },
    [Symbol(Request internals)]: {
        method: 'GET',
        redirect: 'follow',
        path: 'mypath'
        ...
    }
}
```

It turns out that doing `request['Symbol(Request internals)']` doesn't work.

I mentioned above that when symbols are used as keys in objects they can't be accessed through
'the standard' methods. 

To access them I had to use `Object.getOwnPropertySymbols(request)` which returns an array of symbols. To find
the particular symbol I had to use a little helper which searches through the array and finds a particular element.

```
const sym = Object.getOwnPropertySymbols(data).find(s => String(s) === 'Symbol(Request internals)')
const path = request[sym].path

```

However, that approach [doesn't agree with TypeScript just quite yet](https://stackoverflow.com/questions/59118271/using-symbol-as-object-key-type-in-typescript)
(as at December 2020), so you will have to add `@ts-ignore` above where `sym` is used!

# Accessing symbolic keys - methods
Sometimes you will get lucky and the object you are working with has some methods to access symbolic keys
easily. I also had to access the 'headers' field in a request as part of this test and fortunately there
is a method that just takes care of this nicely in this object. You can see more [here](https://stackoverflow.com/questions/50060008/nodejs-parse-fetch-response-containing-object-symbolmap).


## More reading
* [https://stackoverflow.com/questions/50060008/nodejs-parse-fetch-response-containing-object-symbolmap/50856907](https://stackoverflow.com/questions/50060008/nodejs-parse-fetch-response-containing-object-symbolmap/50856907)
* [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)
