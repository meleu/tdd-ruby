# Arrays and Floats

In this chapter we're going to deal with arrays and float numbers.

## Arrays

Arrays allow you to store multiple elements in a variable in a particular order. Examples:

```ruby
my_array = [1, 4, 2, 3]

# you can mix different types of objects
another_array = [nil, 1, 2.5, 'Fizz', true, 'Buzz']
```

You can access the elements of an array by its index, where the first is `0`, the second is `1`, and so on. Example:

```ruby
# IRB SESSION

> names = ['john', 'mary', 'chris']
#=> ["john", "mary", "chris"]
> names[0]
#=> "john"
> names[1]
#=> "mary"
> names[2]
#=> "chris"
```

## Floats

Floats are essentially numbers with a decimal point, unlike Integers, which are whole numbers.

Although we can think in floats simply as "numbers with a decimal point", they have some idiosyncrasies. Here's a notable example:

```ruby
# IRB SESSION

> 0.1 + 0.2
#=> 0.30000000000000004
```

😳

Do it on your computer and see it by yourself!

Such a weird thing happens because internally, computers use a fixed amount of bits to store these numbers. The binary representation can precisely store powers of two, but many decimal fractions (like `0.1` and `0.2`) result in infinitely repeating binary fractions. Computers must truncate these infinite representations, leading to small rounding errors.

If you're really curious about this topic, this website can be interesting for you: <https://floating-point-gui.de/>.

## Average

To illustrate the usage of arrays and floats, we are going to create a function to calculate the average of the numbers in an array.

For this calculation we need to:

1. sum all the given numbers
2. divide the sum by the amount of numbers

As we have more than one step to solve this problem, we better write down what we need to do. This helps us to stay focused.

> - [ ] calculate the sum
> - [ ] calculate the average

Good. We can now start the project:

```bash
mkdir average
cd average
```

### Step 1: Calculate the sum

Let's focus on calculating the sum. As usual: test first.

#### Write the test first

Before spending mental energy thinking about how to implement the sum, let's focus on how we would like to call a function that, hypothetically, already solves such problem.

Let's call the class as `StatisticsCalculator` and the method would be simply `#sum`. With this in mind, let's write the test in a file called `statistics_calculator_test.rb`:

```ruby
require "minitest/autorun"
require_relative "statistics_calculator"

class TestStatisticsCalculator < Minitest::Test
  def test_sum_numbers_in_array
    numbers = [1, 2, 3]
    expected = 6
    actual = StatisticsCalculator.new.sum(numbers)
    assert_equal expected, actual
  end
end
```

Run the test and check the error.

#### Write the minimal amount of code for the test to run
 
Solve the error messages until the test **fails** without **errors**. By doing this you should now have a file named `statistics_calculator.rb` with contents like this:

```ruby
class StatisticsCalculator
  def sum(numbers)
  end
end
```

Now your test is failing with no errors.

#### Write enough code to make the test pass

When we have an array, it is very common to have to iterate over them. Getting the sum numbers in an array is a good example, as we need to go through each element and add them in an accumulator.

To iterate over each element we use [the `Array#each` method](https://ruby-doc.org/current/Array.html#method-i-each), and to perform actions with the elements we also pass a block with a parameter (which we learned in the previous chapter).

So, to calculate the sum we do this:

```ruby
class StatisticsCalculator
  def sum(numbers)
    accumulator = 0
    numbers.each { |n| accumulator += n }
    accumulator
  end
end
```

Run the test and it should pass. A passing test means that it's now time for refactoring...

#### Refactor

After carefully looking at the list of available methods in the [the Array class documentation](https://ruby-doc.org/current/Array.html) I noticed a method called `#sum`... 🤔

The documentation says:

> **sum(init = 0) → object**<br>
> **sum(init = 0) {|element| ... } → object**
>
> When no block is given, returns the object equivalent to:
>
> ```ruby
> sum = init
> array.each {|element| sum += element }
> sum
> ```
> 
> For example, `[e1, e2, e3].sum` returns `init + e1 + e2 + e3`.

😳 This looks **a lot** like the code we just wrote for our `sum` function!

Let's see if we can replace our code with a simple method call, like this:

```ruby
def sum(numbers)
  numbers.sum
end
```

Run the test and it should pass, which again triggers the decision: should we refactor more?

How could we refactor an oneliner like this?

In this case my refactoring is: **delete the function and the test!**

We must keep in mind that "**code is liability**". Each line of code in the repository has a cost and needs a reason to be kept there.

In this case I see no value in keeping a `sum` function just to be wrapper around the `Array#sum` call. 

The same decision also applies to the tests. The main benefit of keeping our tests is to give us confidence. If I were doubtful about the `Array#sum` behavior, I would keep a test to validate it, but I trust the Ruby language maintainers (otherwise I wouldn't build serious web applications on top of this technology). I also believe they have tests for `Array#sum`. Therefore I'll delegate to them the responsibility to keep it working.

So, the result of our refactoring is an empty class in `statistics_calculator.rb` file and an empty test class in `statistics_calculator_test.rb`, like this:
 
```ruby
# statistics_calculator.rb
class StatisticsCalculator
end
```

```ruby
# statistics_calculator_test.rb
require 'minitest/autorun'
require_relative 'average'

class AverageTest < Minitest::Test
end
```

Our TODO list is now like this:

> - [x] calculate the sum
> - [ ] calculate the average


### Step 2: Calculate the average

Let's do some basic math with a simple example:

Given the numbers 1, 2, 3, and 4; we have

- sum: 1 + 2 + 3 + 4 = 10
- amount of numbers: 4
- **average**: 10 / 4 = **2.5**

This seems to be a good example for our first test.

#### Write the test first

Put this in your `average_test.rb`:

```ruby
class TestStatisticsCalculator < Minitest::Test
  def test_average
    numbers = [1, 2, 3, 4]
    expected = 2.5
    actual = StatisticsCalculator.new.average(numbers)
    assert_equal expected, actual, "Given: #{numbers}"
  end
end
```

Have you noticed that the assertion expression is a bit different? Turns out that `assert_equal` method accepts a third (and optional) argument with a custom message to be shown when the assertion fails.

Sometimes it's useful to print a meaningful message when our assertion fails. It should help us to see what we're doing wrong. In this case we're printing the input numbers.

We're ready to run the tests and check the error messages.

#### Write the minimal amount of code for the test to run

By following the cycle we've been practicing, you should end up with an `statistics_calculator.rb` like this:

```ruby
class StatisticsCalculator
  def average(numbers)
  end
end

```

Now your test should be failing with no errors, and with a clear message like this:

```
  1) Failure:
TestStatisticsCalculator#test_average [statistics_calculator_test.rb:8]:
Given: [1, 2, 3, 4].
Expected: 2.5
  Actual: nil
```

Look that extra line showing the given numbers. That's the result of that third argument we gave to the `assert_equal` in our test. This can be helpful so you can quickly check if your tests and assertions are correct.

Now we can work on the actual implementation.

#### Write enough code to make the test pass

Recapping the steps to get the average:

1. sum all the given numbers
2. divide the sum by the amount of numbers

We saw that we can get the sum of all numbers in an array simply using `Array#sum`.

Have you noticed that sometimes Ruby code reads almost like natural language? With that in mind, try to guess how you can get the amount of elements in an array...

You probably guessed right: `Array#length` or `Array#size`. Both do the same: return the count of elements in the array. (If you're in doubt, launch an `irb` session and perform some tests)

With this knowledge we can try our first implementation:

```ruby
class StatisticsCalculator
  def average(numbers)
    numbers.sum / numbers.length
  end
end
```

Test failure message:

```
Failure:
AverageTest#test_simple_average [average_test.rb:9]:
Given: [1, 2, 3, 4].
Expected: 2.5
  Actual: 2
```

😳 What?! That's not what we expected...

I think we need an `irb` session for some experiments.

```ruby
# IRB SESSION

> array = [1, 2, 3, 4]
#=> [1, 2, 3, 4]

> array.sum
#=> 10

> array.length
#=> 4

> array.sum / array.length
#=> 2

# let's check with actual numbers:
> 10 / 4
#=> 2
```

As you can see, the expression `10 / 4` returns `2`, and not `2.5` as we were expecting.

Now I need to tell you an interesting aspect of Ruby: arithmetic operators are actually methods called on objects.

This concept can be confusing, so let's clarify with an example: `10` is an object, and when we use `10 / 4` we are actually calling the `/` "slash" method on the `10` object.

In other words: `10 / 4` is a syntactic sugar for `10./(4)`.

```ruby
# IRB SESSION

> 10 / 4
#=> 2

> 10./(4)
#=> 2
```

 If `/` "slash" is a method, then we have documentation for it! As we are calling it in an Integer, we should look for [the Integer#/ documentation](https://ruby-doc.org/current/Integer.html#method-i-2F). And there we can see this description:

> **self / numeric → numeric_result**
>
>Performs division; for integer `numeric`, truncates the result to an integer

That's why our average function is not working properly! As the divisor is an integer, the result is truncated to an integer.

To solve this we just need to convert the divisor to a float. Let's confirm:

```ruby
# IRB SESSION

> 10 / 4.to_f
#=> 2.5
```

Nice. Now we can fix our code:

```ruby
class StatisticsCalculator
  def average(numbers)
    numbers.sum / numbers.length.to_f
  end
end
```

Run the test and it should pass.

#### Refactor

To be fair, the current function is pretty OK. No need for refactoring.

### Assertion with floats

There's one more aspect of average calculation and float numbers that warrants extra exploration: how to write a test when the average results in a repeating decimal?

Let's update our TODO list marking what we did and adding a new test:

> - [x] calculate the sum
> - [x] calculate the average
> - [ ] dealing with repeating decimal numbers

#### Adding a new test

Here's an example with a repeating decimal:

The numbers are 2, 3, and 5; then we have

- sum: 2 + 3 + 5 = 10
- amount of numbers: 3
- **average**: 10 / 3 = **3.333...**

If we want to write a test for this case, how many decimal digits should we use? Let's see what happens with just 3 decimals:

```ruby
class TestStatisticsCalculator < Minitest::Test
  # ...
  
  def test_average_with_repeating_decimal
    numbers = [2, 3, 5]
    expected = 3.333
    actual = StatisticsCalculator.new.average(numbers)
    assert_equal expected, actual, "Given: #{numbers}"
  end
end
```

The tests results in a failure like this:

```
  1) Failure:
TestStatisticsCalculator#test_average_with_repeating_decimal [statistics_calculator_test.rb:16]:
Given: [2, 3, 5].
Expected: 3.333
  Actual: 3.3333333333333335
```

As we can see, creating an assertion with Floats is not that straightforward. Fortunately we have an specific assertion to deal with Floats: `assert_in_delta`. And [it's documentation](https://ruby-doc.org/current/gems/minitest/Minitest/Assertions.html#method-i-assert_in_delta) says:

> **assert_in_delta(exp, act, delta = 0.001, msg = nil)**
> 
> For comparing Floats. Fails unless `exp` and `act` are within `delta` of each other.

Explaining in another way, it checks if the expected and the actual values are within `delta` difference of each other.

Let's rewrite our test using this new assertion:

```ruby
class TestStatisticsCalculator < Minitest::Test
  # ...
  
  def test_average_with_repeating_decimal
    numbers = [2, 3, 5]
    expected = 3.333
    actual = StatisticsCalculator.new.average(numbers)
    assert_in_delta expected, actual, 0.001, "Given: #{numbers}"
  end
end
```

If you run the test now it should pass.

This test was added just to learn how to deal with repeating decimals and didn't require any change on our code. Then let's update our TODO list.

> - [x] calculate the sum
> - [x] calculate the average
> - [x] dealing with repeating decimal numbers

If you're doing version control, that's a good time to commit your changes.


## Other useful Array methods

As you may have noticed, Ruby’s standard library comes with a lot of useful solutions (for example, Array#sum).

Other quite handy Array methods provided by default are:

- `#each_with_index` – use this when you want to iterate over each element and need the index.
- `#sort` – returns a new array with the elements of the original array, but sorted.
- `#sort_by` – similar to `#sort`, but allows you to specify the sorting criterion.
- `#min`, `#max` – returns the minimum/maximum valued element in the array.
- `#minmax` – returns a new 2-element array containing the minimum and maximum values.

This is a short list of the Array’s methods. Be sure to spend some time reading [the Array class documentation](https://ruby-doc.org/current/Array.html); it’s a worthwhile investment of your time. 

## Key Concepts

### Ruby

- Array#sum: get the sum the numbers in an array.
- Array#length (aliased as Array#size): get the amount of elements in an array.
- The default Ruby library has many other useful methods for handling Arrays.
- Division of integers always result in a truncated integer.
  - Change the divisor to a float to get the result in a float.
- Floating point numbers are not always accurate.

### Testing

- Creating a list of tests we need to write is useful to keep our focus.
- `assert_equal` accepts a third parameter where we can enrich the failure message.
- `assert_in_delta` is a better assertion for comparing floats.

By the way, spending sometime skimming [the Minitest assertions documentation](https://ruby-doc.org/current/gems/minitest/Minitest/Assertions.html) is recommended.
