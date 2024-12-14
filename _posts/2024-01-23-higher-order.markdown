---
layout: post
title: All You Need is ... Higher Order Functions
date: 2024-01-23 00:00:00 +0300
description: All You Need is ... Higher Order Functions
img: higherOrder.jpg # Add image post (optional)
tags: [Js, Node] # add tag
---
I believe John Lennon and Paul McCartney wrote the song "All You Need is Higher Order Functions". Or maybe not, who knows, but as a JavaScript developer, or any developer working in a functional programming language, you will benefit greatly from these two wise men's advice. Higher-order functions are great.

So, why higher-order functions? Well, one of the things that JavaScript thrives on is working with functions. You've probably heard that JavaScript treats functions as first-class citizens. That means JavaScript allows us to treat functions like any other object. You do all kinds of cool stuff like pass functions to other functions and store them in objects and arrays. Why, pray tell, would you want to do these kinds of things?  Well lets look at a program that I have written to test is a thing is a thing:

```
const validateThing = async thing => {
  switch (thing) {

    case "car":
      return {
        valid: true,
        thing: "An expensive thing"
      }
    case "toaster":
      return {
        valid: true,
        thing: "Mysteriously makes bread disappear and toast appear"
      }
    case "soy sauce":
      return {
        valid: true,
        thing: "Delicious on rice, but a bit salty"
      }
  }
  return { valid: false, thing: "not a thing" }
}
```

By calling `validateThing('car')` we receive a very helpful little piece of information (a lesson my son is currently learning at this moment) that cars are expensive. We get a similar bit of information if we check toasters or soy sauce.  Now, I have been around for a while and I can tell you there are more things on this planet than cars, toasters, and soy sauce. As we add in more things our case statement starts to grow rapidly. Not only that but writing unit tests for this beast would be crazy. 

So let's do our first refactor.  We are going to take what is, essentially, a procedural program above and make it into a functional program.  We'll break the different validations into functions that can be called individually.

```
const validations = {
  car: async thing => 
  thing == "car" ? { 
    valid: true, 
    thing: "An expensive thing" 
  } : null,
// --- snippet ---
}
const validateThing = async thing => {
  const invalidThing = { valid: false, thing: "not a thing" }
  for (const validation in validations) {
    const res = await validations[validation](thing)
    if (res) return res
  }
  return invalidThing
}

```
For brevity, I removed some of the other things and just left the cars. So how does this change things? For one, if we want to add validations we just add them to our program and call them in `validateThing`. Each of these functions can be individually tested using unit tests and there is less chance that functions are going to interfere with other functions.  But there are some disadvantages.  
1. For every new function we add we have to update `validateThing`
2. What if we have different sets of "things"? Now we need to manage multiple `validateThing` functions

For our last refactor let's use higher-order functions. We'll add all of our validate functions to an object and pass that to our validate function.  We'll call this object `tangibleThings`. We'll also create another object called `intangibleThings` which can each be passed individually.

```
const tangibleValidations = {
  car: async thing =>
    thing == "car" ? {
      valid: true,
      thing: "An expensive thing"
    } : null,
// --- snippet --
}
const intangibleValidations = {
  love: async thing =>
    thing == "love" ?
      {
        valid: true,
        thing: "Something The Beatles sang a lot about."
      } : null,
  }
} 
```
Now we can pass two parameters to `validateThing`: our thing, and a list of validation functions to check it against.  For instance: `validateThing('love', intangibleValidations)` to find out what gives us butterflies in our stomach. We have now completely decoupled any of the implementations for validations from our `validateThing` function, made it extensible so that we can pass specific validation functions, and made it easily testable as we can test all of the individual validation functions independently.

Using higher-order functions in your applications is great, but it is not always the answer. There is some overhead and sometimes the juice isn't worth the squeeze. But when you are working with complex systems that have many moving parts it can make your life, and the lives of your teammates, exponentially better.