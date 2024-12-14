---
layout: post
title: Working with Function Parameters in JavaScript
date: 2024-01-27 00:00:00 +0300
description: Working with Function Parameters in JavaScript
img: funParam.jpg # Add image post (optional)
tags: [Js, Node] # add tag
---
A function signature describes what is necessary when you call a function.  For instance, if we have a function whose signature is `makeDinner(meat, carb, vegetable)`. This includes all the information you need to execute the function.  

Let's say we wanted to create a bird-watching app (growing up my brother, and by extension, I, were avid bird-watchers). We might want to create a bird object in our app which we can initialize a bird with a few pieces of information. Let's start with name, wingspan, and mass:

```
const Bird = ( name, wingspan, mass ) => {
  return {
    fly: () => wingspan < 8 ? "flap, flap" : "FLAP, FLAP",
    stats: () => `${name}\nwingspan: ${wingspan} ft\nmass: ${mass}`
  }
}

const bird = Bird('California Condor', 9.5, 20)
```
This is good if we have all the necessary pieces of information at the time of initiation. What if we don't though? How do we make sure we can create an object with just the information we have?  If, for instance, we don't have the mass we can simply leave it blank and let JavaScript do with it what it will.
```
const bird = Bird('California Condor', 9.5)
```
When we call `bird.stats()` we'll get:
```
California Condor
wingspan: 9.5 ft
mass: undefined
```
Not ideal, but it works.  We could add default values to our parameter list to avoid the undefined:
```
const Bird = ( name, wingspan = 10, mass = 10) 
```
so that if we don't provide a value it selects a reasonable value. OK, maybe not the best as we wouldn't want to end up creating a bluebird that weighs 10 lbs and has a 10 ft wingspan, but let's just roll with it.

The bigger issue comes in if we have to add a new parameter to the function signature.  Say, for instance, we now want to track a bird's conservation status.  The new signature is:
```
const Bird = ( 
  name, 
  wingspan = 10, 
  mass = 10, 
  conservationStatus = 'least concern'
)
```
We added it as the last parameter in our function which is good because it doesn't break any of our existing functions. If we don't update any of our functions they'll just use the default values.  But what if we want to update the call that we issued earlier where we used the default value for mass:
```
const bird = Bird('California Condor', 9.5)
```
we can't simply tack the new value on the end.  We would have to add both the mass and the conservation status:
```
const bird = Bird(
  'California Condor', 
  9.5, 
  10, 
  'Critically Endangered'
)
```
This means to start leveraging this new parameter we may affect other, already functioning parts of our application. That is why I prefer passing objects as parameters:
```
const Bird = ({
  name,
  wingspan = 10,
  mass = 10,
  conservationStatus = 'least concern' }) => {
  return {
    fly: () => wingspan < 8 ? "flap, flap" : "FLAP, FLAP",
    stats: () => `${name}\nwingspan: ${wingspan} ft\nmass: ${mass}`
  }
}
const bird = Bird({
  name: 'California Condor',
  wingspan: 9.5,
  conservationStatus: 'Critically Endangered'
})
```
Why is this better? Well, now it doesn't matter if you did or did not specify one of the parameters previously.  Adding the new value is just adding a new attribute to the object you are passing to the function. It doesn't matter the order they get passed, or what position the missing field is in.  It also makes the function call much more explicit.  We now know exactly which values correspond with which attributes without having to count parameters and constantly look back at the function definition.

Object parameters are a great way to create functions that are easily extensible and less rigid. Put yourself in the shoes of the person who is going to be maintaining that code six months or a year from now (especially because that may be you) and create something maintainable and intuitive. 