# Ruby tooling

## Install Ruby

There are many options to install Ruby on your system, [the official web page](https://www.ruby-lang.org/en/documentation/installation/) lists a bunch of them.

I prefer using a runtime version manager like [asdf](https://asdf-vm.com/) or [mise](https://mise.jdx.dev). If you're not comfortable with version managers, try to install using your favorite package manager (e.g.: [Homebrew](https://brew.sh)) or your system's official one.

The code in this book was tested with Ruby 3.2.2, and should work on any version 3.1+.

## Testing Framework

In this book we're going to use **Minitest**. It already comes with the Ruby Standard Library, so if you installed Ruby, you're ready to use Minitest.

## Ruby documentation

Bookmark this website: <https://ruby-doc.org/>

It has the official documentation for the Ruby language and we're going to use it a lot.

A good tip about the official documentation is how to use the Search tool. When you know the class and the method you're looking for, you search by `ClassName#method_name`. This leads you to where you want. This notation is also frequently used in the Ruby literature.

Even when not prefixed with the class name, the `#` symbol used as a prefix implies that we're talking about a method. Example: `#puts`.

## Interactive Ruby Shell (`irb`)

Using `irb` is an awesome way to quickly try things out, with no need to put code in a file, save it and call the file from the command line. When you're in `irb` you just type Ruby code and check the results.

In order to use the Interactive Ruby Shell, type `irb` in your terminal. You should see something similar to this:

```
$ irb

irb(main):001:0> 
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


## Basic Ruby coding style

Throughout this book you'll see how Rubyists usually write their code. For now the only thing we need to know is that we name our variables and methods using `snake_case` (not `camelCase`).

### Ruby Linter and Formatter

[RuboCop](https://rubocop.org) is the most popular Ruby linter/formatter available.

If you have RuboCop configured in your editor, you'll probably see some lint diagnostics while coding. Although those messages are useful, they can be a distraction when you're trying to learn Ruby for the first time. If you think they're distracting you, just ignore the messages during the first chapters. After you get more familiar with Ruby, you'll start to see the value of those warnings.

If your editor has a "format on save" feature, I suggest to let it disabled (at least while working in the first chapters).

### Coding Assistants

If you have coding assistants, like GitHub Copilot, on your editor, you should turn off it while going through the book. Otherwise it will ruin your learning. Using them while reading the book would produce an [illusion of competence](https://www.memory-improvement-tips.com/illusions-of-competence.html) and would be a waste of your time.

I want you to leverage these kind of tools **after** learning what you're about to learn in this book.


## Before we start...

In order to keep our code in a centralized directory, where we can easily go to whenever we want, let's create an environment variable named `TDD_RUBY_PATH` with the full path to the directory we'll use to save our code.

In my case, I use `${HOME}/code/tdd-ruby-code`, then I would declare the variable like this:

```bash
export TDD_RUBY_PATH="${HOME}/code/tdd-ruby-code"
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
