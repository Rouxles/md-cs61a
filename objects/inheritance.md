---
title: "Inheritance"
weight: 3
draft: true
authors: ["Antonio Kam"]
---
# Inheritance

Inheritance is a powerful tool that is very often used to reduce redundant code. If you have more specific versions of a larger class, inheritance can be extremely useful. Using our `Animal` class from the **Objects** notes, we can use all its general attributes, but then add more specific classes.

```python
class Animal:
    default_food = []
    def __init__(self, name, energy = 100):
        self.name = name
        self.food = self.default_food[:]
        self.energy = energy
        self.times_fed = 0

    def feed(self, food):
        self.food = self.food + [food]
        self.times_fed += 1
        self.energy += 10

    def play(self):
        if energy <= 20:
            return "Not Enough Energy"
        self.energy -= 20
        self.is_happy = True # Creates new instance variable
        return f"{self.name} has {self.energy} energy."

my_cat = Animal("Sochi")
my_cat.feed("Tuna")
```

Instead of making `my_cat` an instance of the `Animal` class, we could instead create a `Cat` class that **inherits** from the `Animal` class - meaning that it contains the same class, methods, and instance variables as the `Animal` class (which can then be overridden).

The syntax for creating a class inherited from another class is shown below:

```python
class Cat(Animal):
    <class body>
```

Right now, this will create a new `Cat` class pointing towards the `Animal` class we've already created, meaning that it can also access the class attributes/variables alongside the methods of the `Animal` class.

![Example of inheritance](https://i.imgur.com/hmO4kIL.png)

In the image above, we can see the `Cat` class we created pointing towards the `Animal` class, meaning that in terms of our lookup order, we first look to see if something exists in the instance, then `Cat`, then `Animal`.

For example, let's edit our `Cat` class a bit, so it has a use, then create an instance:

```python
class Cat(Animal):
    default_food = ["Tuna"]

sochi = Cat("Sochi")
```

When `sochi = Cat("Sochi")` is called, it first looks for an `__init__` method in its own class (`Cat`), which doesn't exist, so it looks up to its parent (`Animal`) for an `__init__`, which is found, so that `__init__` method is used. If there is no `__init__` method in the lookup, that method will simply not be called.

As a result of that, we get the following diagram (in this case, the `__init__` method is called from class `Animal` as that is the highest level where an `__init__` is defined.):

![](https://i.imgur.com/qBP8sJP.png)

> When looking for `self.default_food[:]`, `default_food` from the `Cat` class is taken, because the created instance first looks to see if there is a `default_food` already located in the instance (it doesn't), then looks to where the arrow is pointing, which is the `Cat` class, then looks for `default_food`, which is then found.

---

Our current version of the `Cat` class doesn't really do much. We can **override** our default methods by simply redefining them in our new class. The lookup order will make it such that only our newly defined method will be called.

```python
class Cat(Animal):
    default_food = ["Tuna"]

    def feed(self, food):
        self.food = self.food + [food]
        self.times_fed += 1
        self.energy += 10000
```

![](https://i.imgur.com/1mDi8gW.png)

Now our current diagram looks like this. If we call `sochi.feed("apple")`, it will look at the `feed` method defined in `Cat` and then call it because it exists in that class.

## The `super()` Function

The `super()` function, at least in the scope of CS 61A, looks at its parent class.

This could be useful if you wanted to use a method of the parent class but add a few extra details. The `super()` function automatically passes in `self`.

```python
class Cat(Animal):
    default_food = ["Tuna"]

    def __init__(self, name, energy = 100):
        super().__init__(name, energy) # will automatically define instance variables from the Animal class
        self.is_very_cute = True

    def feed(self, food):
        self.food = self.food + [food]
        self.times_fed += 1
        self.energy += 10000
```

Now, looking at our diagrams, we have the following:

![](https://i.imgur.com/xHtCpmI.png)

If we then create an instance of our newly created `Cat` class, we will do the following:

First, create our instance and find whether an `__init__` exists

![](https://i.imgur.com/SWftHKc.png)

Then, if it does exist, we execute it.

![](https://i.imgur.com/jPgsVWi.png)

## Use Cases

Explicit examples will not be given here because they should not be necessary. You can imagine the world of possibilities that you can do with classes and inheritance! Try thinking of how you would make a card game using OOP. Doing that will reduce A LOT of redundant code and make writing this card game easy. In fact, you might see this sort of thing show up in lab questions (alongside the Ants project).