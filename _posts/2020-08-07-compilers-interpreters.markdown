---
layout: post
title:  "Compilers and Interpreters"
date:   2020-08-10 09:30:57 +0100
categories: compilers introduction
---
Over the past few years I have become more and more interested in compilers. What I find exciting about them
is that they are the boundary between software and hardware. It doesn't matter what kind of programming language
you are working with and it doesn't matter what environment you are working in, at somepoint there is a 
compiler translating your code (somehow!?) into a format that can be run on a computer. I think this is fascinating.

I was explaining how compilers work to someone recently and thought I would write down my explanation 
so that others can learn from it as well. This is a pragmatic, high-level overview of how compilers work. It's aimed at
someone with little to no knowledge of compilers. It's the introduction I wish I had read
when I first started. I really enjoy helping others learn, so hopefully these notes are useful to you as well.

## What is a compiler?

A compiler is a computer program that translates some code into another format that can be executed. That format
can be at a lower-level of [abstraction](https://en.wikipedia.org/wiki/High-level_programming_language) (e.g. translating C++ code to machine code (1's and '0s))
or at the same level of abstraction, e.g. translating TypeScript into JavaScript (the fancy word for this second type
is 'transpilation').

## How is that different to an interpreter?

These two concepts get confused quite. They are both computer programs that are related to executing code.

A **compiler** translates some code into another format but doesn't execute the 'compiled' code itself - it lets another program do that.
E.g. C++ is typically compiled into machine code and then part of the operating system of the machine actually executes that code.
An **interpreter** does some translating and then actually executes that code as well. 

There are hybrids like **JIT-compilers** which do a bit of both. They usually start interpreting code, realise when there
is a chance for optimisation, compile the code in the background ('on the fly') while they are still interpreting
and then swap out un-optimised code for some (more optimised) machine code. This is what the [V8 engine](https://softwareengineering.stackexchange.com/questions/291230/how-does-chrome-v8-work-and-why-was-javascript-not-jit-compiled-in-the-first-pl)
 inside of Chrome does.

## Stages of a compiler/interpreter

There are different ways of compiling and interpreting but both techniques share similar pipelines.

There are 3 main stages in the work of a compiler or an interpreter:  

### Front-end

Both compilers and interpreters do this phase. This phase is all about reading the raw source
code file, working out the instructions in the source code based on whatever language the code was written in
and transforming those instructions into **abstract syntax trees (ASTs)** which are like instructions that 
are neutral from any programming language, but still very much 'human readable'.
For example, the essence of an AST produced from a JavaScript program or a C++ program should look pretty much the same.

In a bit more detail:
* Lexical analysis: A part of the compiler/interpreter called the scanner (aka 'lexer') reads in the source code and
 uses a set of rules (fancy term: a 'lexical grammar') to
work out the meaning of all of the individual characters. It then produces 'tokens'. 
So a scanner would see: 'var a = 3' and produce tokens for these characters: 'var', 'a', '=', '3'. Tokens are objects that contain the actual character (the 'lexeme') and some 
other meta information, for example the line number where the character was located.
* Syntactic Analysis: This is where the parser, another part of the compiler/interpreter, uses the tokens to work out what the users intent was in
the source code. It reads each token and uses a set of rules (fancy term: 'syntactic grammar') to generate a 'abstract syntax trees' (ASTs).
* Static analysis: Different languages and compilers/interpreters do things differently here. It's the point where you do may do some work to improve the ASTs.
For example, if your language was statically typed they you would do type-checking here. Another example is 'variable resolution' - in the ASTs you replace 
variables names like 'a' and 'potato' with the actual values they represent so they can be executed later. This way each tree is separate and can be executed
in isolation.

An interpreter could stop at this point and execute the ASTs (this is what a Tree Walk interpreter does).

### Middle end
This middle ground part is like an interface between the front-end and the back-end parts of a compiler. 

This part could be skipped in simpler compilers/interpreters, but in other compilers it could be used in two main ways. The first is to create an
intermediate representation ('IR'). This is like a set of richer ASTs. For instance, you may want to do 
some optimisations on your code.

A big benefit of having this step in the pipeline is that you can have several 
programming languages that can be translated to the same IR (with their own scanner and parser). This is part of what [LLVM](https://llvm.org/) offers.
The advantage you have of targeting
an IR like LLVM is you can use their back-end parts and create your own language more quickly.

If an interpreter didn't execute code at the end of the 'front-end' part then it would do so now. That's because after this point,
the focus shifts to producing an output for another system/program to execute.

### Backend

This part of the pipeline is only used by compilers. It's is all about translating a bunch of tree data structures from the previous step into a format
that can be executed. This part is focussed on the 'executor' of the code, in contrast to the front-end which was focussed on the source language.

There are 3 different routes that could be taken here:

#### 1 - Transpilation

This a fancy word that describes translating the ASTs into a format that is at the same 'level' as the original language the source code was written in.
A common example is transpiling TypeScript to JavaScript. The outputted JavaScript code and can be executed using a JavaScript engine elsewhere.

#### 2 - Creating bytecode for a virtual machine (VM)

[Bytecode](https://fhinkel.rocks/2017/08/16/Understanding-V8-s-Bytecode/) is kind of halfway between human source code and the assembley code that is used to create the machine code. It's sort of human readable, but not easily.
The bytecode can then be executed on a VM. A VM is like a simulated computer chip. It's not a physical chip but
it's actually a program that does some clever stuff (kind of like with its
own compiler internally) to run your code on whatever platform the VM is actually running on. The big advantage of generating bytecode to target a VM with your compiler
is that you can have code run on different platforms or systems more easily. The VM takes care of making the bytecode run on Windows or Mac etc. This is the 
approach used by the V8 JavaScript engine inside of Chrome and Node.

#### 3 - Machine code

Your compiler can generate code that is designed to be run directly on a machine. This 
usually involves generating assembly language and then using that to generate the 1's and 0's ('machine code') that can be executed.
Deciding how your code translates into the raw machine code means you have more control over how the source code will be executed by the
actual computer chip, so there are opportunities for you to get maximum performance, but at a cost of having extra work
and being able to share less code for other platforms (different platforms have different ways of working with assembley language
and therefore machine code).

#### How to choose?
At somepoint in the all three of these approaches a computer chip is going to execute 1's and 0's.
The decision you have to make when creating a compiler is how far down the chain you want to go yourself.
The tradeoff is between sharing code between different platforms (which reduces your workload and maintenance etc.)
and control over how your code is executed on the machine (which has performance implications). 

## Summary
This was an introduction to what a compilers and interpreters do, what the difference is between them
and some of the tradeoffs that have to be made when creating a compiler or an interpreter. Hopefully you found this useful
and interesting. Let me know what you think!

For further reading I highly recommend [Crafting Interpreters](https://craftinginterpreters.com/)
(a free online book).