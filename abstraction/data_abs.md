---
title: "Data Abstraction"
weight: 3
authors: ["Vanshaj Singhania"]
---

Say you have the following set of information:

```python
person = ['Ben', 'Franklin', '01/17/1706', '04/17/1790', 'Founding Father']
```

What can you discern about this set of information? That it represents a person's first name, last name, date of birth, date of death, and occupation/what they're known for. You might think this is a fairly convenient way to store a person, but what happens when you create hundreds such people? How do you access their dates of birth? Using `person[2]` of course, but let's think from a practical perspective.

Practically speaking, this is extremely unintuitive and serves us little purpose -- in the real world, a person's date of birth isn't their third-indexed quality. In fact, *we don't usually index people's characteristics at all!* So why should we do so in programming? If anything, this makes it pretty hard to keep track of what we're doing.

What if we could instead think of this "person" abstractly? We could pretend that the data we're storing is a person, rather than a list, and that could help us think more realistically about what we're doing.

## Constructing an Abstraction
An abstraction is simply the act of leveraging the features of a programming language to represent things in a similar way as we would in real life. As with a person IRL, the first step for us is to construct this person. How do we do that? Why, using functions of course!

```python
def createPerson(first, last, dob, dod, occuption):
    person = [first, last, dob, dod, occupation]
    return person
```

Notice that we're effectively doing the same thing as before -- creating a list with certain characteristics. The "abstraction" element comes into play when we *refer* to these characteristics.

## Getting Information
Let's create methods to get the full name, date of birth, and occupation of a person using our abstraction.

```python
def getName(person):
    return person[0] + " " + person[1]

def getBirth(person):
    return person[2]

def getOccupation(person):
    return person[4]
```

Again, notice that we're still accessing the elements of a list. How is this any better? Let's look at a scenario.

## Direct Access vs. Abstraction
Say I want to know what `mystery_bear`'s first and last name is. I can either do this:

```python
>>> mystery_bear[0] + " " + mystery_bear[1]
Abs Tract
```

Or I can do this:

```python
>>> getName(mystery_bear)
Abs Tract
```

Now, if I didn't have any programming experience and was to look into this program from the outside, I may or may not be able to describe what the first code segment does. However, if I understand English and am able to think logically, the second code segment is significantly easier to comprehend without programming knowledge -- it gets the name of `mystery_bear`!

## Goal of Abstraction
The goal of abstraction is for us as programmers to be able to cast away the under-the-hood part of implementing a program. If someone gave me a person and told me that I had access to these different getter methods, I no longer need to worry about *how* the person is stored. It could be a list, a tuple, an array, a dictionary, anything!

What does that imply? Say we implement people as lists. We write those getter methods, and we know exactly how the implementation works. We then hand this off to a future 61A student, who doesn't get to *look* at how things work. This student doesn't care -- as long as the getter methods are accessible, the fact that people are actually lists doesn't matter.

Now say we continue through 61A and learn about a new data structure -- dictionaries. I won't explain these right now, but just go with the flow. We realize that dictionaries are inherently better representations of people, so we do something new like so:

```python
# you *do not* have to understand this code! it simply replaces our list abstraction with something new.
# you'll see why in a second
def createPerson(first, last, dob, dod, occupation):
    person = {'first': first, 'last': last, 'dob': dob, 'dod': dod, 'occupation': occupation}
    return person

def getName(person):
    return person['first'] + " " + person['last']

def getBirth(person):
    return person['dob']

def getOccupation(person):
    return person['occupation']
```

Confused? Good! Why? Because you're trying to understand the way we store a person.

But you don't need to, because abstraction. These methods will function **the exact same way** as the methods we defined with our list-based representation of a person. That right there is the beauty of abstraction. It doesn't matter how the information is stored, because you (and our future 61A students) only need to know that the constructor and getter methods exist.

Say I hand you a new `mystery_bear2`, telling you that it's the right type of object for our new abstraction. How would you tell me this bear's name?

```python
>>> getName(mystery_bear2)
Richard Shenary
```

Did you need to know that the bear was a dictionary? No! Heck, I could've given you an encrypted version of the bear and told you that you had a set of getter methods to use on it and the approach would remain the same. Once we've implemented an abstraction, we don't need to worry about how it works. We simply need to be able to access its elements, which is why we create these abstractions to begin with.

If you're still confused, here's a perhaps more relatable example. Say you're using a messaging app that sends your messages directly to your friends when you hit send, without encrypting them or anything. Now, say the app developers realized that this is a bad idea, and they update your app so that when you hit send, your messages are first encrypted somehow, then sent. Does that change the way you send messages? Not at all. You still go through the same process of typing and sending, but what happens under the hood is different. This change doesn't affect *your* interaction with the app at all.

This is exactly what abstraction is. If I gave you a person constructor and a few methods to get this person's name or age or whatever, you don't need to care about what the person looks like in terms of data structures. Even if it's one long string such as `long_ben = "Ben|Franklin|01/17/1706|04/17/1790|Founding Father"`, as long as I give you the methods and tell you they work, all you have to do to get this person's name is `getName(long_ben)`, and trust that I'm not lying about my implementation of `getName`.