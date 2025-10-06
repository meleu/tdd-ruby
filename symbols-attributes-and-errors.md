# Symbols, Attributes and Errors

I mentioned that OOP is a way of grouping data and behavior under the same concept, but most of the projects we worked until now were mostly about behavior.

With the exception of our `Greeter`, that has a `@language` attribute, the other classes we created (NumberConverter, StringDecorator and StatisticsCalculator) were all about behavior, with no "internal state" data to be managed.

In this chapter we're going to use an attribute to manage the object's state and expose some methods to let users change the state in a way we can control.

Let's pretend you want to found your own fintech startup? So let's start our ~~primitive~~ amazing banking system by creating a `Wallet` class.

Note: for simplicity sake our wallet will not deal with the concept of "currency". The amount of "money" is going to be represented simply by Integers.

Our project starts with:

```bash
mkdir wallet
cd wallet
```

## The Test List

Creating a `Wallet` is not that trivial. Before writing any test we need to think in some requirements. And we better write down what we have in mind to avoid being overwhelmed with a lot of ideas consuming our mental energy.

Here's what I have in mind right now

> - [ ] a wallet starts with a zero balance
> - [ ] a deposit in a wallet immediately reflects in its balance
> - [ ] a withdraw from a wallet immediately reflects in its balance
> - [ ] a withdraw with an amount greater than the current balance must result in an error

Maybe we can think in more requirements later, but with this short list of requirements we can start something. I feel much better having this list written down and not occupying my brain. Now I can focus on writing tests and implementing code to pass the tests

## A Wallet Starts With a Zero Balance

### Write the test first

From that list, let's pick "a wallet starts with zero balance" and write a test for it:

```ruby
require "minitest/autorun"
require_relative "wallet"

class WalletTest < Minitest::Test
  def test_wallet_starts_with_zero_balance
    wallet = Wallet.new
    assert_equal 0, wallet.balance
  end
end
```

Run the test and check the error.

### Write the minimal amount of code for the test to run

You know the drill... After running the code and solving all errors until the test **fails** without **errors**, you should have something like this:

```ruby
class Wallet
  def balance
  end
end
```

And a failure like this:

```
  1) Failure:
WalletTest#test_wallet_starts_with_zero_balance [code/wallet/wallet_test.rb:7]:
Expected: 0
  Actual: nil
```

### Write enough code to make the test pass

Let's just "fake it 'till you make it":

```ruby
class Wallet
  def balance
    0
  end
end
```

### Refactor

We know that a balance is an attribute of a wallet, then I think we need an instance variable to store the current amount of money in the wallet.

The current requirement we're working on says that a wallet starts with a zero balance, then let's create a constructor method initializing an instance variable with zero. The code now looks like this:

```ruby
class Wallet
  def initialize
    @balance = 0
  end

  def balance
    0
  end
end
```

Now we can replace that `0` in `#balance` with the instance variable:

```ruby
  def balance
    @balance
  end
```

The tests must still pass.

We're going to refactor even more, but first let's get acquainted with some other Ruby concepts: symbols and attribute accessors.

### Symbols

In Ruby we have a, let's say, "primitive type" called **symbol**. You can consider a symbol as an immutable and unique string. It is a lightweight, efficient way to represent and compare identifiers because each symbol is unique and reusable throughout the program.

We create a symbol simply by using the syntax `:symbol_name`. Ruby then checks if a symbol with the same name already exists in its internal symbol table. If it does, it reuses the existing symbol reference. Otherwise it creates a new one right away, ensuring memory efficiency. This makes symbols particularly suitable for identifiers where you need a consistently comparable and reusable entity without the overhead and mutability of strings.

 Symbols are useful for many things, and in our use case here we're going to use a simple `:balance` to identify an attribute accessor.

### Attribute Accessors

In our last refactoring we created a remarkably trivial method:

```ruby
  def balance
    @balance
  end
```

This is a method that exists for the sole purpose of reading the value of an attribute. When we have such situation, we can use a Ruby's syntactic-sugar known as "attribute accessors". In this case, as we want to *read*, the accessor is `attr_reader`.

So, our `#balance` method can be entirely replaced with this line:

```ruby
attr_reader :balance
```

This single line is enough to express that "the class has an instance variable called `@balance` and you get it's value by calling the `#balance` method."

It's usual to put this line as one of the first ones in the class's definition. That's so the future programmer reading the code can be aware of the class's attributes right away. Our `Wallet` class now looks like this:

```ruby
class Wallet
  attr_reader :balance

  def initialize
    @balance = 0
  end
end
```

And our test is still passing!

> NOTE:
>
> The usual attribute accessors are:
>
> - `attr_reader`: the one we saw above
> - `attr_writer`: creates an instance variable with the given identifier and allows you to assign values to it
> - `attr_accessor`: equivalent to using both reader and writer.

That's it! Let's move to the next item in our list...

## Deposit Method

> - [x] a wallet starts with zero balance
> - [ ] **a deposit in a wallet immediately reflects in its balance**
> - [ ] a withdraw from a wallet immediately reflects in its balance
> - [ ] a withdraw with an amount greater than the current balance must result in an error

### Write the test first

```ruby
class WalletTest < Minitest::Test
  # ...
  
  def test_deposit_increases_balance
    wallet = Wallet.new
    wallet.deposit(10)
    assert_equal 10, wallet.balance
  end
end
```

By running the test we'll have this error:

```
NoMethodError: undefined method 'deposit' for an instance of Wallet
```

### Write the minimal amount of code for the test to run

```ruby
class Wallet
  attr_reader :balance

  def initialize
    @balance = 0
  end

  # added this method
  def deposit(amount)
  end
end
```

Now the test **fails** with:

```
Expected: 10
  Actual: 0
```

### Write enough code to make the test pass

When we make a deposit in a wallet, we want the balance to increase. Let's solve this by adding the given amount to the wallet's `@balance`:

```ruby
class Wallet
  # ...
  
  def deposit(amount)
    @balance = @balance + amount
  end
end
```

Run the tests and they should pass.

### Refactor

In Ruby there's a shorthand for a situation like this:

```ruby
@balance = @balance + amount
```

It can be rewritten like this:

```ruby
@balance += amount
```

Therefore, our class now looks like this:

```ruby
class Wallet
  attr_reader :balance

  def initialize
    @balance = 0
  end

  def deposit(amount)
    @balance += amount
  end
end
```

If the tests are still passing, let's move on to the next item from our list.

## Withdraw Method

> - [x] a wallet starts with zero balance
> - [x] a deposit in a wallet immediately reflects in its balance
> - [ ] **a withdraw from a wallet immediately reflects in its balance**
> - [ ] a withdraw with an amount greater than the current balance must result in an error

### Write the test first

```ruby
class WalletTest < Minitest::Test
  # ...
  
  def test_withdraw_decreases_balance
    wallet = Wallet.new
    wallet.deposit(100)
    wallet.withdraw(20)
    assert_equal 80, wallet.balance
  end
end
```

This is the error:

```
NoMethodError: undefined method 'withdraw' for an instance of Wallet
```

### Write the minimal amount of code for the test to run

```ruby
class Wallet
  # ...
  def withdraw(amount)
  end
end
```

The **failure** is:

```
  1) Failure:
WalletTest#test_withdraw_decreases_balance [wallet_test.rb:20]:
Expected: 80
  Actual: 100
```

Which means that `#deposit` worked as expected but `#withdraw` did not.

### Write enough code to make the test pass

That shorthand notation we used for addition, `+=`, has an equivalent for subtraction: `-=`. Then let's use it:

```ruby
class Wallet
  # ...
  
  def withdraw(amount)
    @balance -= amount
  end
end
```

The test should pass.

### Refactor

The code is pretty neat:

```ruby
class Wallet
  attr_reader :balance

  def initialize
    @balance = 0
  end

  def deposit(amount)
    @balance += amount
  end

  def withdraw(amount)
    @balance -= amount
  end
end
```

No need for a refactoring this time.

## Ruby Errors and Exceptions

If the wallet has not enough funds, the withdraw should result in an error, right? But before addressing such requirement, let's quickly understand how errors work in Ruby.

The way to handle errors in Ruby is by raising an exception. An Exception is a special kind of object that can be raised to stop the normal execution of a program. As mentioned on the [Exception class documentation](https://ruby-doc.org/3.4.1/Exception.html):

> Class `Exception` and its subclasses are used to indicate that an error or other problem has occurred, and may need to be handled.

The programmer using a code that can raise an exception should either deal with the problem that raised it or let Ruby exit the program completely.

In the documentation we also have the [built-in exception class hierarchy](https://ruby-doc.org/3.4.1/Exception.html#class-Exception-label-Built-In+Exception+Class+Hierarchy), where we can see many errors that we're already dealing with since the "Hello, World" chapter. Namely:

- `NameError`, when we execute the first test of a chapter and the class is not yet created.
- `NoMethodError`, when a method was not yet created.
- `ArgumentError`, when the number of arguments we pass to a method is not what it's expecting to receive.

Those errors are all descendants of the Exception class.

Let's start an `irb` session and raise some errors on purpose:

```ruby
# IRB SESSION

# reference to an undefined indentifier
##############################################
> my_var = this_is_an_unknown_identifier
# (irb):1:in '<main>': undefined local variable or method 'this_is_an_unknown_identifier' for main (NameError)
#         from <internal:kernel>:168:in 'Kernel#loop'
#         from ...

# creating a dummy object and calling a method
##############################################
> my_var = Object.new
# => #<Object:0x00007f2f27cf0ac8>
> my_var.this_is_an_unknown_method
# (irb):3:in '<main>': undefined method 'this_is_an_unknown_method' for #<Object:0x00007f2f27cf0ac8> (NoMethodError)
#         from <internal:kernel>:168:in 'Kernel#loop'
#         from ...


# creating a dummy method that does nothing
##############################################
> def my_method; end
# => :my_method
> my_method("one", "two")
# (irb):5:in 'my_method': wrong number of arguments (given 2, expected 0) (ArgumentError)
#         from (irb):6:in '<main>'
#         from <internal:kernel>:168:in 'Kernel#loop'
#         from ...
```

As you can see, code with perfectly valid syntax can raise errors when we make mistakes. This is how Ruby warns us that we're doing wrong things in our code.

There are situations where we make mistakes that are perfectly valid for Ruby, but not OK for the problem domain we're working with. In such cases we need to explicitly raise errors in our own code.

For example, for Ruby, when we subtract a number from a smaller one, it simply results in a negative value. In our domain, the banking system, we don't want to allow that. When we try to withdraw more funds than what's available in a wallet, it must result in an error.

The simplest way to explicitly raise an error is just using `raise` followed by a string with a description. Example:

```ruby
# IRB SESSION

> raise "Not enough funds"
# (irb):1:in '<main>': Not enough funds (RuntimeError)
#         from <internal:kernel>:168:in 'Kernel#loop'
#         from ...
```

As you can see, the raised error is quite generic: `RuntimeError`. We can also raise a more specific error by giving to `raise` the name of the error class.

In our case, we want to raise an error when the *argument* given to `#withdraw` is greater than the balance. Maybe we can use `ArgumentError`... Let's see how to raise it:

```ruby
# IRB SESSION

> raise ArgumentError, "Not enough funds"
# (irb):1:in '<main>': Not enough funds (ArgumentError)
#         from <internal:kernel>:168:in 'Kernel#loop'
#         from ...
```

Although `ArgumentError` is more specific than `RuntimeError`, it's still generic. It's the same error raised if we call `#withdraw` with 3 arguments, for example. We can (and should) be even more specific here. Let's write this as a requirement in our list and deal with it later.

> - [ ] withdraw when there's no funds must raise a specific error

For now I want to just make sure an error is raised when there's no funds.

## Raise Error When There's Not Enough Balance

Our requirements list now looks like this:

> - [x] a wallet starts with zero balance
> - [x] a deposit in a wallet immediately reflects in its balance
> - [x] a withdraw from a wallet immediately reflects in its balance
> - [ ] **a withdraw with an amount greater than the current balance must result in an error**
> - [ ] withdraw when there's no funds must raise a specific error

### Write the test first

The syntax to write an assertion that certain code raises an error is a bit different from what we've been using. Check it out:

```ruby
class WalletTest < Minitest::Test
  # ...
  
  def test_withdraw_more_than_balance_raises_error
    wallet = Wallet.new
    # the code expected to raise an error must be
    # inside the block given to 'assert_raises'
    assert_raises(ArgumentError) { wallet.withdraw(10) }
  end
end
```

Run the tests and you'll see this **failure**:

```
  1) Failure:
WalletTest#test_withdraw_more_than_balance_raises_error [code/wallet/wallet_test.rb:25]:
ArgumentError expected but nothing was raised.
```

### Write enough code to make the test pass

Well, checking if the given amount is greater than the balance is pretty straight forward, we just need an `if` and then raise an error when the condition is true:

```ruby
class Wallet
  # ...
  
  def withdraw(amount)
    if amount > @balance
      raise "Not enough funds"
    end

    @balance -= amount
  end
end
```

Run the tests:

```
  1) Failure:
WalletTest#test_withdraw_more_than_balance_raises_error [code/wallet/wallet_test.rb:25]:
[ArgumentError] exception expected, not
Class: <RuntimeError>
Message: <"Not enough funds">
---Backtrace---
/.../code/wallet/wallet.rb:14:in 'Wallet#withdraw'
code/wallet/wallet_test.rb:25:in 'block in WalletTest#test_withdraw_more_than_balance_raises_error'
---------------
```

Whoops! I forgot to specify the specific error to `raise`. Then it raised the super generic `RuntimeError`, which is not expected by our test. Let's fix that:

```ruby
def withdraw(amount)
  if amount > @balance
    raise ArgumentError, "Not enough funds"
  end

  @balance -= amount
end
```

And now all tests should be passing!

### Refactor

The first refactoring I want to do is to use another Ruby syntactic-sugar. When we have an if block with only one instruction, we can write the expression in an almost-plain-English line, like this:

```ruby
raise ArgumentError, "Not enough funds" if amount > @balance
```

Then, the withdraw method becomes this:

```ruby
def withdraw(amount)
  raise ArgumentError, "Not enough funds" if amount > @balance

  @balance -= amount
end
```

Run the tests and they should pass.

## Creating a Custom Error

As we mentioned, although the `ArgumentError` is more specific than `RuntimeError`, it's still generic. An evidence for this is that it requires a "Not enough funds" description to make it clear which kind of problem actually happened.

Let's solve this by creating a custom error class with a self-documenting name, like `NotEnoughFundsError`.

> - [x] a wallet starts with zero balance
> - [x] a deposit in a wallet immediately reflects in its balance
> - [x] a withdraw from a wallet immediately reflects in its balance
> - [x] a withdraw with an amount greater than the current balance must result in an error
> - [ ] **withdraw when there's no funds must raise a specific error**

### Write the test first

We still don't know how to create a custom error. Regardless, let's go ahead and **change** our test to assert that the desired error is raised:

```ruby
class WalletTest < Minitest::Test
  # ...

  # here we're changing the existing test, now
  # passing 'NotEnoughFundsError' in the assertion
  def test_withdraw_more_than_balance_raises_error
    wallet = Wallet.new
    assert_raises(NotEnoughFundsError) { wallet.withdraw(10) }
  end
end
```

Running the tests:

```
  1) Error:
WalletTest#test_withdraw_more_than_balance_raises_error:
NameError: uninitialized constant WalletTest::NotEnoughFundsError
    code/wallet/wallet_test.rb:25:in 'WalletTest#test_withdraw_more_than_balance_raises_error'
```

### Write the minimal amount of code for the test to run

The simplest way to create a custom error is by simply creating an empty class inheriting from `StandardError`, like this:

```ruby
class NotEnoughFundsError < StandardError; end
```

And that's enough!

A question that can arise now is: in which file should we put this?

Let's keep things simple and declare it in the same file as the wallet code. So, add this to the top of your `wallet.rb`:

```ruby
class NotEnoughFundsError < StandardError; end

class Wallet
  # ...
end
```

Run the tests and you should see this **failure**:

```
  1) Failure:
WalletTest#test_withdraw_more_than_balance_raises_error [code/wallet/wallet_test.rb:25]:
[NotEnoughFundsError] exception expected, not
Class: <ArgumentError>
Message: <"Not enough funds">
---Backtrace---
/.../code/wallet/wallet.rb:15:in 'Wallet#withdraw'
code/wallet/wallet_test.rb:25:in 'block in WalletTest#test_withdraw_more_than_balance_raises_error'
---------------
```

### Write enough code to make the test pass

We already saw that kind of failure. To fix that we just need to change our code and make it raise the expected error. Then our `#withdraw` method becomes like this:

```ruby
def withdraw(amount)
  raise NotEnoughFundsError, "Not enough funds" if amount > @balance

  @balance -= amount
end
```

Run the tests and they should pass.

### Refactor

You know what? There's a redundancy in this line that's bothering me:

```ruby
raise NotEnoughFundsError, "Not enough funds" if amount > @balance
```

If the error is named `NotEnoughFundsError` I think there's no need to say "Not enough funds" again. Then let's just remove that message from the code:

```ruby
raise NotEnoughFundsError if amount > @balance
```

Taking a look at our `wallet.rb` as a whole:

```ruby
class NotEnoughFundsError < StandardError; end

class Wallet
  attr_reader :balance

  def initialize
    @balance = 0
  end

  def deposit(amount)
    @balance += amount
  end

  def withdraw(amount)
    raise NotEnoughFundsError if amount > @balance

    @balance -= amount
  end
end
```

Run the tests and they should pass.

Let's mark the last item from our todo list as done and wrap up this chapter.

> - [x] a wallet starts with zero balance
> - [x] a deposit in a wallet immediately reflects in its balance
> - [x] a withdraw from a wallet immediately reflects in its balance
> - [x] a withdraw with an amount greater than the current balance must result in an error
> - [x] withdraw when there's no funds must raise a specific error

## Handling Edge Cases

Here are some ideas of scenarios for you to write tests and implement:

> - [ ] a deposit with a non-positive value must raise an error
> - [ ] a withdraw with a non-positive value must raise an error
> - [ ] the use of non-positive values must raise a specific error (e.g.: `InvalidAmountError`)

## Key Concepts

### Ruby

- A symbol is a way to have a unique and reusable identifier to be used in our Ruby code (see the [Symbol class documentation](https://ruby-doc.org/3.4.1/Symbol.html) for more info).
- Attribute accessors are useful to provide access to attributes with a less verbose notation.
- An Exception is used to indicate that an error has occurred in our program. Useful information can be found in the [Exception class documentation](https://ruby-doc.org/3.4.1/Exception.html)
- We explicitly `raise` exceptions when we want to make clear to the users of our code that they're doing something wrong
- A simple way to create a custom exception is by creating an empty class inheriting from `StandardError`.
- This is just an intro to Ruby exceptions. In later chapters we will cover more sophisticated scenarios.

### Testing

- Again, writing down in a list the ideas for tests as soon as they appear in your mind is useful to keep your focus on the task you're solving at the moment.
- `assert_raises` expects the code that raises the exception to be in a block. Check [the documentation](https://ruby-doc.org/3.4.1/gems/minitest/Minitest/Assertions.html#method-i-assert_raises) for more info.
