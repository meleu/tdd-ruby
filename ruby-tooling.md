# Ruby tooling

## Install Ruby

There are many options to install Ruby on your system, [the official web page](https://www.ruby-lang.org/en/documentation/installation/) lists a bunch of them.

I prefer using a runtime version manager like [asdf](https://asdf-vm.com/) or [mise](https://mise.jdx.dev). If you're not comfortable with version managers, try to install using your favorite package manager (e.g.: [Homebrew](https://brew.sh)) or your system's official one.

The code in this book was tested with Ruby 3.2.2, and should work on any version 3.1+.

Open up your terminal and run `ruby --version`. You should see something like this:

```
$ ruby --version
ruby 3.2.2
```

## Minitest

In this book we use **Minitest**.

### Why Minitest?

1. you only have to know Ruby to use Minitest
2. it comes with the Ruby Standard Library

When I first tried to learn about testing in Ruby I was influenced by the information that most job opportunities require RSpec. Then I noticed I spent more energy learning the "RSpec language" than the principles of TDD. I didn't enjoy the experience and I hope you don't feel this pain too.

Just keep in mind that the principles you'll learn here, are easily applicable with RSpec too.

## Linter and Formatter

[RuboCop](https://rubocop.org) and [StandardRB](https://github.com/standardrb/standard) are two popular linters and formatters for Ruby. I recommend StandardRB, as it's simpler and has less things to configure.

If you have one of these linters and formatters configured in your editor, just be mindful that it can sometimes change your code in a way you don't want (e.g.: delete a variable that you don't use *yet*).

If you think the auto formatter is confusing you, try to turn off the "format on save" feature of your editor (at least while working in the first chapters).

## Run tests quickly

During the practices we'll run tests frequently, and we want to easily trigger them. Maybe you'll need to discover the key combination used to trigger the tests from your editor, but I can suggest an agnostic solution: the [rerun gem](https://rubygems.org/gems/rerun).

Install it with `gem install rerun`.

Once it's installed we can easily trigger a test on file update. Example:

```bash
rerun -x -- ruby my_test.rb
```

## A note on Coding Assistants

**You should not use AI coding assistants while going through the book,** otherwise it will ruin your learning.

Don't get me wrong! I know that tools like GitHub Copilot or the Cursor editor are useful, and I also use them in my daily job. The problem is that using them while doing the exercises in this book would cause an [illusion of competence](https://www.memory-improvement-tips.com/illusions-of-competence.html) and that would be a waste of your time.

I want you to leverage these tools **after** learning what you'll learn from this book.

## Interactive Ruby Shell (`irb`)

Using `irb` is an awesome way to quickly try things out, with no need to put code in a file, save it and call the file from the command line. When you're in `irb` you just type Ruby code and check the results.

In order to use the Interactive Ruby Shell, type `irb` in your terminal. You should see something similar to this:

```
$ irb

irb(main):001> 
```

This is the `irb` prompt, where you can type Ruby code and see the results.

Let's try a "hello world":

```
irb(main):001> puts "Hello, World!"
Hello, World!
=> nil
irb(main):002>
```

If you got a result similar to this 👆, then everything is fine.

The output there shows the printed string `Hello, World!` and then it shows the value returned by the `puts` method. Don't worry if you don't understand these concepts for now. In many chapters you'll be invited to try things out on `irb`. So you'll soon be very familiar with it. For now it was just a test.

To exit the `irb` just type `exit`:

```
irb(main):002> exit
```

**NOTE**: to make it easier to read, throughout the book the `irb` sessions will be presented omitting the verbose prompt, and the return value will appear as a comment, like here:

```ruby
# IRB SESSION

> puts "Hello, World!"
Hello, World!
#=> nil

> exit
```

## Ruby documentation

Bookmark this website: <https://ruby-doc.org/>

It has the official documentation for the Ruby language and we're going to use it a lot.

A good tip about the official documentation is how to use the Search tool. When you know the class and the method you're looking for, you search by `ClassName#method_name`. This leads you to where you want. This notation is also frequently used in the Ruby literature.

Even when not prefixed with the class name, the `#` symbol used as a prefix implies that we're talking about a method. Example: `#puts`.

## Wrapping up

At this point you should have:

- Ruby installed
- installed the `rerun` gem
- typed at least one command inside `irb`
- visited **and bookmarked** <https://ruby-doc.org/>
- a text editor available
