# Objects, Methods and Integers

Ruby is an Object-Oriented Programming (OOP) language. In Ruby (almost) everything we interact with is an object. That includes basic data types like numbers, strings and even `nil`. Every value in Ruby has an underlying object representation and can be manipulated with methods.

Ruby is also known to be "weakly typed", which means that type checking is not strictly enforced. This feature allows variables to change types dynamically at runtime. Example:

```ruby
x = 10      # x is an Integer
x = "hello" # x is now a String
```

The object-oriented approach combined with the dynamic type system make Ruby a powerful and flexible language. In this chapter we're going to use these features to code a decimal to binary converter.

## Decimal to Binary Converter

The numeral system humans are used to use is the decimal system. It has this name because it uses ten different digits to represent the numbers: `0`, `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8`, `9`.

The [binary numeral system](https://simple.wikipedia.org/wiki/Binary_number) is a way to write numbers using only two digits: `0` and `1`. As it only needs two digits, we can say it's a _base two_ number system.

For computers the binary system is extremely efficient because they need to store information in only two simple different states: "on" or "off" (`1` or `0`). Sets of binary numbers can be used to represent any information, such as text, audio, or video.

For the code we're going to work on this chapter I'm assuming you at least know what a binary number is and that it's a _base two_ number system.

You don't need to know the math needed to convert a decimal number to binary notation (Ruby has convenient ways to do it). But I'm assuming you know that the binary `1001` is not one thousand and one (you don't even need to know that it's nine).

If you need more information on this topic, [this Wikipedia page](https://simple.wikipedia.org/wiki/Binary_number) can be a good start.

The very first thing is to create a directory for us to work:

```bash
# remember to define TDD_RUBY_PATH in your shell configuration
cd $TDD_RUBY_PATH
mkdir dec2bin
cd dec2bin
```

Now let's start our _Decimal to Binary Converter™_ project following the TDD cycle:

- Write a test
- Run the test, see it fails and check the error message
- Write enough code to make the test pass
- Refactor

### Write the test first

We still have no idea about how to implement this converter, then *how can we write a test for a code that doesn't even exist?!* That's strong and valid question. The answer is: **write the test using the best interface you can think of to perform the operation**.

Keeping this in mind, I list here my ideas for a great interface to a function able to convert a decimal number to its binary representation:

- a function named `dec2bin`
- it accepts an integer number as the only argument
- it returns the binary representation of the given number

Something like this:

```ruby
binary = dec2bin(123)
```

Yeah, that looks good. Let's go ahead and create a test for that (inexistent) function!

We will need to, at least, know what would be a successful conversion. For this I use the [Wikipedia page about binary numbers](https://simple.wikipedia.org/wiki/Binary_number), where we can see a table like this:

| decimal | binary |
| :-----: | -----: |
|    0    |    `0` |
|    1    |    `1` |
|    2    |   `10` |
|    3    |   `11` |
|    4    |  `100` |
|    5    |  `101` |
|    6    |  `110` |
|    7    |  `111` |
|    8    | `1000` |

Let's use 8 to write our first test.

Create a file named `dec2bin_test.rb`:

```ruby
require "minitest/autorun"
require_relative "dec2bin"

class TestDec2Bin < Minitest::Test
  def test_convert_eight
    actual = dec2bin(8)
    expected = "1000"
    assert_equal expected, actual
  end
end
```

Run this test and check the error message.

### Error vs. Failure

In the very first run of our test we see an error:

```
$ ruby dec2bin_test.rb

dec2bin_test.rb:2:in `require_relative': cannot load such file -- /path/to/dec2bin (LoadError)
        from dec2bin_test.rb:2:in `<main>'
```

It's important to understand the subtle difference between an **error** and a **failure**.

An **error** means that there's something broken in our code, and it prevents our tests from running. A **failure** means that our test ran, but it failed assert the defined expectations.

When we face an error, we should check the message and write the minimal amount of code for the test to run.

### Write the minimal amount of code to for the test to run

By doing this we're letting the tests guide our development. That's a core concept of Test-Driven Development.

Back to the error message:

```
$ ruby dec2bin_test.rb

dec2bin_test.rb:2:in `require_relative': cannot load such file -- /path/to/dec2bin (LoadError)
        from dec2bin_test.rb:2:in `<main>'
```

We're requiring a file that doesn't exist. Then let's create the file, run the test again and see the next error:

```
$ # creating the file
$ touch dec2bin.rb

$ # running the test
$ ruby dec2bin_test.rb

# Running:

E

Finished in 0.000271s, 3690.0373 runs/s, 0.0000 assertions/s.

  1) Error:
TestDec2Bin#test_convert_eight:
NoMethodError: undefined method `dec2bin' for #<TestDec2Bin:0x00000001348f40f0>
    dec2bin_test.rb:7:in `test_convert_eight'

1 runs, 0 assertions, 0 failures, 1 errors, 0 skips
```

Now the error message says `NoMethodError: undefined method 'dec2bin' ...`.

Let's create that method in our `dec2bin.rb`:

```ruby
def dec2bin
end
```

Run the test, check the message:

```
1) Error:
TestDec2Bin#test_convert_eight:
ArgumentError: wrong number of arguments (given 1, expected 0)
    /path/to/dec2bin.rb:1:in `dec2bin'
    dec2bin_test.rb:7:in `test_convert_eight'
```

Let's fix the `wrong number of arguments` in our `dec2bin.rb`:

```ruby
def dec2bin(number)
end
```

Run the test, check the message:

```
  1) Failure:
TestDec2Bin#test_convert_eight [dec2bin_test.rb:8]:
Expected: "1000"
  Actual: nil
```

Now our test is finally running with no errors! It's now failing, but at least it has no errors. We're almost there!

You may be thinking that you're wasting your time in this tedious loop of running the test, checking the error message and writing the minimal amount of code to fix the error message. I have two points about this practice:

- It is a nice way to prevent over-engineering - your tests are the requirements in form of code, and your software just needs to meet such requirements.
- You'll soon find ways to automatically run tests right after saving your file.

Even if my arguments are not convincing you, please stick with this practice while we're here.

### Write enough code to make the test pass

The failure message says that the expected result is `"1000"` but it received `nil`. So, let's fix this like a pedantic programmer and "write the minimal amount of code to make the test pass":

```ruby
def dec2bin(number)
  "1000"
end
```

Ah hah! Foiled again! TDD is a sham, right?

Maybe we should add another test to `dec2bin_test.rb`:

```ruby
def test_convert_two
  actual = dec2bin(2)
  expected = "10"
  assert_equal expected, actual
end
```

Running the tests:

```
# Running:

F.

Finished in 0.000265s, 7547.1687 runs/s, 7547.1687 assertions/s.

  1) Failure:
TestDec2Bin#test_convert_two [dec2bin_test.rb:14]:
Expected: "10"
  Actual: "1000"

2 runs, 2 assertions, 1 failures, 0 errors, 0 skips
```

If our pedantic instincts evolve to the point where we want to be a prick, we could add an if in our code just to answer with `"10"` when the argument is `2`. But that feels like [a game of cat and mouse](https://en.m.wikipedia.org/wiki/Cat_and_mouse).

Let's stop here and start to work on the code that will actually convert a decimal to its binary representation.

## How to convert to binary?

### "Everything" is an Object

In the beginning of this chapter I said: **every value in Ruby has an underlying object representation and can be manipulated with methods**.

That includes the integer numbers. They are objects and we can interact with them using their methods.

Another fact about Ruby objects is that all of them have a string representation that can be obtained by the `#to_s` method (`to_s` stands for "to string").

As our goal is to convert a decimal to its binary representation, and this representation is written in a string, maybe we can get some help from `Integer#to_s`. Let's take a look at its documentation...

### The Integer#to_s method

As we want to work on Integers, we need to check the documentation about the Integer class: <https://ruby-doc.org/current/Integer.html>

In that page we can see a pretty decent amount of information about Integers, including what they can do (in other words, which methods they have).

We don't need to read all that page, but use it as a reference when needed. As we are suspecting the `Integer#to_s` can help us, let's take a look at [its documentation](https://ruby-doc.org/current/Integer.html#method-i-to_s) (below I bring only the part related to our problem):

> **to_s(base = 10) → string**
>
> Returns a string containing the place-value representation of `self` in radix `base` (in 2..36).
>
> ```ruby
> 12345.to_s     # => "12345"
> 12345.to_s(2)  # => "11000000111001"
> ```

Hey! That looks promising! The method accepts an argument that acts as the base for the string representation we want to get from the integer. As the binary system uses base two, let's check if it can be used in our converter.

Before opening our code editor and writing our implementation, let's play a bit in the Interactive Ruby Shell (`irb`).

We want to check if `Integer#to_s` is able to give us a binary representation of an integer. So, let's try it with `8.to_s(2)`:

```ruby
# IRB SESSION

> 8.to_s(2)
#=> "1000"
```

Yeah! That seems to be exactly what we want! Let's try different values:

```ruby
# IRB SESSION
> 7.to_s(2)
#=> "111"

> 2.to_s(2)
#=> "10"

> 0.to_s(2)
#=> "0"

> 15.to_s(2)
#=> "1111"
```

Alright! I'm convinced! Let's use this method in our converter.

### First implementation

Now that we know `Integer#to_s` can solve our problem, let's use it in our code:

```ruby
def dec2bin(number)
  number.to_s(2)
end
```

Running the tests:

```
# Running:

..

Finished in 0.000261s, 7662.8350 runs/s, 7662.8350 assertions/s.

2 runs, 2 assertions, 0 failures, 0 errors, 0 skips
```

Great! All tests passing means that it's time to refactor.

### Refactor

There's no much room for refactoring in a single line function. However, the refactoring phase is not just about the tidying up the production code. The tests also deserve to be tidy.

Currently our test class looks like this:

```ruby
class TestDec2Bin < Minitest::Test
  def test_convert_eight
    actual = dec2bin(8)
    expected = "1000"
    assert_equal expected, actual
  end

  def test_convert_two
    actual = dec2bin(2)
    expected = "10"
    assert_equal expected, actual
  end
end
```

So far I've been writing tests assigning values to `expected` and `actual` variables and then passing them to `assert_equal`. This makes the code more explicit and intention revealing. However, for simple cases like the ones we have here, a more realistic approach would be to inline the expected value and the function call like this:

```ruby
class TestDec2Bin < Minitest::Test
  def test_convert_eight
    assert_equal "1000", dec2bin(8)
  end

  def test_convert_two
    assert_equal "10", dec2bin(2)
  end
end
```

Run the tests and they should pass. Therefore, it's time for another round of refactoring.

One important aspect of tests to keep in mind is: we should have one test per behavior. If we look carefully, both tests we currently have are testing the same behavior, a simple case of converting an integer to its binary notation. So, I think both tests should be merged into one (and the test be renamed accordingly):

```ruby
class TestDec2Bin < Minitest::Test
  def test_dec2bin
    assert_equal "1000", dec2bin(8)
    assert_equal "10", dec2bin(2)
  end
end
```

Run the test and it should pass.

Now we can ask ourselves if it makes sense to keep two assertions of the same behavior. Well, the second assertion did its job when we were exploring the behavior and it was useful. Now I think we don't need that anymore, so let's keep only one assertion.

```ruby
class TestDec2Bin < Minitest::Test
  def test_dec2bin
    assert_equal "1000", dec2bin(8)
  end
end
```

Run the test and it should pass. And we're done with this refactoring phase.

Now let's use our current code in an application.

### Source Control

Now it's a good time to commit what we have:

```bash
git add dec2bin_test.rb dec2bin.rb
git commit -m 'feat(dec2bin): print numbers in binary notation'
```

## d2b CLI

Now that we have working software, backed by tests, we should be safe to use it in a "real" application.

Let's write an extremely simple application that reads a number from user's input and prints the binary representation of the number.

Create a file named exactly like this: `d2b`. Note that there's no `.rb` extension in the file.

Here are the contents to be put in the `d2b` file (explanation comes right away):

```ruby
#!/usr/bin/env ruby

require_relative "dec2bin"

print "integer: "
my_number = gets

binary = dec2bin(my_number)
puts "binary: #{binary}"
```

In the very first line we're putting a [shebang](<https://en.wikipedia.org/wiki/Shebang_(Unix)>) to tell our OS which interpreter we wan to use to execute the commands in this file, in our case we're telling the OS to use the `ruby` executable found in the user's `PATH` (it's not necessary to know all the details here, but if you're curious [this article](https://dev.to/meleu/what-the-shebang-really-does-and-why-it-s-so-important-in-your-shell-scripts-2755) can help).

The `print` method is just like `puts`, but it doesn't add a trailing newline. This is useful to keep the cursor right in front of the `integer:` string.

The `gets` method is used to get user's input. It returns the data submitted by the user, and we store it in the `my_number` variable.

The rest of the code should be familiar to you and easy to understand.

In order to run this program, we need to give the executable permission to the file.

```
chmod a+x d2b
```

Now we're ready to run it:

```
$ ./d2b
integer:
```

Nice. It's waiting for our input. Let's give it a number.

```
$ ./d2b
integer: 7
/path/to/dec2bin.rb:2:in `to_s': wrong number of arguments (given 1, expected 0) (ArgumentError)
        from /path/to/dec2bin.rb:2:in `dec2bin'
        from ./d2b:8:in `<main>'
```

😳 How could this happen? We used TDD to code our function and it passed the tests!

That's time to tell you a truth about Test-Driven Development: TDD is not a way to _assure_ your code does not have bugs.

TDD is a way to facilitate and guide development, giving you short feedback loops (as you don't need to test your software manually) and lead your implementation to a better design.

Although TDD can reduce _a lot_ the appearance of bugs, _making sure_ your code doesn't have bugs is not something TDD can promise.

### Debugging with `irb`

After this kinda frustrating news, let's try to understand what's wrong on our code. Check the main part of the error message:

```
/path/to/dec2bin.rb:2:in `to_s': wrong number of arguments (given 1, expected 0) (ArgumentError)
```

The message says that the error happened in `dec2bin.rb:2`, which means in the 2nd line of the file.

```ruby
def dec2bin(number)
  number.to_s(2) # 👈 ERROR HAPPENED HERE
end
```

The message also says that we passed a wrong number of arguments to the `to_s` method. But in the documentation we saw that `Integer#to_s` accepts an argument. 🤔 Uhm... Is that `number` really an `Integer`?

In order to check that we're going to turn again to one of our best friends: `irb`.

Ruby provides a way to open an `irb` session from anywhere in your program using `binding.irb`. This is helpful for debugging and is exactly what we need now.

Add `binding.irb` right before the buggy line. Your `dec2bin.rb` should look like this:

```ruby
def dec2bin(number)
  binding.irb
  number.to_s(2)
end
```

Now let's repeat the steps where we faced the error:

```
$ ./d2b
integer: 7
From: /path/to/dec2bin.rb @ line 2 :

    1: def dec2bin(number)
 => 2:   binding.irb
    3:   number.to_s(2)
    4: end

irb(main):001:0>
```

Now we're on the `irb` prompt, right before the point where the crash happened. How cool is that?! 🙂

Once we're on the `irb` prompt we can run any Ruby code. An interesting way to inspect what's in a variable is by using [the `#p` method](https://ruby-doc.org/current/Kernel.html#method-i-p). Let's use it to check what exactly is in the `number` variable:

```ruby
# IRB SESSION

> p number
"7\n"
#=> "7\n"
```

👀 That's a String composed of a character `7` followed by a newline. That means that our `dec2bin` function was called with a String as an argument!

Let's check our `d2b` again, adding some notes:

```ruby
#!/usr/bin/env ruby

require_relative "dec2bin"

print "integer: "
my_number = gets            # 👈 my_number IS ASSIGNED HERE

binary = dec2bin(my_number) # 👈 dec2bin IS CALLED HERE
puts "binary: #{binary}"
```

We're assigning a value to `my_number` with `gets`, which returns the user's input _as a String_. When we pass this string to `#dec2bin` it calls `String#to_s` instead of `Integer#to_s`. And [String#to_s documentation](https://ruby-doc.org/current/Integer.html#method-i-to_s) doesn't accept an argument. That's why our program is crashing!

This is an example of how Ruby's dynamism is a double-edged sword. It can be powerful and allow rapid development, but also requires extra attention. In this case the lack of type checking allowed us to pass an unexpected data type that crashed our application.

Now, before working in a solution for this bug, we'll apply another valuable testing practice: **when you find a bug, replicate it in a test case _before fixing it_.**

**NOTE**: once we found the bug, we can now remove the `binding.irb` line from our `dec2bin.rb` code.

### Replicate bugs in tests

Let's write a test giving the problematic String to the `dec2bin` function:

```ruby
def test_convert_number_in_string
  input = "7\n"
  assert_equal "111", dec2bin(input)
end
```

Run the test and see if the crash was really replicated:

```
  1) Error:
TestDec2Bin#test_convert_number_in_string:
ArgumentError: wrong number of arguments (given 1, expected 0)
    /path/to/dec2bin.rb:2:in `to_s'
    /path/to/dec2bin.rb:2:in `dec2bin'
    dec2bin_test.rb:19:in `test_convert_number_in_string'
```

Nice! Now we can start working on a solution and quickly check if we're on the right path.

### Fixing the bug

Fortunately we can easily solve this issue by converting the string to an Integer using the `String#to_i` method ([documentation](https://ruby-doc.org/current/String.html#method-i-to_i)). It's also fortunate that this method is also available for Integers ([documentation](https://ruby-doc.org/current/Integer.html#method-i-to_i)).

This is an example of how the Ruby's dynamism can promote rapid development. If we were coding with a strongly typed language, we would need to code different functions to allow receiving different data types. With Ruby we can code only one function and work on ways to handle the dynamic typing. As you can notice, everything is a trade-off (and if you're reading until here, you probably like Ruby's dynamism).

In order to fix the bug we just need to chain `to_i` and `to_s`:

```ruby
def dec2bin(number)
  number.to_i.to_s(2)
end
```

Run the tests and they should be passing now.

Run the CLI again and it should work without crashing.

## Source Control

As our repo already has the hello-world code from the previous chapter, let's specify the scope of the current changes in the commit message.

```bash
git add dec2bin_test.rb dec2bin.rb d2b.rb
git commit -m 'feat(dec2bin): use #to_i before #to_s(2) & add a CLI'
```

## Key Concepts

Let's recap what we learned in this chapter.

### Ruby

- OOP: everything in Ruby is an object
- Dynamic typing: variables can change types at runtime
- String representation: all objects have a `to_s` method.
- [Ruby documentation](https://ruby-doc.org/) is an essential resource of information.
- `irb`: quickly experiment Ruby code
- `binding.irb` is a useful debugging technique
- `p`: inspect the contents of a variable
- `gets`: read user's input
- method chaining: calling multiple methods in sequence (e.g.: `number.to_i.to_s(2)`)

### Testing

- Test-first approach: write the test before implementation code.
- Test Error vs. Test Failure
- Minimal implementation: write just enough code to make the tests pass (without being pedantic, please).
- Test code also needs refactoring to stay tidy.
- TDD guides the development, but does not assure our software is free of bugs.
- Replicating bugs in tests: add test cases for discovered issues before fixing them.
