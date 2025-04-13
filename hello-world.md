# Hello, World

It is traditional for your first program in a new language to be [Hello, World](https://en.m.wikipedia.org/wiki/%22Hello,_World!%22_program). So, let's do it!

Open your terminal and create a `hello` directory:

```bash
# the env var we created in the previous chapter
cd $TDD_RUBY_PATH

# create the directory
mkdir hello

# go into the directory
cd hello
```

Open your favorite editor, create a new file called `hello.rb` and put the following code inside it:

```ruby
puts("Hello, World")
```

To run it type `ruby hello.rb` in your terminal and hit `<Enter>`. Then you should see the string `Hello, World` being printed in your screen.

### How it works

The `puts` instruction stands for "Put String", and what it does is to put the string you give to it in the output.

In our code we're giving the "Hello, World" string.

## How to test

How do you test this program?

The answer for this question is our first example of how TDD promotes software design.

To make our program testable it is good to separate our "domain[^1]" code from the outside world (side-effects[^2]). In our program the greeting string is our domain, and the `puts` is a side effect (printing to the standard output).

Let's separate these concerns so we can easily test our domain's code.

As we want to use Object-Oriented Design, we're going to create a class named `Greeter` with a method named `#hello` to generate the string with the greeting. Now our code should look like this:

```ruby
class Greeter
  def hello()
    return("Hello, World")
  end
end

greeter = Greeter.new()
puts(greeter.hello())
```

The method simply returns the string `Hello, World`. Then we created an instance of `Greeter` and invoked the `hello` method, passing the results to `puts`.

### Idiomatic Ruby

I'd like to call your attention to the fact that the code above has a lot of parentheses. If you're coming from another programming language you may think it's perfectly normal.

In Ruby the use of these parentheses are usually optional. They may be required to clear up what may otherwise be ambiguous in the syntax, but in simple cases the Ruby programmers usually omit them.

Let's code like a real Rubyist and clear the unnecessary parentheses from our code:

```ruby
class Greeter
  def hello
    return "Hello, World"
  end
end

greeter = Greeter.new
puts greeter.hello
```

Another Ruby syntactic sugar[^3] is that the last evaluated expression of a method is always returned. Therefore the use of the `return` in the last line of that method is unnecessary.

So, let's clear our code a little more:

```ruby
class Greeter
  def hello
    "Hello, World"
  end
end

greeter = Greeter.new
puts greeter.hello
```

That's much easier on the eyes, don't you agree?

### Separation of Concerns

As mentioned before, we created a class to achieve a separation of concerns. It was created specifically to handle the greeting string, but how this string is going to be used is not the class's concern.

A common Rubyist practice is to put classes in their own file and name it after the class but using `snake_case`. Then let's create our `greeter.rb`:

```ruby
class Greeter
  def hello
    "Hello, World"
  end
end
```

As our class is now in a different file, the `hello.rb` needs to require that file to have access to the Greeter class.

Here's how the `hello.rb` must look like:

```ruby
require_relative 'greeter'

greeter = Greeter.new
puts greeter.hello
```

Just to check if everything is fine, type `ruby hello.rb` in your terminal again. You should still see the string `Hello, World` being printed in your screen.

### Writing your first test

Now create a new file called `greeter_test.rb` where we are going to write tests for our `Greeter` class.

Don't worry if you don't understand everything. After writing and running this test file I'll explain this code.

```ruby
require 'minitest/autorun'
require_relative 'greeter'

class TestGreeter < Minitest::Test
  def test_say_hello_world
    greeter = Greeter.new
    actual = greeter.hello
    expected = "Hello, World"
    assert_equal expected, actual
  end
end
```

The next step is to run the test.

Enter `ruby greeter_test.rb` in your terminal and you should see something like this:

```
$ ruby greeter_test.rb
Run options: --seed 20392

# Running:

.

Finished in 0.001096s, 912.1682 runs/s, 912.1682 assertions/s.

1 runs, 1 assertions, 0 failures, 0 errors, 0 skips
```

If you see an output similar to this 👆 then your test passed!

### Minitest "rules"

Writing a test with Minitest is like writing a method in a class, but with some "rules" (some of them are not actual strict rules)

- The file should be named like `*_test.rb` (not mandatory, but a good practice)
- The file must `require 'minitest/autorun'` so we can run the tests from the command line.
- The file must `require_relative 'greeter'` so it can access the code being tested.
- The test must be written in a subclass of `Minitest::Test` (don't worry if you still don't know what a subclass is)
- The test method must start with `test_`
- The assertion determines if the test is to be considered successful or not. In this case we are asserting that the expected value is equal to the actual value using `assert_equal expected, actual`.

In our `greeter_test.rb` code we're covering some new Ruby concepts:

#### require and require_relative

When a file needs the code defined in another file, a way to access it is requiring the "another file".

When we use `require`, we're getting the code from a package installed in our system (in Ruby we call such packages as "gems").

When we use `require_relative`, we're getting the code from a file stored in a path relative to the current file. In our case, as both `greeter_test.rb` and `greeter.rb` are in the same directory, we can simply use `require_relative 'greeter'` (the `.rb` extension can be omitted).

#### creating a subclass

In the code below we're creating a new class named `TestGreeter` as a subclass of `Minitest::Test`:

```ruby
class TestGreeter < Minitest::Test
  # ...
end
```

The concept of class and subclass will be explained in another moment. For now just keep in mind that when we create the `TestGreeter` as a subclass of `Minitest::Test`, this means that `TestGreeter` inherits the behavior defined in `Minitest::Test`.

#### The actual test method

This is the method where we are actually testing our hello method:

```ruby
def test_say_hello_world
  greeter = Greeter.new
  actual = greeter.hello
  expected = "Hello, World"
  assert_equal expected, actual
end
```

We create a greeter object, then call the `hello` method, then define the expected result, and then finally assert if the expected result is equal to the actual result.

When we ran this test it passed, which is nice. Now let's move on...

## Hello, YOU

In the last example, we wrote the test _after_ the code had been written. We did this so that you could get an example of how to write a class, a method and create a test for it. Now we're going to delete that test we just wrote a start a fresh one. From this point on, we will be _writing tests first_, and then the implementation (usually called _production code_).

This is basic test-driven development and allows us to make sure our test is truly testing what we want. When you retrospectively write tests, there is the risk that your test may continue to pass even if the code doesn't work as intended.

Our next requirement is to specify the recipient of the greeting. Let's start by capturing these requirements in a test (remember to delete the previous one). The `greeter_test.rb` now looks like this:

```ruby
require 'minitest/autorun'
require_relative 'greeter'

class TestGreeter < Minitest::Test
  def test_hello_to_people
    greeter = Greeter.new
    actual = greeter.hello("John")
    expected = "Hello, John"
    assert_equal expected, actual
  end
end
```

Now run `ruby greeter_test.rb` and you should see an error like this:

```
  1) Error:
TestGreeter#test_hello_to_people:
ArgumentError: wrong number of arguments (given 1, expected 0)
    greeter.rb:2:in `hello'
    greeter_test.rb:7:in `test_hello_to_people'
```

It's important to pay attention to the error message here, it gives us the information we need to figure out what's wrong with our code.

The Ruby interpreter is telling what you need to do to continue. In our test we passed an argument to the `#hello` method, but it is not prepared to receive an argument. That's why Ruby is telling us: `wrong number of arguments (given 1, expected 0)`.

Edit the `#hello` to accept an argument. Now your `greeter.rb` should look like this:

```ruby
class Greeter
  def hello(name)
    "Hello, World"
  end
end
```

If you try and run your tests again, you should see a failure with this message:

```
  1) Failure:
TestGreeter#test_hello_to_people [greeter_test.rb:9]:
Expected: "Hello, John"
  Actual: "Hello, World"
```

The test is now failing because it's not meeting our requirements.

Let's make the test pass by concatenating the `name` we passed as argument to the string we want to return.

```ruby
def hello(name)
  "Hello, " + name
end
```

When you run the tests now, it should pass.

Normally, as part of the TDD cycle, we should now _refactor_.


### A note on source control

At this point, we have working software backed by a test. It's a good time to commit our code:

```bash
git add hello.rb greeter.rb greeter_test.rb
git commit -m 'wip: hello-world'
```

Don't push to `main` though, we are going to refactor next. It is nice to commit at this point in case you somehow get into a mess with refactoring - you can always go back to the working version.

### Refactor: string interpolation

The string concatenation is working fine, but the usual way to achieve this kind of result is by interpolation. In Ruby, we do this with `#{variable_name}`, like here:

```ruby
def hello(name)
  "Hello, #{name}"
end
```

Run the test and it should pass.

## Hello, world... again

The next requirement is when our method is called with no arguments, it defaults to printing `Hello, World`, rather than `Hello, `.

As TDD practitioners, we write the _tests first_, so let's write a new failing test.

```ruby
class TestGreeter < Minitest::Test
  # ...

  def test_say_hello_world_when_called_with_no_args
    greeter = Greeter.new
    actual = greeter.hello
    expected = "Hello, World"
    assert_equal expected, actual
  end
end
```

Do note that the test method now have a descriptive (and long) name. It's important to give descriptive names to your tests, so you can know what to do when they fail.

After running the tests we'll see an error message like this:

```
  1) Error:
TestGreeter#test_say_hello_world_when_called_with_no_args:
ArgumentError: wrong number of arguments (given 0, expected 1)
    greeter.rb:2:in `hello'
    greeter_test.rb:14:in `test_say_hello_world_when_called_with_no_args'
```

That error message is telling us that:

- the `TestGreeter#test_say_hello_world_when_called_with_no_args` failed
- it failed due to the `ArgumentError`, because the `#hello` expected 1 argument and we didn't give any.

Let's check our `#hello` again:

```ruby
def hello(name)
  "Hello, #{name}"
end
```

Now we have a dilema:

- `#hello` expects an argument so we can use `hello("John")`
- we also want to be able to call `#hello` with no arguments and it should respond with `"Hello, World"`

To solve this we can define a default value for the `name` argument, like this:

```ruby
def hello(name = 'World')
  "Hello, #{name}"
end
```

As we're defining a default value for the `name` variable, calling `#hello` with an argument is optional. Because if none is passed, it uses the default value.

Run the tests and you should see a successful run. It satisfies the new requirement and we haven't accidentally broken the other functionality.

It is important that your tests are _clear specifications_ of what the code needs to do.

### Back to source control

Now that we are happy with the code, let's amend the previous commit to check in the new version of our code with its test.

Example:

```bash
git add greeter.rb greeter_test.rb
git commit --amend -m 'feat: TDDing hello world'
```

### Discipline

Let's go over the cycle again:

- Write a test
- Run the test, see it fails and check the error message
- Write enough code to make the test pass
- Refactor

On the face of it this may seem tedious, but sticking to the feedback loop is important.

Not only does it ensure that you have _relevant tests_, it helps ensure _you design good software_ by refactoring with the safety of tests.

Seeing the test fail is an important check because it also lets you see what the error message looks like. As a developer it can be very hard to work with a codebase when failing tests do not give a clear idea as to what the problem is.

By ensuring your tests are _fast_ and setting up your tools so that running tests is simple, you can get in to a state of flow when writing your code.

By not writing tests, you are committing to manually checking your code by running your software, which breaks your state of flow. You won't be saving yourself any time, especially in the long run.

## Keep going! More requirements

A new requirement arrived! The greeter needs to be able to greet people in a different language.

Our  `Greeter` currently defines only a method (the behavior), but in the [previous chapter](ood.md) we mentioned that OOP is about grouping together data and behavior. Now it's a good time to make use of an instance variable to hold the greeter's language.

We're going to achieve this by defining the language when we create the greeter object, passing the language to the constructor method.

Of course, we'll use TDD to flesh out this functionality.

### Hola

Let's write a test creating a Spanish greeter and calling the `#hello`.

```ruby
def test_spanish_greeter
  spanish_greeter = Greeter.new("spanish")
  actual = spanish_greeter.hello("Juan")
  expected = "Hola, Juan"
  assert_equal expected, actual
end
```

Remember not to cheat! _Test first_.

```
# Running:

E..

Finished in 0.001163s, 2579.5512 runs/s, 1719.7008 assertions/s.

  1) Error:
TestGreeter#test_spanish_greeter:
ArgumentError: wrong number of arguments (given 1, expected 0)
    greeter_test.rb:20:in `initialize'
    greeter_test.rb:20:in `new'
    greeter_test.rb:20:in `test_spanish_greeter'

3 runs, 2 assertions, 0 failures, 1 errors, 0 skips
```

We can see that our first two tests are still passing, but the new one is getting an error: `ArgumentError: wrong number of arguments (given 1, expected 0)`.

A curious thing is that the error message says the problem happened in the `initialize` method. This happens because in Ruby, when we create an object with `#new`, it invokes the `#initialize` method. By default `#initialize` doesn't do anything, but we can **override** this behavior by defining it in our class. Then, let's do it!

Put the `#initialize` method as the first method of your class:

```ruby
class Greeter
  def initialize(language)
    @language = language
  end
  # ... rest of the code...
end
```

Here we're defining an **instance variable** named `@language`. Any variable prefixed with an `@` symbol is considered an instance variable. It belongs to an specific instance of the class and can be accessed in any method.

Our constructor method is assigning the value passed as argument to the `@language` instance variable.

Now let's run the test and check the output:

```
# Running:

EFE

Finished in 0.002096s, 1431.0492 runs/s, 477.0164 assertions/s.

  1) Error:
TestGreeter#test_say_hello_world_when_called_with_no_args:
ArgumentError: wrong number of arguments (given 0, expected 1)
    greeter.rb:2:in `initialize'
    greeter_test.rb:13:in `new'
    greeter_test.rb:13:in `test_say_hello_world_when_called_with_no_args'

  2) Failure:
TestGreeter#test_spanish_greeter [greeter_test.rb:23]:
Expected: "Hola, Juan"
  Actual: "Hello, Juan"

  3) Error:
TestGreeter#test_hello_to_people:
ArgumentError: wrong number of arguments (given 0, expected 1)
    greeter.rb:2:in `initialize'
    greeter_test.rb:6:in `new'
    greeter_test.rb:6:in `test_hello_to_people'

3 runs, 1 assertions, 1 failures, 2 errors, 0 skips
```

😱 Our change broke all tests!!!

Always keep this in mind: if your change breaks tests that are unrelated to your current work, you're probably doing something wrong!

Tests are a safety net that brings confidence to change the code with no fear. If tests fail because you've broken the code, the cure is simple: undo the last change and make a better one.

In our case here we're breaking the previous tests because we added a new _mandatory_ argument to the `#new` method: `language`. In order to fix this we should make it optional by setting a default value for it.

```ruby
class Greeter
  def initialize(language = 'english')
    @languague = language
  end
  # ... rest of the code...
end
```

Let's run the tests:

```
# Running:

..F

Finished in 0.001142s, 2627.1267 runs/s, 2627.1267 assertions/s.

  1) Failure:
TestGreeter#test_spanish_greeter [greeter_test.rb:23]:
Expected: "Hola, Juan"
  Actual: "Hello, Juan"

3 runs, 3 assertions, 1 failures, 0 errors, 0 skips
```

Good, now we have a failing test output with a clear direction about what must be done to make it pass. It's expecting a greeting with `Hola` and our code is greeting with `Hello`, so let's fix our `#hello`:

```ruby
def hello(name = 'world')
  if @language == 'spanish'
    greeting = 'Hola'
  else
    greeting = 'Hello'
  end

  "#{greeting}, #{name}"
end
```

Run the tests and you'll see it pass. Time for refactoring.

For now, I decided to make no changes, then let's move on.

### Bonjour

Let's add a test for the French language, by following the steps:

- Write a test asserting that if you pass in `"french"` you get `"Bonjour, "`
- See it fail, check the error message
- Do the smallest reasonable change in the code

```ruby
def test_french_greeter
  french_greeter = Greeter.new('french')
  actual = french_greeter.hello('Jean')
  expected = 'Bonjour, Jean'
  assert_equal expected, actual
end
```

The test will fail with:

```
  1) Failure:
TestGreeter#test_french_greeter [greeter_test.rb:30]:
Expected: "Bonjour, Jean"
  Actual: "Hello, Jean"
```

Then let's write enough code to make the test pass.

```ruby
def hello(name = 'world')
  if @language == 'spanish'
    greeting = 'Hola'
  elsif @language == 'french'
    greeting = 'Bonjour'
  else
    greeting = 'Hello'
  end

  "#{greeting}, #{name}"
end
```

Run the tests and you'll see it pass.

Time for _refactoring_. Let's take this opportunity to learn how to use the `case` statement.

```ruby
def hello(name = 'world')
  case @language
  when 'spanish'
    greeting = 'Hola'
  when 'french'
    greeting = 'Bonjour'
  else
    greeting = 'Hello'
  end

  "#{greeting}, #{name}"
end
```

After the change run the tests again to make sure you didn't break anything.

The code is working as expected, but I'm starting to feel like `#hello` is accumulating too much logic in it.

I want to create a new method just to handle the multilingual greeting. As such method is not intended to be called from the "outside world", but only from the `#hello` method, we can define it as a **private method**.

A private method can only be used internally by the object. Any attempt to call it from outside will result in an error. For example, if you try to execute `greeter.greeting`, you'll get an error.

Alright, with the `#greeting` method our `greeter.rb` becomes like this:

```ruby
class Greeter
  def initialize(language = 'english')
    @language = language
  end

  def hello(name = 'World')
    "#{greeting}, #{name}"
  end

  private def greeting
    case @language
    when 'spanish'
      greeting = 'Hola'
    when 'french'
      greeting = 'Bonjour'
    else
      greeting = 'Hello'
    end
    greeting
  end
end
```

Run the tests and it should pass.

One question that might arise here is: "shouldn't we create a test for the greeting method?". And my answer for this case is: **no**. We want to validate the desired behavior, which is to greet people in a proper language. Our test suite is validating this behavior through the `#hello` method.

OK, moving on...

Passing tests triggers a decision for us: refactor or stop coding this feature?

I still want a refactoring to make the `#greeting` method more idiomatic.

First, Rubyists usually put the `private` keyword in a single line. When we do this, all methods below that line is considered private.

```ruby
class Greeter
  # ... code...

  private # code below this line is considered private

  def greeting
    # ...
  end
end
```

Run the test and it should pass. Next refactoring...

Remember when I said that the last evaluated expression of a ruby method is always returned? The whole `case` block is an expression and we can make it the last evaluated one:

```ruby
private

def greeting
  case @language
  when 'spanish'
    'Hola'
  when 'french'
    'Bonjour'
  else
    'Hello'
  end
end
```

Run the tests again and it should pass.

### Source Control

Let's commit what we've done so far:

```bash
git add hello.rb greeter.rb greeter_test.rb
git commit -m 'feat(hello): multilingual hello world'
```

### Olá, Hallo, Ciao, Konnichiwa

As an exercise add greetings for other languages.

Remember the cycle:

- Write a test
- Run the test, see it failing and check the error message
- Write enough code to make the test pass
- Refactor

## Key Concepts

This is probably the fanciest `Hello, World` you have ever written, isn't it?

We learn a bunch of things here.

### Ruby

- how to create a class
- how to create methods (and private methods)
- how to create a constructor method (`initialize`)
- instance variables are prefixed with the `@` symbol
- parentheses in method calls are optional
- the last evaluated expression of a method is always returned
- string interpolation (e.g.: `"Hello, #{name}"`)
- `if-else` and `if-elsif-else` statements
- `case` statements
- how to write tests with Minitest (it's just Ruby)

### Testing

The TDD process and _why_ the steps are important

- _Write a failing test and see it fail_
  - so we know we have written a _relevant_ test for our requirements
  - and see that it produces an _easy to understand description of the failure_
- Writing the smallest amount of code to make it pass
  - so we know we have working software
- _Then_ refactor, backed with the safety of our tests
  - to ensure we have well-crafted code that is easy to work with

We've gone from `greeter.hello` to `greeter.hello("name")` and then to `spanish_hello("name")` in small and easy to understand steps.

Of course this is trivial compared to "real-world" software, but the principles still stand.

TDD is a skill that needs practice to develop, and by breaking problems down into smaller components that you can test, you will have a much easier time writing _and reading_ software.

[^1]: Domain: in software engineering the **domain** represents the target subject of a specific programming project.

[^2]: When we invoke a function (or method), we usually give an input and expect a value to be returned. When the function has any effect other than its primary effect of returning a value, it's said that it has a **side-effect**. Printing a string is considered a side-effect because it interacts with the external environment by sending output to a display or console. This action changes the state of the system's output device, which is outside the function's local scope.

[^3]: **Syntactic sugar** is a syntax within a programming language that is designed to make things easier to read or to express.
