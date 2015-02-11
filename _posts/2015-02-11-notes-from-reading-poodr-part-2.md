---
layout:     post
title:      "Notes from reading POODR (Part 2)"
subtitle:   "Ruby tips useful in writing good code"
share_text: "Ruby tips useful in writing good code - Notes from reading POODR (Part 2)"
date:       2015-02-11 12:00:00
author:     "Paweł Nguyen"
---

## Practical Object-Oriented Design in Ruby

These are notes I took while reading [POODR](http://www.poodr.com/) by [Sandi Metz](http://www.sandimetz.com/) a while ago.
If you're a Ruby dev and haven't read POODR, I highly recommend it.

All of code snippets below are examples from POODR and come from [this repository](https://github.com/skmetz/poodr).

Part 1 of those notes is [here](/2015/02/04/notes-from-reading-poodr-part-1/).

## Ruby tips

Here are hints on how to write better Ruby code I noted down. They are short and might be meaningless when out of context,
but they may still bring some value or refresh your memory if you read the book.

### Simple classes

Use `Struct` and `OpenStruct` in place of simple classes. Main difference is that `Struct` takes initialization arguments
in order, and `OpenStruct` accepts a hash from which it derives attributes.


<script src="http://gist-it.appspot.com/github/skmetz/poodr/blob/master/chapter_2.rb?slice=191:195"></script>
<script src="http://gist-it.appspot.com/github/skmetz/poodr/blob/master/chapter_8.rb?slice=404:407"></script>

### attr_reader

Use `attr_reader` or *private method* in objects instead of instance variables.
It turns data into behaviour at once and by doing so it hides data:

<script src="http://gist-it.appspot.com/github/skmetz/poodr/blob/master/chapter_2.rb?slice=12:23"></script>


### Constructors

Using `fetch(args[:something], default)` can be more handy than `args[:something] || default` because `fetch` returns `default`
only when key wasn't found. `||` returns `default` also when `args[:something]` is `nil` or `false`.


### Template Method Pattern

Template Method Pattern consists of a method that is used as default value. It is then overwritten in classes that inherit or include it:

<script src="http://gist-it.appspot.com/github/skmetz/poodr/blob/master/chapter_6.rb?slice=298:311"></script>

Using *hook methods* allows subclasses to optionally override behaviour. This way you can remove `super` calls and achieve less coupling:

<script src="http://gist-it.appspot.com/github/skmetz/poodr/blob/master/chapter_9h.rb?slice=16:33"></script>

### Composition

A `Bicycle` can be *composed of* `Parts` which respond to `spares` message. `Parts` holds many `Part` objects.

<script src="http://gist-it.appspot.com/github/skmetz/poodr/blob/master/chapter_8.rb?slice=106:140"></script>

Use `Forwordable` and `Enumerable` for classes like `Parts` to make it act similar to Arrays.

<script src="http://gist-it.appspot.com/github/skmetz/poodr/blob/master/chapter_8.rb?slice=243:257"></script>

## Summing up

I know that I'm praising POODR once again, but as you can see it contains not only design and architecture tips.
There's also a lot of practical advice on writing Ruby code. I noted down only those that were important for me at the time of
reading, there are a lot of other hints that can't be shortened to a note and are worth reading yourself.

You can find Part 1 of those notes [here](/2015/02/04/notes-from-reading-poodr-part-1/)