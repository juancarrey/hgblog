+++
title = "Code Quality and Laziness"
date = "2020-09-14"
description = "How laziness has overcome code quality and developers tend to cut corners"
tags = [
    "clean code", "developers", "code quality",
]
author = "Juan Carrey"
+++

Software development in general is getting his feet on a deep mug of tooling derived from the inherited
laziness of many developers to overcome pressures and meet the demands of speed.

Speed has overcome any other metric on software development, quality among them, even outcomes which sounds nuts to me.

This is very dangerous, let's dive in why
## The future of software development

Every day there is more and more tooling around minimizing the lines of code needed to do CRUD, making CRUD apps in seconds.
The thing is, not everything is CRUD, and not everything fits the model we are used to seeing everywhere. In fact I belive almost nothing fits this model.

In tons of sample code out there, I see a repetitive pattern: The magnificent thin layered architecture, so thin that these layers just do nothing but
 passing a vertical, fully mutable entity full of getters and setters from layer to layer until reaching the database, 
 and back up until reaching the "RESTFul" API.

We just gave birth to **CRUDFull Layer Architecture**, aka L-CRUD (Lazy CRUD)

## Laziness

All this would not have happened if we, as software engineers say "No" to these "emerging as standard" methodology, and 
start thinking beyond the typical CRUD and start asking about meaningful actions to the domain.

Yes, it's probably more work, but is definetly worth it in the long term.

I have seen things like this:
``` 
var myThing = myThingService.get(id);
myThing.myProperty1 = newProperty1Value;
myThing.myProperty2 = newProperty2Value;
myThingService.save(myThing);
```

Looks familiar ? Run! I am not sure even where to start, this even has race conditions that can potentially override changes.
This type of coding also invites some lazy devs to copy and paste the code into multiple places and then the fun begins.

Here is something slightly different:

```
myThingService.handle(new UpdateCertainPropertiesCommand(id, newProperty1Value, newProperty2Value))
```

Yes, we will have to create all methods/functions to handle that action up to storage, but if we need to do this, 
is because it makes freaking sense from business domain side, otherwise we wouldn't have such a thing.

#### We achieve multiple things here:

* Abstracts on how the update actually happens, it might be domain driven behind the scenes, maybe even event-sourced, or even sent to a bus asynchronously,
we do not know, and we could not care less.
* Makes code easier to read and to reason about.
* There is no mutable entity passed around.
* Decouples the current context (**this**) from *myThingService*
* We are not L-CRUD gang anymore

## How this affects code quality

It usually starts with a clean CRUD, it works well for starting project, but things start to change, and soon
these layers will start to grow.

Having mutable shared entities mixed with imperative thinking and some lazy developers is the perfect combination for disaster. 
The result is a messy code, hard to follow and reason about, which will lead to poor code quality code going down the rabbit hole.

## Summary

* Immutability brings lots of benefits, use it more often
* Every layer has a domain, decouple them
* API is not always CRUD, think about business needs first
* "Accepted standards" are not panacea and sometimes they are trash
* Name the things properly, I know my samples are of no help, but I have no domain :)

There is other stuff to care about too, but I am just focusing on the ones I am seeing evolve in a dangerous direction.