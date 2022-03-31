---
tags: abstraction interface
---
# #2 Size matters... specially when talking about interfaces

This note will be about _interfaces_.
No need to be a programmer to know about interfaces.
They are everywhere in our life. 
Right now you might be using many.
And right now you are thinking "hey! you did not yet define what you exactly mean by _interface_".
You are right, interfaces are so many things (your computer keyboard, an USB port, the electricity plug, the screen you are looking at, the REST endpoint that served this page, ...), but as you might suspect, we will talk about interfaces in programming languages.
Yes, these things you usually declare with the `interface` keyword.

I will say it from the beginning: :heart_eyes: I love interfaces :heart_eyes: 
Why? 
Because they are a simple tool to create _abstractions_ and all this _programming_ thing is only about building the right abstractions to master the complexity of the problem we are trying to solve.
I'm not saying something new here:

> _Controlling complexity is the essence of computer programming_
>
> Brian Kernighan

and

> _We control complexity by building abstractions that hide details when appropriate. 
> We control complexity by establishing conventional interfaces that enable us to construct systems by combining standard, well-understood pieces in a "mix and match" way._
> 
> [Structure and Interpretation of Computer Programs](https://doc.lagout.org/programmation/Lisp/Scheme/SICP.pdf){:target="_blank" rel="noopener"}
 
As you can imagine, interfaces being such an important tool in building software, the idea of programming languages including means to create them is not new.
Interfaces are present in programming languages from the '70s, for example in [Modula](https://www.research-collection.ethz.ch/handle/20.500.11850/68669){:target="_blank" rel="noopener"}. 
Yes, long before Java and the whole object oriented wave. 
Interfaces and modules, or components, are related concepts so it is not a surprise to see interfaces in the language which introduced modules in programming.

Defining interfaces is not necessarily easy.
The above citation subtly mentions one difficulty in defining interfaces: _We control complexity by building abstractions that **hide details when appropriate**._

When do we need to add an interface? 
What the interface should expose as behavior?
These are not simple to answer questions, we could measure the talent of a developer by how she/he answers to these questions.

# Smaller-the-Better 

By thinking on the above questions we might be tempted to see interfaces from, let's say, a Modula/C++/Java point of view where the focus is on the implementer of the interface rather than on its consumers.
In these languages we usually ask _What is the contract my component should expose to its environment?_ rather than _What  contract must be exposed by those components my component uses?_

Languages like Java, where each component (class) must declare what interfaces it actually implements (_nominal subtyping_) kindly force us to think on interfaces from the implementer point of view, leading naturally to bigger interfaces.
And we know that
> [_The bigger the interface, the weaker the abstraction_](https://www.youtube.com/watch?v=PAAkCSZUG1c&t=5m17s){:target="_blank" rel="noopener"}
>
> Rob Pike

Interfaces defining many methods are, in Java, the norm and not the exception.
You might argue that this is not the fault of the language and blame developers.
But I think nominal subtyping has some credit in that inclination to define big interfaces.

Other languages use a totally different approach: components do not need to declare what interfaces they implement.
The compiler will control the interface compatibility between interacting components by checking if used components expose the methods required by the consumers without requiring explicit declaration of implemented interfaces, that is _structural subtyping_.

Structural subtyping allows developers to put the focus on the interface consumer side.
We can say it allows _consumer driven design_ of interfaces.

In GO, for example, an interface can be declared in the same place you need to use it.
This naturally leads to smaller interfaces because the abstraction is created exactly where it is needed and with the strictly necessary behavior (this is in line with the [Interface Segregation Principle](https://en.wikipedia.org/wiki/Interface_segregation_principle){:target="_blank" rel="noopener"}, isn't it?).

Let's illustrate the GO approach to interfaces with an example.
Imagine you want to copy bytes from a `source` to a `destination`
```go
func Copy(source ???, destination ???) error { ... }
```

What type should be `source`? And `destination`?

In GO we can define those types in-place, beside the function using them.
So let's define `reader` and `writer` types and use them in the definition of `Copy`

```go
type reader interface {
    Read() ([]bytes, error)
}

type writer interface {
    Write([]byte) error
}

func Copy(source reader, destination writer) error { 
    ... source.Read() ... destination.Write(bytes) ... 
}
```

Then when calling `Copy` we can pass as `source` any component exposing a `Read` method (the same for `destination` and `Write`,  you already got the point)
And, very important, these components are not aware they implement these interfaces (notice that, in the example, interfaces are declared as not public, in fact in GO we could even use anonymous interfaces)
therefore we can call `Copy` by passing components not under our control (we can not do that in a language with nominal subtyping)   

Of course you can define small interfaces (a.k.a. _role interfaces_) in Java, and in other languages with nominal subtyping, but it demands more work: when adding a new interface you need to update (if possible!) the list of implemented interfaces of all implementing classes.
Some people see this as a good thing because it avoids _accidental subtyping_.

Independently from the subtyping approach of the language we use, :point_right: **striving for small interfaces is always a winning bet** :point_left:
