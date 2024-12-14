---
layout: post
title: The Joy of Singleton
date: 2024-02-05 00:00:00 +0300
description: The Joy of Singleton
img: singleton.jpg # Add image post (optional)
tags: [Js, Node] # add tag
---
There are a lot of people who will tell you that a Singleton is an anti-pattern. Be that as it may, they are something you are going to come across at some point in your development travels and it is good to know about them.

A Singleton is pretty much what it sounds like. It is a single function/object that can't be replicated.  Any time you instantiate another instance, instead of creating a copy, it returns the single instance of the object. This is somewhat of a controversial design pattern because anything can make changes to the object which results in similar problems to those you encounter with global scope: lack of encapsulation, mutable values, and side effects.

You may find, however, that the pros outweigh the cons. Let's imagine for a moment that we are creating the earth (eep). Our planet is going to have all the necessities of a world: sea, earth, and, of course, critters.

```
function World() {
    this.earth = []
    this.water = []
    this.creatures =[]
}
```

Now let's instantiate our world and add a critter:

```
const myWorld = new World()
myWorld.creatures.push("Kangaroo")
```

Wonderful.  Our world is soon going to be brimming with life. Unfortunately, we run into a problem if elsewhere in our program, you instantiate the world again.  

```
const yourWorld = new World()
console.log(yourWorld)
```
This is going to return a planet completely barren of critters, including the kangaroo we added earlier. This is not surprising given we instantiated a new world, but this is not what we want for our Singleton. Instantiating a new world should give us a reference to that same function we created earlier. So, let's implement our function as a Singleton.

```
const World = function () {
  if (World._instance) {
    return World._instance
  }
  World._instance = this
  this.earth = []
  this.water = []
  this.creatures = []
}
```
Now when we instantiate a World it is going to check if an instance of the world already exists.  If it does, it'll return it, otherwise it will create one.  Now, if we create `myWorld` and `yourWorld`, adding a critter to one will be reflected in the other:

```
const myWorld = new World()
const yourWorld = new World()

myWorld.creatures.push("Kangaroo")
console.log(myWorld)
// World { earth: [], water: [], creatures: [ 'Kangaroo' ] }
console.log(yourWorld)
// World { earth: [], water: [], creatures: [ 'Kangaroo' ] }
```

You have yourself a Singleton. You may not realize it but you are probably already using them. If you are using ESM import/export modules those are Singletons. Hopefully, this gives you a better understanding of how they work under the hood, and feel more comfortable using Singletons when the need arises.