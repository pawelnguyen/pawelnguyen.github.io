---
layout:     post
title:      "Notes from reading POODR (Part 1)"
subtitle:   "Single Responsibility, Dependencies, Antipatterns and Recognizing Hidden Ducks"
date:       2015-02-04 12:00:00
author:     "Paweł Nguyen"
---

## Practical Object-Oriented Design in Ruby

These are notes I took while reading [POODR](http://www.poodr.com/) by [Sandi Metz](http://www.sandimetz.com/) a while ago.
If you're a Ruby dev and haven't read POODR, I highly recommend it.

All of code snippets below are examples from POODR and come from [this repository](https://github.com/skmetz/poodr).


## Best time for Design decisions

When to take design decisions? If cost of doing nothing is the same now and in future, postpone design decision until
more information comes. On the other hand if there is chance that sub-optimal architecture will be reused by someone or replicated,
it should be fixed early. A tension exists between improving now and improving later.


## Determining if a class has a single responsibility

We need to interrogate this class, ask it questions and see if it makes sense:

`Mr Gear, what is your ratio?` is fine, but `Mr Gear, what is your tire (size)?` is obviously not.

Another way is to try describing in one sentence what it does, without "or"/"and".



## Dependencies

### Removing argument order dependencies

Remove argument order dependencies by using `args` and `fetch`.

<script src="http://gist-it.appspot.com/github/skmetz/poodr/blob/master/chapter_3.rb?slice=169:174"></script>

We can do `def initialize(chainring, cog, wheel)`. But if we use `args`, we can instantiate this class with any order of arguments.

### Injecting dependencies

*Inject dependencies* in order to decouple code.

<script src="http://gist-it.appspot.com/github/skmetz/poodr/blob/master/chapter_3.rb?slice=54:70"></script>

This way `Gear` doesn't know that `Wheel` class exists, its instance was injected.

### Isolating dependencies

*Isolate dependencies* when dependency injection can't be achieved.

<script src="http://gist-it.appspot.com/github/skmetz/poodr/blob/master/chapter_3.rb?slice=86:101"></script>

Moving `Wheel` dependency to separate method is a good way to isolate it.

### Reversing dependencies

Call method with dependent arguments to *reverse dependencies*.

<script src="http://gist-it.appspot.com/github/skmetz/poodr/blob/master/chapter_3.rb?slice=263:299"></script>

This example shows how to reverse dependencies. Now `Gear` doesn't know anything about `Wheel`.

### Dependencies direction

Classes should depend on things that change less often than they do.


## Antipatterns

### Inheritance

Use inheritance when object has a switch/if based on type/category variable and tries to determine what methods send to self
based on that.


### Recognizing hidden ducks

Coding patterns that indicate *hidden ducks*:

- `case` that switch on class
- `kind_of?` and `is_a?`
- `responds_to?`

<script src="http://gist-it.appspot.com/github/skmetz/poodr/blob/master/chapter_5.rb?slice=63:71"></script>

This gist shows how to handle hidden ducks. Instead of switching through preparers and
[calling methods](https://github.com/skmetz/poodr/blob/master/chapter_5.rb#L30) on parts of trip, we let them do the hard
work instead and encapsulate this logic where it belongs - in preparers.

### Duck type with shared behaviour
A `module` can be used instead of duck type with shared behaviour/role.

<script src="http://gist-it.appspot.com/github/skmetz/poodr/blob/master/chapter_7.rb?slice=51:72"></script>

This snippet shows how to use modules in situations when we have a duck type (`schedule` - can be swapped thanks to `attr_writer`)
and shared behaviour.

## Other

### Law of Demeter

Objects should talk only to their immediate neighbours. Avoid "train wrecks" like `comment.post.blog.author`. Doing so reduces
code coupling and amount of dependencies.

### Composition over inheritance

Use composition when having choice. Composition matches more `has-a`, inheritance `is-a`
relation.

Composition allows objects to have structural independence at the cost of explicit message delegation.
Inheritance gives you message delegation for free at the cost of maintaining a class hierarchy.

## Summary

Each of the topics I mentioned in this post is very broad and each of them could be described in detail in a separate
blog post. But the idea here was to gather notes that are essence of POODR and that help me in my day-to-day work.
If they helped you as well, don't hesitate to share them.