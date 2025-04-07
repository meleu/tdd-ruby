# Learn Ruby with TDD

⚠ **This is a work in progress by [meleu](https://github.com/meleu)!** ⚠

This book aims to teach you **Ruby** and **Test-Driven Development** (spoiler: we're going to use **Minitest** as the testing framework).

Since we use Ruby, it's inevitable that we also talk about **Object-Oriented Programming** (OOP). And as we use TDD, it's inevitable that we're going to talk about **Software Design**.


## Why?

### Why Ruby?

Ruby has an elegant syntax that is natural to read and easy to write. It offers an extremely productive ecosystem where we can quickly build a product and put it online.


### Why TDD?

There are many books and articles talking about the benefits of TDD. There are also articles defaming TDD, but if you're reading this book I believe you have hope that it's a useful skill.

Of course I'm on the side of those who think that TDD is useful. The strongest argument in favor of TDD is that **it promotes a better software design**. By using TDD you are encouraged to make your code testable, which means less coupled and easier to maintain.

Now, instead of repeating all the other good arguments already mentioned in those books and articles, I'm going to mention a new one:

**In the current AI era TDD is more valuable than ever**.

With AIs writing code much faster than humans, such activity becomes cheaper. This leads me to a conclusion that **the real value is less in the code, and more in the tests**. Your ability to convert real world requirements into runnable tests will make you a more productive and valuable professional.

### Why Minitest?

The most used testing frameworks in the Ruby community are:

- RSpec: a feature-rich testing framework that provides a Domain Specific Language for writing tests.
- **Minitest**: a lightweight testing framework that provides utilities to write your tests using "pure Ruby"

You will soon realize (or already know), that most of the Ruby jobs out there use RSpec. So why did I decide to use Minitest in this book? Two reasons:

1. you only have to know Ruby to use Minitest
2. it comes with the Ruby Standard Library

When I first tried to learn about testing in Ruby I was influenced by the information that most job opportunities require RSpec. Then I noticed I spent more energy learning the "RSpec language" than the principles of TDD. I didn't enjoy the experience and I hope you don't feel this pain too.

This book aims to teach you Ruby and the TDD principles at the same time. This is already an ambitious goal. If I were to use RSpec, that would be one more thing to be taught, and it would be overwhelming.

Just keep in mind that with the testing practices and principles you'll learn here, you'll be able to smoothly learn the RSpec DSL later.


## What you should expect

- Explore the Ruby language by writing tests.
- **Get a solid grounding with TDD**.
- Strengthen your knowledge of OOP
- Be confident that you'll be able to start writing robust, well-tested software in Ruby.

The chapters are presented in a sequence where the Ruby features are introduced and used to create a program. The implementation is always driven by tests, reinforcing the TDD cycle, and with an Object-Oriented approach.

Initially the TDD cycle may seem tedious, but as you progress through the book you'll see how productive it is to get instant feedback on your work.

You'll notice that the first programs are really simple. That's intentional. The greatest value will be in the way we solve the problems. The thought process, the tools we use, the tests we write, the way we organize our code...

If I used "real world problems", I would need to spend words explaining the problem's domain and then talk about the techniques. That would be lengthy and confusing. By keeping the problems simple, I'm sure you'll find a way to adapt the techniques to the "real world problem" you have at hand.


## Who this is for

- People who are interested in picking up Ruby.
- People who already know some Ruby but want to explore testing more.

I assume you already wrote some code (any language) and understand the basic concepts of algorithms.

It's also expected that you're comfortable using the terminal (or at least not scared to use it).

Basic knowledge of git is helpful, but not mandatory.


## What you'll need

- A computer
- A Unix-like operating system (Linux, MacOS or Windows with WSL)
- Installed Ruby (3.1+)
- Installed git
- A text editor
- A terminal


## Feedback

Open issues or submit Pull Requests in [this book's repository](https://github.com/meleu/tdd-ruby).


## Credits

This book is inspired by the awesome [Learn Go with Tests](https://quii.gitbook.io/), by [Chris James](https://quii.dev/). If you're interested in Golang, you should check that!
