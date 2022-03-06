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

I love interfaces. Why? Because they are a simple tool to create _abstractions_ and all this _computer programming_ thing is about building and using abstractions.
Programming is about building, the right, abstractions that allows us master the complexity of the problem we are solving. 

> _Controlling complexity is the essence of computer programming_
>
> Brian Kernighan

and

> _We control complexity by building abstractions that hide details when appropriate. 
> We control complexity by establishing conventional interfaces that enable us to construct systems by combining standard, well-understood pieces in a "mix and match" way._
> 
> [Structure and Interpretation of Computer Programs](https://doc.lagout.org/programmation/Lisp/Scheme/SICP.pdf)
 
As you can imagine, because interfaces are such an important tool in building software, the idea of programming languages including means to create interfaces is not new.
Interfaces are present in programming languages from the '70s, for example in [Modula](https://www.research-collection.ethz.ch/handle/20.500.11850/68669) (yes, long before Java and all the object oriented hype)

Defining interfaces is not necessarily easy.
The above citation subtly mentions one difficulty in defining interfaces: _We control complexity by building abstractions that hide details **when appropriate**._

When do we need to add an interface? 
What the interface should expose?
In other words: What is the _contract_ that our component should expose to its environment?

By stating the above questions we might be tempted to see interfaces from, let's say, a Modula/C++/Java approach to interfaces where the focus is on the implementors of the interface rather than on its consumers.






Interface segregation principle