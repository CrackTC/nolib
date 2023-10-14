# prologue

> Above all, I hope we don't become missionaries. Don't feel as if you're Bible salesmen. The world has too many of those already. What you know about computing other people will learn. Don't feel as if the key to successful computing is only in your hands. What's in your hands, I think and hope, is intelligence: the ability to see the machine as more than when you were first led up to it, that you can make it more.

> These skills are by no means unique to computer programming. The techniques we reach and draw upon are common to all of engineering design. We control complexity by building abstractions that hide details when appropriate. We control complexity by establishing conventional interfaces that enable us to construct systems by combining standard, well-understood pieces in a "mix and match" way. We control complexity by establishing new languages for describing a design, each of which emphasizes particular aspects of the design and deemphasizes others.

# Chapter 1: Building Abstractions with Procedures
## 1.1 The Elements of Programming

> Every powerful language has three mechanisms for accomplishing this:
> - *primitive expressions*, which represent the simplest entities the language is concerned with,
> - *means of combination*, by which compound elements are built from simpler ones, and
> - *means of abstraction*, by which compound elements can be named and manipulated as units.

> The contrast between function and procedure is a reflection of the general distinction between describing properties of things and describing how to do things, or, as it is sometimes referred to, the distinction between declarative knowledge and imperative knowledge. In mathematics we are usually concerned with declarative (what is) descriptions, whereas in computer science we are usually concerned with imperative (how to) descriptions.

> The importance of this decomposition strategy is not simply that one is dividing the program into parts. After all, one can also partition a program into ten or a hundred parts without organizing them as a hierarchy. The hierarchy plays an important role in controlling complexity: it imposes an organization on the program that makes it substantially easier for our minds to grasp.

> So a procedure definition should be able to suppress detail. The users of the procedure may not have written the procedure themselves, but may have obtained it from another programmer as a *black box*. A user should not need to know how the procedure is implemented in order to use it.

> The idea of block structure originated with the programming language Algol 60. It appears in most advanced programming languages and is an important tool for helping to organize the construction of large programs.

## 1.2 Procedures and the Processes They Generate

> A procedure is a pattern for the *local evolution* of a computational process. It specifies how each stage of the process is built upon the previous stage. We would like to be able to make statements about the overall, or global, behavior of a process whose local evolution has been specified by a procedure. This is very difficult to do in general, but we can at least try to describe some typical patterns of process of process evolution.

> In general, the technique of defining an *invariant quantity* that remains unchanged from step to step is a powerful way to think about the design of iterative algorithms.
