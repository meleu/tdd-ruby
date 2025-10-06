# Object-Oriented Design

> **NOTE**:
>
> If you are already familiar with OOP in Ruby, and concepts like constructor method, instance variables, and sending messages, then you can safely skip this chapter.

In the introduction of this book I told that we'll inevitably talk about **Object-Oriented Programming** (OOP) and **Software Design**. The combination of these concepts can be called **Object-Oriented Design**. In this chapter we'll see a short description of these concepts. And apply them in the context of the Ruby language.

Note that the code shown in this chapter is just an illustration of how Ruby code looks like. Don't worry if you're not familiar with that, you'll learn all that as you progress through the book.

## What is Software Design?

My favorite definition comes from [Sandi Metz](https://www.poodr.com/):

> Every application is a collection of code; **the code's arrangement is the *design***.

Although the way we organize our code can influence how computers execute it, the main target audience is the humans who need to maintain this code over time.

We organize our code into functions, classes, methods, modules, etc., so we can have a better understanding of our software. With this organization we hope to be able to, effortlessly, change our application whenever necessary - usually implementing new features or fixing bugs.

When we organize our code thinking in objects, we're doing Object-Oriented Design. But what is this OO thing?

## What is Object-Oriented Programming

First let's remember how procedural programming works.

In procedural languages there's a clear distinction between data and behavior. We put the data into variables and pass them to functions (the behavior). Although we commonly use both data and behavior to solve the same sort of problem, there's a conceptual distance between them. Such distance sometimes brings confusion and unpredictable problems. Object-Oriented Programming is an attempt to prevent such problems.

A simplistic way to describe OOP is to say that it's a way of grouping both data and behavior into the same concept. An object holds the data needed to solve a problem and also the logic that performs operations on that data in order to solve the problem.

Below we'll see the main OOP concepts with a Rubyist perspective.

### Classes and Objects

In OOP we usually model concepts from the real world in our code. During this process we usually think in categories of things that we want to represent in code. For example, we want to model the concept of "something that says `Hello, World`", then we need to think in an entity that could encapsulate this "something". Let's say, a Greeter class.

In order to represent the entities, we create a **class**. In a class we can define the data we need (e.g. the greeter's default language) and the behavior that use that data (e.g. to greet people properly).

Here's an example of Ruby code implementing a class named Greeter and its behavior of returning the `Hello, World!` string.

```ruby
class Greeter
  def hello
    "Hello, world"
  end
end
```

A class is like a blueprint of the concept you're modeling. And to make use of such concept we usually create *instances* of the classes, also known as **objects**.

In order to create an object from a class, we use a constructor method.

### Constructor method

In Ruby we create objects by calling the `new` method, which is known as the constructor method.

One interesting thing is that when we call the `new` method, Ruby actually executes the `initialize` method. If you want to initialize your new object with some data, then you need to implement an `initialize` method for your class.

As an example let's create the `initialize` for our Greeter class, where we set the language to be used in the greetings:

```ruby
class Greeter
  def initialize(language)
    @language = language
  end

  # ...
end
```

Here are some examples of how it would be called to create greeter objects with different languages:

```ruby
spanish_greeter = Greeter.new("spanish")
japanese_greeter = Greeter.new("japanese")
```

### Instance variables

Both objects in the previous example were created from the same class, but have unique characteristics. We passed different arguments to the constructor method, and the contents of those arguments were stored in an **instance variable** named `@language`.

Instance variables have this name because they are variables with values that are unique to each instance. In our example we use an instance variable to store the greeter's default language.

### Instance methods

When we create classes we can also define **instance methods**, representing the behavior of the object. They hold functionalities that may be used to solve something. For example, we could have an instance method named `hello` to greet someone using the greeter's default language (or using a specified one). For example:

```ruby
# instantiating a greeter object:
german_greeter = Greeter.new("german")

# invoking the `hello` method:
german_greeter.hello("Johann")
#=> "Hallo, Johann"
```

Creating a hello method able to greet people in different languages is what we are going to do in the next chapter.

### Calling a method or sending a message?

In Ruby we invoke methods by sending a message to an object. In the previous example we sent the `hello` message, along with the name `Johann`, to the `german_greeter` object.

If you are coming from another OOP language, you're probably used to use the term "calling a method" instead of "sending a message", but in Ruby literature we use the message sending terminology.

When an object receives a message, it looks into its own class for a corresponding method. If found, the method is invoked, if not, an exception is raised (but that is a topic for a later chapter).

## Key Concepts

In this chapter we learned that Software Design is basically the way we organize our code.

We also saw that OOP is a nice way to design our software by grouping together data and behavior under the same concept.

We briefly saw concepts like:

- classes
- constructor method
- objects
- instance variables
- instance methods

Of course there are much more to say about these topics, but with what we have so far we're able to go ahead and start coding. We will learn more as we progress through the book.
