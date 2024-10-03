# Loops, Blocks and Strings

When we want to do something repeatedly, we use loops. There are many ways to create loops in Ruby, in this chapter we're going to see a couple of them.

## Text Banner

We're going to write a method that adds a decoration to a string to make it look like it's in a banner, like this:

```
================
this is a banner
================
```

Starting the `banner` project:

```bash
# define TDD_RUBY_PATH in your shell configuration
cd $TDD_RUBY_PATH
mkdir banner
cd banner
```

Then let's start with the test.

### Write the test first

Here's our first test:

```ruby
require 'minitest/autorun'
require_relative 'banner'

class TestBanner < Minitest::Test
  def test_banner
    expected = "====\ntext\n====\n"
    actual = banner('text')
    assert_equal expected, actual
  end
end
```

Maybe you felt a little uncomfortable with that hard-to-read value assigned to `expected`. Yeah, me too.

I think that mentally counting the amount of `=`s to put above and below the text is very error prone. It can make future maintainer's life worse (keep in mind that the future maintainer can be you). Let's find a better way to write that.

#### Here Documents

A [here document](https://docs.ruby-lang.org/en/master/syntax/literals_rdoc.html#label-Here+Document+Literals) (aka heredoc) is a more comfortable way to read a multilinear block of text. In our case, as we want to check if the borders of the banner have the proper length in a quick glance, we would use a heredoc like this:

```ruby
def test_banner
  expected = <<~BANNER
    ====
    text
    ====
  BANNER
  actual = banner('text')
  assert_equal expected, actual
end
```

Here we're using a "squiggly heredoc" so we can have indented content between the opening identifier (`<<~BANNER`) and the closing one (`BANNER`)

Check the [official documentation](https://docs.ruby-lang.org/en/master/syntax/literals_rdoc.html#label-Here+Document+Literals) if you need more info about heredocs.

### Write the minimal amount of code for the test to run

Running the test:

```
banner_test.rb:2:in `require_relative': cannot load such file -- /path/to/banner (LoadError)
        from banner_test.rb:2:in `<main>'
```

As expected, we got an **error** (not a **failure**).

*Keep the discipline!* You don't need to know anything new to make the test fail properly.

All you need to do right now is write enough code to make the test **fail** with no **errors**.

If you let the error messages drive the development you'll end up with a file named `banner.rb` with this content:

```ruby
def banner(text)
end
```

Run the test and now it should **fail** with no errors.

### Write enough code to make the test pass

In order to create the banner we need:

1. check how many characters there are in the given string
2. put the `=` character the same amount of times as the length of the given string, above and below the string

#### String length

For our first goal, we can check the String class documentation and look if it has a method to give us the length of a string. Turns out that such method is the [String#length](https://docs.ruby-lang.org/en/master/String.html#method-i-length).

Let's experiment in `irb`:

```ruby
# IRB SESSION

> 'foo'.length
#=> 3

> 'meleu'.length
#=> 5

> 'long string with many words...'.length
#=> 30
```

Great, that's exactly what we need!

Moving on...

#### Loops and Blocks

One of the simplest ways to create a loop in Ruby is by using [the Integer#times method](https://docs.ruby-lang.org/en/master/Integer.html#method-i-times). Here's an example:

```ruby
5.times do
  puts "Learn Ruby with TDD"
end
```

Run this code in `irb` and you'll see that it prints `Learn Ruby with TDD` 5 times.

Although that code is easy to read, almost like natural language, the way it's written can be new for us, programmers.

In that code we're using the `#times` method and also a *block*.

Blocks are frequently used in Ruby. It's like a way of bundling up a set of instructions for use elsewhere.

In our code above, the block starts with the keyword `do` and ends with the `end`. The block is being passed to the `#times` method to be executed.

We're going to talk more about blocks, but for now that's enough.

#### Concatenating Strings

The simplistic way to concatenate strings is by using the `+` plus sign, like this:

```ruby
# IRB SESSION

> name = 'meleu'
#=> "meleu"

> 'Hello, ' + name
#=> "Hello, meleu"
```

If we want to append content to a variable we could use the `+=` operator, like this:

```ruby
# IRB SESSION

> msg = 'Hello'
#=> "Hello"

> msg += ', meleu'
#=> "Hello, meleu"

> msg
#=> "Hello, meleu"
```

Although it's possible to append contents to a string variable with the `+=` operator, the most usual Rubyist way to do it is by using the [shovel operator](https://docs.ruby-lang.org/en/master/String.html#method-i-3C-3C): `<<`

```ruby
# IRB SESSION

> msg = 'Hello'
#=> "Hello"

> msg << ', meleu'
#=> "Hello, meleu"

> msg
#=> "Hello, meleu"
```

The final result can look like the same, but there are subtle differences between `+=` and `<<`. The most notable one is that using `<<` has a better performance, specially when used in a loop, which is our case. Let's stick with the `<<`.

> [This StackOverflow Q&A](https://stackoverflow.com/q/4684446) has some good answers about the differences between `+=` and `<<`. Including some code where you can prove the difference in performance.


#### First implementation

Let's recap what we've just learned:

1. Get the string length with `text.length`
2. Repeat instructions in a loop `x` times by
    - bundling up a set of instructions in a block
    - passing the block to `x.times`
3. Append contents to a string variable with `var << 'more content'`

Combining these techniques we can create a text banner function like this:

```ruby
def banner(text)
  # create an empty string
  border = ""

  # get the text length and call the
  # .times method to create a loop
  text.length.times do
    border << '=' # appending '=' to the border
  end

  # interpolating the border before and after the text
  "#{border}\n#{text}\n#{border}\n"
end
```

I've put some comments just to explain what we're doing now. In the refactor phase we're going to clean them up.

Maybe the only thing that require an extra explanation is the `text.length.times` expression. We can use that because `String#length` returns an Integer, and then we can use `Integer#times`, which is what creates the loop

Let's run the test:

```
Run options: --seed 15260

# Running:

.

Finished in 0.000478s, 2092.0506 runs/s, 2092.0506 assertions/s.
1 runs, 1 assertions, 0 failures, 0 errors, 0 skips
```

Great! A successful test triggers our brain to make that decision: refactor or commit?

Let's refactor!

### Refactor

I'd like to use this refactoring session to talk about blocks again...

##### Bracket Blocks

When a block contains just a single instruction, it's usual to use `{` brackets `}` to delimiter the block, like this:

```ruby
5.times { puts "Learn Ruby with TDD" }
```

Run this 👆 in `irb` and confirm.

With this new knowledge we can make our code shorter:

```ruby
def banner(text)
  border = ""
  text.length.times { border << '=' }
  "#{border}\n#{text}\n#{border}\n"
end
```

Run the test and you'll see that it's still working fine.

#### String multiplication

One interesting thing in [String documentation](https://docs.ruby-lang.org/en/master/String.html#method-i-2A) is that we can use `string * integer` to get a new string containing `integer` copies of the original `string`. Like this:

```ruby
"Ho! " * 3
# => "Ho! Ho! Ho! "
```

This is an example of how expressive the Ruby language can be.

With this knowledge we can refactor our banner function so it doesn't even need a loop:

```ruby
def banner(text)
  border = '=' * text.length
  "#{border}\n#{text}\n#{border}\n"
end
```

Run the test to confirm this is working as expected, and then we're done with this feature.

### Source Control

```bash
git add banner_test.rb banner.rb
git commit -m 'feat(banner): put text in a banner'
```

## New requirements

Let's imagine we released our software to the world. This is when the "real work" begins: maintaining our software. We start to see our code being used in ways we didn't anticipate, and bugs start to appear.

A few days after publishing our code a user submitted a bug report saying that it doesn't work with multiline strings.

When we receive a bug report the very first thing to do is to reproduce it. Let's do it in `irb` (be sure to be in the same directory as your code):

```ruby
# IRB SESSION #

> # yeah! we can require code in irb too!
> require_relative 'banner'
#=> true

> text = "this is\na multiline\nstring"
#=> "this is\na multiline\nstring"

> # confirming it's multiline
> puts text
this is
a multiline
string
#=> nil

> puts banner(text)
==========================
this is
a multiline
string
==========================
#=> nil
```

Indeed, that's not what we would expect from the banner code. We didn't envisaged this use case when we were developing. Such situation is one of the most common things in the day-to-day work of a professional developer, we must get used to it.

In fact this situation is so common that more than fifty years ago a computer scientist named [Manny Lehman](<https://en.wikipedia.org/wiki/Manny_Lehman_(computer_scientist)>) already noticed that and said:

> **A system must be continually adapted or it becomes progressively less satisfactory.**

This is the first of the eight [Lehman's laws of software evolution](https://en.wikipedia.org/wiki/Lehman's_laws_of_software_evolution)

Once we confirmed that the bug report is valid. Then let's ~~fix it~~ reproduce it in a test case.

### Write the test first

We expect the borders to be as long as the longest line, then let's write a test for it:

```ruby
def test_banner_with_multiple_lines
  expected = <<~BANNER
    =============
    text
    with multiple
    lines
    =============
  BANNER
  text = "text\nwith multiple\nlines"
  actual = banner(text)
  assert_equal expected, actual
end
```

Run the test to see the error/failure message:

```
  1) Failure:
TestBanner#test_banner_with_multiple_lines [banner_test.rb:25]:
--- expected
+++ actual
@@ -1,6 +1,6 @@
-"=============
+"========================
 text
 with multiple
 lines
-=============
+========================
 "
```

Good thing that the test **failed** with **no errors**, but we still need to sort the failure.

The output shows exactly the difference between the expected and the actual borders, while the text in between remains the same.

Now that we have a good test, we can work on our fix.

### Write enough code to make the test pass

If our border needs to be as long as the longest line in the string, we need a way to check each line length.

#### Iterating over each line of a String

In the String class page we can see [a method called `#each_line`](https://docs.ruby-lang.org/en/master/String.html#method-i-each_line).

When we call `String#each_line` giving a block to it, it creates substrings that are the result of splitting the original string at each occurrence of a new line. The new thing for us here is that `#each_line` needs a place (a variable) to where it puts the created substring. In this case we need to use a block with a parameter.

#### Block Parameters

When the instructions within our block need to reference the value they're currently working with, we specify a block parameter. Let's see an example in `irb`:

```ruby
# IRB SESSION

> "multi\nline\nstring".each_line { |line| p line }
"multi\n"
"line\n"
"string"
#=> "multi\nline\nstring"
```

In each iteration the substring is stored in the `line` variable and we can use it however we want. In the example above we're just inspecting it with [the `Kernel#p` method](https://docs.ruby-lang.org/en/master/Kernel.html#method-i-p).

That looks promising! 🙂


#### Longest line

Let's use what we've learned to check the longest line.

```ruby
def banner(text)
  # assume the max_length is zero
  max_length = 0

  # loop over each line and check if its length > max_length
  text.each_line do |line|
    length = line.length
    max_length = length if length > max_length
  end
  # create the border
  border = '=' * max_length

  "#{border}\n#{text}\n#{border}\n"
end
```

In the loop above we're checking if the current line length is longer than `max_length`. If that's true, then we update the value of `max_length`.

Running the test:

```
  1) Failure:
TestBanner#test_banner_with_multiple_lines [banner_test.rb:25]:
--- expected
+++ actual
@@ -1,6 +1,6 @@
-"=============
+"==============
 text
 with multiple
 lines
-=============
+==============
 "
```

Whoops! Looks like there's an extra `=` character in the border.

The reason for this is that `String#each_line` creates substrings by splitting the original string at the occurrences of a new line, *but it preserves the newline character*.

We could see that in our `irb` session:

```ruby
# IRB SESSION #

> "multi\nline\nstring".each_line { |line| p line }
"multi\n"
"line\n"
"string"
#=> "multi\nline\nstring"
```

So, in order to accurately get the length of a line we must ignore the trailing newline character. Fortunately we ha method for that: [String#chomp](https://docs.ruby-lang.org/en/master/String.html#method-i-chomp).

This time you check by yourself on `irb`. Try it with a string like `"meleu\n"`.

Here's the new version of our code using `#chomp`:

```ruby
def banner(text)
  max_length = 0
  text.each_line do |line|
    # using chomp 👇 here
    length = line.chomp.length
    max_length = length if length > max_length
  end
  border = '=' * max_length

  "#{border}\n#{text}\n#{border}\n"
end
```

Run the tests and they should pass now.

Time for refactoring.

### Refactor

I think our banner code is starting to accumulate too much logic in it. It should be a function that just puts borders above and below a given text.

Unfortunately real world is not that simple. We found a need to handle the case where the input text has multiple lines. Our `banner` is handling this but its complexity increased. This situation reminds another one of the [Lehman's laws](https://en.wikipedia.org/wiki/Lehman's_laws_of_software_evolution):

> **As a system evolves, its complexity increases unless work is done to maintain or reduce it.**

We need to proactively fight against this increasing complexity. In our case here we're going to achieve this by delegating the longest-line-detection logic to a separate function

```ruby
def banner(text)
  border = '=' * max_line_length(text)
  "#{border}\n#{text}\n#{border}\n"
end

def max_line_length(text)
  max = 0
  text.each_line do |line|
    length = line.chomp.length
    max = length if length > max
  end
  max
end
```

Run the tests and they should pass.

For now I think I'm happy with this version. So, let's move on.

### Source Control

```bash
git add banner_test.rb banner.rb
git commit -m 'fix(banner): handle multiline text'
```

## Key Concepts

### Ruby

- Heredocs
- Loops with `Integer#times`
- Blocks
  - with `do`/`end`
  - with `{` brackets `}`
  - with a parameter
- String methods
  - concatenating with `<<` (aka shovel operator)
  - string "multiplication" with `*`
  - `length`
  - `each_line`
  - `chomp`

### TDD

- Reinforced TDD practices (write test first!)
- After receiving a bug report:
  - first reproduce it in a test case
  - then start working on the fix

### Software Engineering

The two first [Lehman's laws of software evolution](https://en.wikipedia.org/wiki/Lehman's_laws_of_software_evolution).

> **Continuing Changee**: A system must be continually adapted or it becomes progressively less satisfactory.
> 
> **Increasing Complexity**: As a system evolves, its complexity increases unless work is done to maintain or reduce it.


