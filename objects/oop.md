---
title: "Object-Oriented Programming"
weight: 2
draft: false
authors: ["Antonio Kam"]
---

# Objects

## Object Oriented Programming (OOP)

OOP is a method for organizing programs. It includes

- Data Abstraction
- Bundling together related programs/information/behaviour

Each **object** can have its own local **state** (meaning its own variables), and also knows how to manage its own state.

OOP is particularly useful when you have many similar things that can be further generalized to avoid repeating redundant code. For example, you could have an `Animal` object, with other sub-classes of the `Animal` (for example, `Turtle`, `Cat`, `Bear`, etc.) - this (inheritance) will be covered in the next page.

## Terminology

- A **class** is a template for creating your own data type (for example, an Elephant class)
- An **instance** of a class is called an **object** (in this context, an instance would be one elephant in the Elephant class)
- Each **object** has its own variables called **instance variables** that describe its current state
- Each **object** can also have its own functions called **methods**
- Each **class** can have its own variables called **class variables** that contain information about the class itself

## Example

Let's make an Animal class.

I want each of my animals to have instance variables for food and happiness as well as methods (equivalent to functions) to have animals play with each other.

In Python, whenever we want to create an instance of a class, we just assign a variable to a call to the class:

The example below won't necessarily make sense for now, but I'll break it down later into smaller parts that will each be explained.

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

### Object Construction

In this case, `Animal(<args>)` is called the **constructor**.

When our constructor (`my_cat = Animal("Sochi")`) is called, it does the following:

1. Create a new instance of the `Animal` class
2. Call the `__init__` method of the class with the new instance as the first argument (passed in as `self`), along with any additional arguments in the call expression.
   - In this case, the new instance becomes `self`, and afterwards, is assigned to `my_cat`. This will hopefully make more sense after you see the diagram a bit below.
   - The Pythonic term for a variable wrapped in double underscores is called a **dunder** method.

### Instance Variables

```python
class Animal:
    default_food = []
    def __init__(self, name, energy = 100):
        self.name = name
        self.food = self.default_food[:]
        self.energy = energy
        self.times_fed = 0
```

**Instance Variables** are variables that each **instance** of an object (most likely) has, which describes the **state** of the object. These instance variables can be modified using each of the object's methods. Additionally, new instance variables can be assigned outside the constructor.

Dot notation here (for example with `self.name`) allows you to access all attributes of an object (including both instance variables and instance methods). The left-hand side does not need to be `self` - it can be anything that evaluates to an object instance (more on this later).

In this case, **self** refers to the instance being passed in - not every animal is the same, so `self` shouldn't refer to the same animal. This might be more clear with the next example.

#### Mutating Instance Variables

```python
class Animal:
    default_food = []
    def __init__(self, name, energy = 100):
        self.name = name
        self.food = self.default_food[:]
        self.energy = energy
        self.times_fed = 0

my_cat = Animal("Sochi")
```

First, let me draw a representation of the Animal class that we're referring to in the code block above. This is **not** the official Python Tutor environment diagram for a representation of classes, but this notation is a very clear way to see the **lookup order** of certain variables.

![Class Animal](https://i.imgur.com/mA2YUZR.png)

{{% notice note %}}
I haven't put every single method in the class 
{{% /notice %}}

Afterwards, when we call `my_cat = Animal("Sochi")`, we create a new instance of the class. I will denote a new instance with a circle rather than a rectangle, label the newly created instance with `my_cat` (the name of the variable assigned to it), then an arrow will be drawn to the class that this instance refers to. This will be even more important when we look at inheritance in objects.

![With instance](https://i.imgur.com/bvKwZS8.png)

Now, we can see that our variable `my_cat` has instance variables `name, food, energy, times_fed`, which can be mutated by either mutating `my_cat` or using methods, which we will show later.

You might be wondering where `self.food` comes from. In this case, `self.default_food[:]` will look for `default_food` in its own instance, which it can't find because nothing is defined, so it will then look at the 'frame' above, or in this case, the class itself for `default_food`, which it does contain. As a result, that value is used for `self.food`. You can think of this as variable lookup with different frames.

To mutate our instance, we can use `my_cat` to do so.

```python
my_cat.energy += 10
```

Running the following line of code will look for whatever `my_cat` is pointing to, which in this case, is the instance, then find `energy` and update that.

![Updated Instance](https://i.imgur.com/4FccVEA.png)

This is sort of like list mutation: you can access an element in the instance by using **dot notation**, then update it through assignment.

### Method Invocation

Calling `my_cat.feed("Tuna")` will invoke the following function:

```python
class Animal:
     def feed(self, food):
         self.food = self.food + [food]
         self.times_fed += 1
         self.energy += 10
```

{{% notice note %}}
This does the same thing as calling `Animal.feed(my_cat, "Tuna")` if you want to see where the `self` variable comes from
{{% /notice %}}

In this case, `my_cat` gets **implicitly** passed in as `self`, so the first argument in `my_cat.feed("Tuna")` (`Tuna`) will get passed in to `feed` as `food`.

As a result, we will update our instance variables following the instructions in the function, where `self` refers to the instance we made in the image above.

![Even more updated instance](https://i.imgur.com/To8ruMM.png)

{{% notice note %}}
Notice how we have only updated the **instance** variables but not the **class** variables related to the (located in the rectangle). The new methods in the `Animal` class were already defined before, I added them in the image above for clarity reasons - (it shows where the function comes from)
{{% /notice %}}
