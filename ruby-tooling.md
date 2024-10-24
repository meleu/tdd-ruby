# Ruby tooling

## Install Ruby

There are many options to install Ruby on your system, [the official web page](https://www.ruby-lang.org/en/documentation/installation/) lists a bunch of them.

I prefer using a runtime version manager like [asdf](https://asdf-vm.com/) or [mise](https://mise.jdx.dev). If you're not comfortable with version managers, try to install using your favorite package manager (e.g.: [Homebrew](https://brew.sh)) or your system's official one.

The code in this book was tested with Ruby 3.2.2, and should work on any version 3.1+.

## Testing Framework

The Ruby community has two main testing frameworks:

- RSpec: a feature-rich testing framework that provides a Domain Specific Language for writing tests.
- **Minitest**: a lightweight testing framework that provides utilities to write your tests using "pure Ruby"

In this book we're going to use **Minitest**, for two reasons:

1. it comes with the Ruby Standard Library.
2. you only have to know ruby to use Minitest

## Ruby documentation

Bookmark this website: <https://ruby-doc.org/>

It has the official documentation for the Ruby language and we're going to use it a lot.

One good tip about using the official documentation is how to use the Search tool. When you know the class and the method you're looking for, you search by `ClassName#method_name`. This leads you to where you want. This notation is also frequently used in the Ruby literature.

Even when not prefixed with the class name, the `#` symbol used as a prefix implies that we're talking about a method. Example: `#puts`.

## Interactive Ruby Shell (`irb`)

Using `irb` is an awesome way to quickly try things out, with no need to put code in a file, save it and call the file from the command line. When you're in `irb` you just need to type the code and check the results.

In order to use the Interactive Ruby Shell you just need to type `irb` in your terminal. You should see something similar to this:

```
$ irb

irb(main):001:0> 
```

This is the `irb` prompt, where you can type Ruby code and immediately see the results.

Just to experiment, let's try a "hello world":

```
irb(main):001> puts "Hello, World!"
Hello, World!
=> nil
irb(main):002>
```

If you got a result similar to this 👆, then everything is fine.

The output there shows the printed string `Hello, World!` and then it shows the value returned by the `puts` method. Don't worry if you don't understand these concepts for now. In almost all chapters of this book you'll be invited to try things out on `irb`. So you'll soon be very familiar with it. For now it was just a test.

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


## Basic Ruby coding style

Throughout this book you'll see how Rubyists usually write their code. For now the only thing we need to know is that we name our variables and methods using `snake_case` (not `camelCase`).

### Ruby Linter and Formatter

[RuboCop](https://rubocop.org) is the most popular Ruby linter/formatter available.

If you have RuboCop configured in your editor, you'll probably see some lint diagnostics while coding. Although those messages are useful, they can be a distraction when you're trying to learn Ruby for the first time. If you think they're distracting you, just ignore the messages during the first chapters. After you get more familiar with Ruby, you'll start to see the value of those warnings.

If your editor has a "format on save" feature, I suggest to let it disabled (at least while working in the first chapters).

## Before we start...

In order to keep our code in a centralized directory, where we can easily go to whenever we want, let's create an environment variable named `TDD_RUBY_PATH` with the full path to the directory we'll use to save our code.

In my case, I use `${HOME}/src/tdd-ruby-code`, then I would declare the variable like this:

```bash
export TDD_RUBY_PATH="${HOME}/src/tdd-ruby-code"
```

I recommend you to put this 👆 line of code in your `.bashrc` or `.zshrc` file (or whatever file you use to configure your shell). This is going to be useful for when you want to quickly `cd $TDD_RUBY_PATH`.

We'll also keep track of our changes with git, then let's initialize a repository:

```bash
cd $TDD_RUBY_PATH
git init
```


## Wrapping up

At this point you should have:

- Ruby installed
- typed at least one command inside `irb`
- visited **and bookmarked** <https://ruby-doc.org/>
- understood that we name variables and methods using `snake_case`
- a text editor available
- created the `TDD_RUBY_PATH` variable in your shell
- initialized a local git repository
