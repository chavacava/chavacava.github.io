---
tags: abstraction interfaces
---
# #2 Interfaces

This note will be about _interfaces_.
No need to be a programmer to know about interfaces.
They are everywhere in our life. 
Right now you might be using many.
And right now you are thinking "hey! you _chavacava_, you did not yet define what you exactly mean by _interface_".
You are right, interfaces are so many things (your computer keyboard, an USB port, the electricity plug, the screen you are looking at, the REST endpoint that served this page, ...), but as you suspect, I will talk (write in fact!) about interfaces in programming languages.
Yes, these interfaces that you usually declare with an `interface` keyword.

I love interfaces. 
Why? 
Because they are a simple tool to create _abstractions_ and all this _computer programming_ thing is about building the right abstractions that allows us master the complexity of the problem we are solving.
I'm not discovering something here:

> _Controlling complexity is the essence of computer programming_
>
> Brian Kernighan

and

> _We control complexity by building abstractions that hide details when appropriate. 
> We control complexity by establishing conventional interfaces that enable us to construct systems by combining standard, well-understood pieces in a "mix and match" way._
> 
> [Structure and Interpretation of Computer Programs](https://doc.lagout.org/programmation/Lisp/Scheme/SICP.pdf)
 
As you can imagine, because interfaces are such an important tool in building software, the idea of programming languages including means to create interfaces is not new.
Interfaces are present in programming languages from the '70s, for example in [Modula](https://www.research-collection.ethz.ch/handle/20.500.11850/68669) (yes, long before Java and all the object oriented wave)

Defining interfaces is not necessarily easy.
The above citation subtly mentions one difficulty in defining interfaces: _We control complexity by building abstractions that hide details **when appropriate**._

When do we need to add an interface? 
What the interface should expose as behavior?
These are not simple to answer questions, the talent of a developer can be measured by how she/he answer to these questions.
 
By thinking on the above questions we might be tempted to see interfaces from, let's say, a Modula/C++/Java point of view where the focus is on the implementors of the interface rather than on its consumers.
We ask _What is the contract that our component should expose to its environment?_ rather than _What are the contract that must be exposed by those components that our component uses?_

Languages like Java, where each component (classes in that case) must declare what interfaces they implement kindly force us to think on interfaces from the implementor point of view.
That leads naturally to bigger interfaces.
And we know that
> _The bigger the interface, the weaker the abstraction_
>
> Rob Pike

Interfaces defining many method are, in Java, the norm and not the exception.
You might argue that this is not the fault of the language and blame  developers.
But I think Java approach to interfaces has some credit in that inclination to define big interfaces.

The GO language takes a totally different approach: components do not need to declare what interfaces they implement.
The compiler will control the interface compatibility between interacting components.

The GO approach allows developers to put the focus on the interface consumer side.
An interface can be created (declared) in the same place you need to use it.
This naturally leads to smaller interfaces because the abstraction is created where it is needed and with just the appropriate behavior.

Let's illustrate with an example.
Imagine you want to copy bytes from a `source` to a `destination`
```go
func Copy(source ???, destination ???) error { ... }
```

What type should be `source`? And `destination`?
In GO we can define those types in-place, beside the function using them.

```go
func Copy(source reader, destination writer) error { 
    ... source.Read() ... destination.Write() ... 
}

type reader interface {
    Read() ([]bytes, error)
}

type writer interface {
    Write([]byte) error
}
```

Then when calling `Copy` we can pass as `source` any component exposing a `Read` method (the same for `destination` and `Write` but you already got the point)
And, very important, these components are not aware they implement these interfaces (in fact, in the example, the interfaces are declared as not public)
That means we can call `Copy` by passing components whom are not under our control (we can not do that in Java)

Java (and the like) do not forbids doing this but is more laborious.

Abstract classes are not interfaces!

Interface segregation principle