## #1 Testing is bad...

...,really bad, at checking your code **does not** have bugs.
As our friend Edsger Dijkstra said:

> _testing can be a very effective way to show the presence of bugs, but it is hopelessly inadequate for showing their absence_

Under this perspective a test should be considered successful only if it has actually found bugs.
Running hundreds or thousands of tests without spotting bugs is, possibly, more the symptom of poor tests than a proof of code correctness.

So, how can you show that your code does not have bugs?
Well, to reach a high degree of confidence, and even the proof, of your code been "bug free" you could use **formal methods**: a set of techniques that allow you specify, develop and verify your programs in a mathematical rigorous way.

Even if _formal methods_ is today a living and dynamic research domain in computer science, its roots are in the, now far, 70's when the _program correctness_ problem emerged.
Since then, multitude of techniques were developed but their utilization remains almost anecdotal.
Why? Because putting in practice these techniques still requires skills far beyond those we learn in traditional engineering education.
In resume, formal methods are still a "_researcher thing_" rather than an "_industrial tool_"

## Look mom... no tests!

To let you grasp the potential of formal methods I'll introduce you a concrete example of their use on code written in Java, a language widely used in industry.

The following code is plain Java annotated with [Java Modeling Language (JML)](https://www.cs.ucf.edu/~leavens/JML/index.shtml) specifications.

```java
// Borrowed from Formal Specification with JML (https://research.utwente.nl/en/publications/formal-specification-with-jml)
public class LimitedIntegerSet {
    public final int limit ;
    private int arr [];
    private int size = 0;

    public LimitedIntegerSet ( int limit ) {
        this.limit = limit ;
        this.arr = new int [ limit ];
    }

  /*@ requires size < limit && ! contains ( elem );
    @ ensures \result == true ;
    @ ensures contains ( elem );
    @ ensures ( \forall int e ;
    @                   e != elem ;
    @                   contains ( e ) <== > \old ( contains ( e )));
    @ ensures size == \old ( size ) + 1;
    @
    @ also
    @
    @ requires ( size == limit ) || contains ( elem );
    @ ensures \result == false ;
    @ ensures ( \forall int e ;
    @                   contains ( e ) <== > \old ( contains ( e )));
    @ ensures size == \old ( size );
    @*/
    public boolean add ( int elem ) { /* ... */ }

  /*@ ensures ! contains ( elem );
    @ ensures ( \forall int e ;
    @                   e != elem ;
    @                   contains ( e ) <== > \old ( contains ( e )));
    @ ensures \old ( contains ( elem ))
    @           == > size == \old ( size ) - 1;
    @ ensures ! \old ( contains ( elem ))
    @           == > size == \old ( size );
    @*/
    public void remove ( int elem ) { /* ... */ }

  /*@ ensures \result == ( \exists int i ;
    @                               0 <= i && i < size ;
    @                               arr [ i ] == elem );
    @*/
 public /* @ pure @ */ boolean contains ( int elem ) { /* ... */ }

 // other methods
}
```
Clear, right?

JML specifications follow the _design-by-contract_ paradigm establishing _preconditions_ that must hold to successfully call a method (the `requires` annotations in the code) and _postconditions_ the method must hold at the end of its execution (`ensures`)

The beauty of this is that once JML specifications are written, you can run tools that will **statically** (i.e. without executing your code) check that method implementations respect the spec.

While some researchers claim that formal methods are a replacement for testing, a more pragmatic approach is to see them as complementary tools.

## Test, test, and test
Well not sure you can forget tests... formal methods are not mainstream and even with them it seems that we will need testing. 
So lets write better tests to catch more bugs (we know they are there)!

We are, in average, not so good at writing tests. 
Why? 
Well there are many factors: 
some studies highlight differences in the mindset of _programmer_ and _tester_ roles and the difficulty we have to incarnate both; 
we have less experience (flight hours) in testing than in developing (yes I know both are the same but you know what I mean); 
often when testing we focus on the wrong goal (coverage) and not on finding bugs.

We test by feeding our code with some inputs we consider interesting and checking if the code behaves as expected; in other words we do _example-based testing_
What can be wrong?
Inputs!
It seems we are not good to choose our examples :)

What can we do to better test our programs? 
We could start delegating to the computer the generation of a subset of our tests inputs. We are lucky and some testing approaches and their corresponding tools can help us in this task, for example:

- Property-based-testing
- Fuzzy-testing

This note is taking too long so I'll not dive in the details of these two testing approaches (maybe in a future note) but I know you will check them (I you were not yet aware of them) and look for the available tooling for your favorite language.

Testing is very important (I do not need to tell you why) and we need to be better at.

# Bonus-track Game

Lets say you must test a function that receives three segment lengths and it returns the kind of triangle these segments form.
The function signature is in the lines of:
```
func triangleKind(s1, s2, s3 float) kind 
```

How many test cases you can imagine?