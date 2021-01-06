# Preface

### What is this book about?

This book is a result of designing and teaching a new module, CS2030 Programming Methodology II, at the National University of Singapore (NUS), starting in 2017.

CS2030 is designed for students who have gone through a typical basic programming module module and have learned about problem solving with simple programming constructs such as loops, conditions, and functions.  In a typical introductory programming module, such as CS1010 and its variants at NUS, students tend to write small program (in the order of tens or hundreds of lines of code) to solve a programming homework, work alone on their code, and move on to solve the next problem once the homework is done.

The first aim of CS2030 is to change the students' mindset and to make them learn to write software that will continue to evolve as software requirement changes, and to write software that will be read and modified by other programmers (including their future selves).

The second aim of CS2030 is to level-up the complexity of programs that the students write, from order of hundreds of lines to thousands of lines.  CS2030 will bridge the
students between writing toy programs to solve specific problem in CS1010 and writing larger real-world software in their later modules, such as CS2103 Software Engineering.

A programming language is the medium in which programmers can express their intention and construct software, and thus is critical to supporting the aims above.  With the appropriate features and tools, once can tame the complexity of software, make the code written friendlier to other programmers, and easier to evolve.  The third aim of CS2030 is thus to expand the students' mind on different ways one can construct software and the principles behind some of the programming langauge constructs.  In particular, CS2030 focuses on objects, types, and functions, as three key constructs towards building programmer-friendly software.  It covers both object-oriented paradigm and functional paradigm as two different approaches to construct software, with a strong emphasis on type safety.

The final aim of CS2030 is to introduce students to programming language concepts, to bridge them from introductory programming to advanced modules such as programming language design and implementations.  Part of CS2030 introduces students to the design decisions behind some of the constraints and the workings behind the programming language compilation and execution, giving them a glimpse inside the programming system that so far has been mostly treated as a black box in introductory modules.

This book serves as a guide the students in CS2030 and other students who might be interested in learning to build better, more complex, programmer-friendly software, having learned introductory programming.

### The Choice of Java

We decided to use one programming language throughout the module and throughout this book.  This decision means that we need to pick a language with static typing and strongly typed, and support both object-oriented and functional of programming.  Considering multiple factors, we decided to choose Java for CS2030, for its popularity, syntax familiarity, and smoother transitions to later modules in the NUS computing curriculum.

While Java is definitely not the most elegant programming language when expressing programs in functional style, we hope that students and readers can still learn the principles of functional programming and apply it in other programming languages.  This choice is a trade off between having to switch to a different language in the middle of a module.  

### What This Book Is Not About

This is not a book on Java programming.  We will not comprehensively cover Java syntax 
and features, except those relevant to the concepts we wish to teach.  In fact, we will avoid and even ban students from using certain Java features (such as `var`) for pedagogical purposes. 

This is not a book on software engineering either.  Software engineering is a broad discipline on its own and deserves another book.  Further, there are already many well-written books on software engineering out there.  Rather, this book is about the programming principles and constructs on top of which programmers can design better software.  To motivate the importance of these principles and constructs and see how they can be used, we will inevitably cover some of the software engineering design principles, such as the Liskov's Substitution Principle (the L in SOLID), Tell-Don't-Ask, Composition over Inheritence, etc.  But we will not comprehensively cover object-oriented design or software design in general (e.g., we will not cover S,O,I,D in SOLID).
