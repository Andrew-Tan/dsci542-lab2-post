## Introduction to functional programming

Procedural programming, object-oriented programming, functional programming, we refer to these terms as programming paradigms. They practically define how you should style or structure your code to make it more efficient and easy to read.

Functional programming, in particular, has long been seen as the future standard for writing code. However, even those who have been working in the computer science industry for many years are still not able to properly understand this concept and make use of it properly in regular programming works. It is not very surprising as many people might think code style is not very important. After all, if the code works, then everything is good. There is no need to care about these details if we are not computer science experts... Right? Well, no. Being able to use functional programming can be vital, especially if you are working in a team and your code will need to be understood by others. Functional programming can improve the maintainability, readability, efficiency of your code greatly, and help others avoid making some mistakes when using your code.

This article aims to provide a brief introduction to functional programming, in the form of comparing against another popular programming paradigm: object-oriented programming, to explain the characteristic of functional programming and why it is better.

#### Object-oriented programming (OOP) and its problems

Object-oriented programming (OOP) is probably the most popular programming paradigm in recent years. If you look at the [PYPL Popularity of Programming Language](https://pypl.github.io/PYPL.html) index, you'll probably notice that the top 5 most popular programming languages (except for JavaScript) are mainly designed for OOP. The core concept of OOP is "object". You want to design your code base mainly with the concept of objects. For example, let us design a `Wallet` class. A wallet class should be able to hold cash, and we should be able to put in or draw cash from the wallet. One design in Python could be the following:

```python
# We should probably handle edge cases as well, but let us ignore it for simplicity.
class Wallet:
    def __init__(self, initial_cash=0):
        self.cash = initial_cash
    
    def put(self, amount):
        self.cash = self.cash + amount
    
    def draw(self, amount):
        self.cash = self.cash - amount
    
    def cash_in_wallet(self):
        return self.cash


# Example usage
wallet = Wallet(10)
wallet.put(5)
wallet.draw(3)

wallet.cash_in_wallet()
# Should return 12
```

This is a simple and classical example of an OOP class. The `Wallet` class holds an internal state `cash`, which gets modified when calling `put()` or `draw()`. Such design is very neat, intuitive, and also has much better maintainability. But it also comes with quite a few drawbacks. One of the biggest drawbacks is mutability. Every time `put()` and `draw()` are called, they modify the internal state `cash` in place. This characteristic might be acceptable if our program is single-threaded, but it quickly becomes a serious issue when we start trying to scale our program to be able to run in multiple threads or even multiple machines. Our program could run into [race conditions](https://en.wikipedia.org/wiki/Race_condition) when multiple threads are trying to modify the same state at roughly the same time.

![race condition](https://miro.medium.com/max/512/1*15B0mRJWmLWUxmw4QT7DCw.gif)

Animated race condition [Source](https://medium.com/@val_deleplace/does-the-race-detector-catch-all-data-races-1afed51d57fb)

Race conditions are one of the worst issues that can happen to our programs, as they make the programs' behavior unpredictable. Although we could use [locking](https://en.wikipedia.org/wiki/Lock_(computer_science)) to guarantee that only one entity can access this state at a given time, that comes with the cost of decreased performance and increased complexity of code.

#### Functional programming (FP) to the rescue

Functional programming (FP), on the other hand, takes a completely different approach than object-oriented programming. The core idea here is, we focus more on functions than objects. Under this paradigm, functions are treated as objects, which means we will be able to pass them, return them, or build them from other functions. Moreover, objects become immutable right after their creation. If we want to "modify" a state of an object, we have to create a new one with the modified state. As an example, let us re-design our `Wallet` class earlier in a functional programming way:

```python
# We should probably handle edge cases as well, but let us ignore it for simplicity.
class Wallet:
    def __init__(self, initial_cash=0):
        # There's no way in Python to prevent a state from being modified on the language level.
        # (Since Python is mainly designed for Object-oriented programming)
        # However, if we want to implement our library in a functional way, we should never change this state directly.
        self.cash = initial_cash
    
    def cash_in_wallet(self):
        return self.cash


def put(wallet, amount):
    current_cash = wallet.cash_in_wallet()
    return Wallet(current_cash + amount)


def draw(amount):
    current_cash = wallet.cash_in_wallet()
    return Wallet(current_cash - amount)


# Example usage
wallet = Wallet(10)

wallet = put(wallet, 5)
wallet = draw(wallet, 3)

wallet.cash_in_wallet()
# Should return 12
```

Under a functional programming paradigm, we no longer modify the internal state `cash` in `put()` and `draw()`, but instead creating a new `Wallet` object whenever we need to change `cash`. By rewriting `put()` and `draw()`, we turned them into **pure functions**, which do not have **side effects** that modify states outside of the function's local environment (e.g. `cash`). 

In functional programming, all functions must be pure functions without any side effects. By no longer requiring changing internal states, we eliminate potential race conditions fundamentally and make our code much easier to reason about. Note that, the language used for the examples above is Python, which is a language that almost provides no help in enforcing the functional programming paradigm. There are languages such as Scala that are originally designed for functional programming.

#### Why functional programming is not the norm (yet)

While having so many advantages over object-oriented programming, there is one big question: why functional programming is not the norm? why the object-oriented style is still the most popular paradigm? Well, this could be just a matter of time. The presentation "[Why Isn't Functional Programming the Norm?](https://www.youtube.com/watch?v=QyJZzq0v7Z4&t=1107s)" by Richard Feldman left a deep impression on me when I watched it a few years ago. I generally agree with his opinion in the presentation. 

If you look back at the history of programming language, you'll find that the transition between languages is very gradual. This is also happening with programming paradigms. People first discover various styles in existing programming languages that could improve the quality of their code, then as some styles continue getting popularity, they gradually turn into paradigms. Next, people will start to realize the limitations of using existing languages with the new paradigm as the fundamental design of the language is not for the new paradigm. This then becomes the beginning of the emergence of another new programming language. Such a process had happened for the transition to object-oriented programming, and there is no reason to not believe that it will happen again for functional programming. In fact, many existing languages (e.g. Java, Python) have been implementing new language features that would make functional programming easier, and we also see many new functional programming languages (e.g. Scala, Kotlin) emerges and slowly gaining popularity. These are all signals to us to believe that functional programming is being more and more accepted as the future.
