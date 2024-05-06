# Discussion and Reflection


The bulk of this project consists of a collection of five
questions. You are to answer these questions after spending some
amount of time looking over the code together to gather answers for
your questions. Try to seriously dig into the project together--it is
of course understood that you may not grasp every detail, but put
forth serious effort to spend several hours reading and discussing the
code, along with anything you have taken from it.

Questions will largely be graded on completion and maturity, but the
instructors do reserve the right to take off for technical
inaccuracies (i.e., if you say something factually wrong).

Each of these (six, five main followed by last) questions should take
roughly at least a paragraph or two. Try to aim for between 1-500
words per question. You may divide up the work, but each of you should
collectively read and agree to each other's answers.
[ Question 1 ] 

For this task, you will three new .irv programs. These are
`ir-virtual?` programs in a pseudo-assembly format. Try to compile the
program. Here, you should briefly explain the purpose of ir-virtual,
especially how it is different than x86: what are the pros and cons of
using ir-virtual as a representation? You can get the compiler to to
compile ir-virtual files like so: 

racket compiler.rkt -v test-programs/sum1.irv 

(Also pass in -m for Mac)

Ir-virtual’s purpose is to present the sequence of commands executed by the function before the final result is displayed. The difference between ir-virtual and x86 is that with ir-virtual’s clear indication of the utilized variable, the associated function, and the variable’s assignment, the readability is much more programmer-friendly. With x86, it’s interpreted by the hardware because it’s machine code. In terms of pros, as stated above, ir-virtual is easier for programers to understand, variables and functions are clearly indicated, and its conciseness lends itself better to following along. In terms of cons, while the absence of assembly code is easier to interpret, assembly also allows for more in-depth tracking of  programming processes. For this reason ir-virtual could be more difficult to debug.


[ Question 2 ] 

For this task, you will write three new .ifa programs. Your programs
must be correct, in the sense that they are valid. There are a set of
starter programs in the test-programs directory now. Your job is to
create three new `.ifa` programs and compile and run each of them. It
is very much possible that the compiler will be broken: part of your
exercise is identifying if you can find any possible bugs in the
compiler.

For each of your exercises, write here what the input and output was
from the compiler. Read through each of the phases, by passing in the
`-v` flag to the compiler. For at least one of the programs, explain
carefully the relevance of each of the intermediate representations.

For this question, please add your `.ifa` programs either (a) here or
(b) to the repo and write where they are in this file.

multcond.ifa: (cond (if (not bop?) (print 0)]
                 [(? lit? l) (print 270)]
                 [(+ 2 ((* 5 (+ (- 9 5) -8)))) (print (+ 347 677))]
                 [else (print #f)])

Output: If assuming that bop is true and l is not literal, then it would print out 1024. If the input is a bop, it would print 0, and if l is a literal, then it would print 270.

arith2.ifa : (print (- (* 7373 (+ 1 5432)) 350))
Output: 40057159

Explanation: The transformation process begins with an initial pass, transitioning from Scheme's if-arith->if-tiny, where variable bindings are simplified while letting larger arithmetic expressions like (* 7373 (+ 1 5432)) to remain intact. In the ANF transformation, expressions are nested within let bindings, allowing each sub-expression to be assigned to a variable for efficiency and optimization. Progressing to the virtual stage, the translation focuses on converting let bindings into register movements, if statements into standard cmp / jmp patterns, and arithmetic operations into basic assembly instructions like ADDQ and SUBQ. This stage enhances efficiency by connecting operations into low-level instructions suitable for direct execution. Finally, in the X86 form, the assembly code takes shape, formatted into a .asm file, where the original Scheme expression is deconstructed into its assembly equivalent, ready for compilation. Throughout this process, the code undergoes transformations, refining the structure and optimizing its execution at each stage, ultimately culminating in a streamlined and efficiently executable assembly representation.

bopcheck.ifa : (if (not bop? b)
              (print #f)
              (print #t))

Output: If b is not a bop, it would print out false, if b is a bop, it will print out true. 


[ Question 3 ] 

Describe each of the passes of the compiler in a slight degree of
detail, using specific examples to discuss what each pass does. The
compiler is designed in series of layers, with each higher-level IR
desugaring to a lower-level IR until ultimately arriving at x86-64
assembler. Do you think there are any redundant passes? Do you think
there could be more?

In answering this question, you must use specific examples that you
got from running the compiler and generating an output.

Typically, each pass of the compiler involves adjusting the stack pointer and performing varied assembly command sequences, which in turn shapes the expected outcome of the call. Based on what function is called, there is a higher chance of redundant passes happening. Arith0 is one that contains a lot of redundancy. A lot of the move calls appear to be redundant since they are often adjusting the stack pointers up and down the stack or allocating storage. Bool0 has much simpler assembly, with less move instructions and more executable instructions. There is definitely a possibility for more redundant passes based on the two we analyzed above.

[ Question 4 ] 

This is a larger project, compared to our previous projects. This
project uses a large combination of idioms: tail recursion, folds,
etc.. Discuss a few programming idioms that you can identify in the
project that we discussed in class this semester. There is no specific
definition of what an idiom is: think carefully about whether you see
any pattern in this code that resonates with you from earlier in the
semester.

	There are a few programming idioms that can be identified. A lot of the definitions in ifarith->ifarith-tiny are a great example, such as the ‘bop?’ built-in function and others such as the ‘symbol?’ racket defined function. Recursion is another one that can be easily identified because many pattern matches with multiple arguments match recursively and call ifarith->ifarith-tiny to assess the ‘e-body’. Which leads us to another idiom; pattern matching. There is a variety of pattern matching in the ifarith->ifarith-tiny function.

[ Question 5 ] 

In this question, you will play the role of bug finder. I would like
you to be creative, adversarial, and exploratory. Spend an hour or two
looking throughout the code and try to break it. Try to see if you can
identify a buggy program: a program that should work, but does
not. This could either be that the compiler crashes, or it could be
that it produces code which will not assemble. Last, even if the code
assembles and links, its behavior could be incorrect.

To answer this question, I want you to summarize your discussion,
experiences, and findings by adversarily breaking the compiler. If
there is something you think should work (but does not), feel free to
ask me.

Your team will receive a small bonus for being the first team to
report a unique bug (unique determined by me).

Our discussion allowed us to talk through each of the test programs, making sure we understood how they functioned. This ultimately led us to running each one and screening the output for errors/incorrectness. Through this process, we discovered a buggy program: the const.ifa test case. In let*, the variables are written as [x0 = 5] and [x1 = 4], when instead they should be [x0 4] and [x1 4].



[ High Level Reflection ] 

In roughly 100-500 words, write a summary of your findings in working
on this project: what did you learn, what did you find interesting,
what did you find challenging? As you progress in your career, it will
be increasingly important to have technical conversations about the
nuts and bolts of code, try to use this experience as a way to think
about how you would approach doing group code critique. What would you
do differently next time, what did you learn?

In working on this project, we deepened our understanding of both test cases and how the compiler functions. In terms of practical skills, we also learned how to effectively code as a group and communicate about the code. What was interesting to us was the breakdown of the assembly, and being able to understand the individual registers and steps a function takes to work properly. The part that was the most challenging or perhaps time consuming was discussing exactly how the compiler code functions. Opening up a dialogue and digging through line by line allowed us to ensure we were on the same page about how compiler.rkt was functioning, but it definitely took a bit of time. This was also the part of the project we found to be the most enriching. Next time, with our enhanced knowledge in dissecting code together, we would move more quickly from this portion of the project to breaking down things like the assembly and searching for bugs. Overall, we learned how to work effectively together, and were able to deepen our understanding and debugging skills in a number of ways. We enjoyed working on this project!


