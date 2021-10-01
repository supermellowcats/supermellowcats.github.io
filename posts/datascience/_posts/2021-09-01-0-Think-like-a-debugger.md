---
layout: post
title: "Think like a debugger: 10 lessons for fixing code faster"
categories: DataScience
comments: true
published: true
excerpt: "Reflections from teaching 2 quarters of 'Computer Science with Social Science Applications' at the University of Chicago"
tags: code teaching programming Python
---

I helped a cohort of computational social scientists learn to code in Python over two quarters at the University of Chicago. In 6 months, we went from defining our first functions and writing conditional logic, all the way to building a social science simulation from scratch. That experience led to the following advice for beginner programmers.

## Think like a debugger: how to get better at fixing code

As you learn to code, you're bound to run into lots of errors. It's like trying to build a car from its parts, except you're also simultaneously trying to drive it. You have to learn two sets of skills at the same time - but each depends on the other.

Think about it. When things break, the failure may be in how you **built** something (how you connected two things together, your understanding of some component, how you designed or wrote your code, etc.), but also perhaps in how you **used** it (incorrect syntax, incorrect function calls, variable name reuse/overwrite, etc.). If you're unlucky, it could be some messy mix of the two.

### Lesson 1: Help your brain learn better with rest and structure

Learning to code can be stressful because the need to develop complex skillsets in parallel. You are a human being, so there are human things you can do to help yourself learn this challenging but rewarding skill.

In a course environment, I encourage you to:
- **plan enough time for every programming task/project/environment**
- **read the problem statement carefully and make notes/mind maps** of the steps or specifics
- **work in multiple disjoint sessions**.
- make sure to **take breaks**
- ask for help
-  if required, go back to the drawing board and recheck all the minutiae of the problem

If you're learning at your own pace, it's additionally important to:
- **set goals for covering material**, be it a textbook, course, YouTube playlist, whatever
- **build projects from scratch** - coding along with a 'demo' is great, but you really learn by building things

Depending on your learning style, documenting your understanding (as well as the specifics of programming assignments) with notes, diagrams, or to-do lists may also be extremely helpful.

### Lesson 2: Don't aim to write bug-free code, aim to get good at fixing bugs

Everyone, even seasoned programmers, write buggy code. The mark of a seasoned programmer is not the ability to write bug-free code the first time - it is the ability to read an error message and instantly, even instinctively, know exactly how to fix it. More than anything, this comes from experience - **it will never take you hours to fix the same bug the second time**.

If your goal is "no bugs", you will be disappointed. Learning involves making mistakes, especially while programming.

### Lesson 3: Test often

If you're new to programming, you might often find yourself coding a big block (20-100 lines), then running it to find that it breaks. Your code is now likely to fail at multiple points in sequence - you fix one bug, and there's a new one right after it. Fixing each one is an iterative process of root-causing and learning through trial and error. This type of repetitive context switching can be very tiring, especially when progress on each bug is slow. I have seen students get fatigued from this process and think they're bad at debugging. They're not - it's just a lot of brainwork to get used to.

There's a simple fix to avoid having to fix 5-10 bugs in a single debugging session - **test often**.

Preferably, manage versions of your codebase using Git so you can revert to previous versions if required, but that's a whole different story.

### Lesson 4: Write **clean code** from the get-go
Clean code is an investment in the person who has to read, understand, maintain, or fix the code in the future. Unless you're being paid to code, that future person is always you. Ergo, to be kind to your future self (or to keep a job), you must write clean code. How do you do that?

Clean code is **easy to read**. Functions and variables are named consistently and smartly. Read more about good naming conventions [here](https://visualgit.readthedocs.io/en/latest/pages/naming_convention.html).

Clean code is **structured**. Code is organized logically for function, design, and readability. This is a great practice because it forces you zoom out to think about the structure of your code, not just the specific programming task at hand.

<!-- Insert link to structured code blog post?
-->

**Clean code is easier to debug.** The function is the simplest unit for a test. Being able to test/trust in units of functions helps root-cause problems faster. Smartly designed functions are easier to test/trust.

[Here](https://blog.alexdevero.com/6-simple-tips-writing-clean-code/) is some more good reading on clean code.

### Lesson 5: Every bug has a cause and effect - build a map between them

The first time you encounter certain type of bug can be incredibly frustrating - especially when you don't understand what's going wrong. As you program more, this will happen often. The first most important thing you need is **patience** - everything you struggle with now will be something that comes more intuitively to you the next time. **Do** try to synthesize your trials into learning rather than bruteforcing a 100 fixes until the thing stops breaking. Cultivate a problem root-causing approach to understanding *why* your code failed.

This is the only way to ensure that your **second** time fixing the same type of bug is quick and painless. Make your effort and time count the first time!

### Lesson 6: Become comfortable with the right level of abstraction

When you're coding professionally, programming is about never, ever, reinventing the wheel - unless there's a really good reason to. It tends to be expensive in time, money, and maintenance.

On the other hand, when you're learning, it can be very instructive to deconstruct and reinvent the wheel. For example, here's one of the simplest ways of illustrating `for` loops:
```
split_sentence = sentence.split(' ')
# splits a list of characters by the space character

sent_lower = [word.lower() for word in split_sentence]
```

It might be faster to just use `sent_lower = sentence.lower().split(' ')`, but that doesn't teach you what a `for` loop is. This works even for higher level concepts in machine learning, data visualization, or distributed programming.

Your task is to learn what **level** of abstraction you should be operating at. It isn't always necessary, feasible, or possible to know every part of every tool you need. Learn when to just copy-paste someone else's solution that you found on StackOverflow or Google. Learn how and when to tweak to get what you need.

<!--It's good to abstract away complex units of tasks into functions or classes. This has several advantages:
- you will have logically designed and finite set of abstractions to test, instead of a massive unstructured code base with infinite plausible points of failure. This avoid using 100s of temporary variables in the `global` scope).
-->

### Lesson 7: Learn to find the right resources when you're stuck
Learn how to effectively Google an error message. Learn the correct parts of the traceback to search (maybe including your path `/Users/aabir/Documents/code/project_name` *won't* help your search results be relevant?). Become familiar with StackOverFlow, Github and its Issue and PR pages. Identify Google Groups or mailing lists of communities of users for more niche libraries or languages.

If your question hasn't been answered anywhere online, check again with a different search. Then again. And only then should you ask a human being. If you do need help from a human - ask for it comprehensively. Check back for a future post on how to do this!

<!-- TODO: How to ask for help programming/debugging help blog post-->

### Lesson 8: Use print statements at intermediate points
This is truly a golden hack, one that has helped me diagnose *countless* silly errors. Use `print()` to make sure your variables are as you expect them to be. Use them liberally, as long as the results are visually understandable (maybe don't print 1000s of lines of output).

Pro tip: With data cleaning, machine learning, and deep learning, it is very helpful to print the shapes of your arrays at intermediate points, especially before and after matrix multiplications.

### Lesson 9: Write tests
How do you know that your code is failing? Are you running a code snippet that gives you an unexpected output? That code snippet is a test.

Maybe you have something like this:

```
x = toLowerCase('Hello WorLd')
# toLowerCase is a function you defined somewhere else
if x = x.lower():
    print('toLowerCase() not working correctly')
    raise ImplementationError # this line forces the code to fail here LOUDLY
```

Tweak it and wrap it in a function!

```
def test_toLowerCase():
    for word in ['Hello WoRLd', 'OK Computer']:
      lowercase_sent = toLowerCase(sent)
      assert lowercase_sent = sent.lower(), 'toLowerCase() not working correctly'
```

That's it, you've written your first test. Unit-testing is simply the practice of writing tests for the smallest unit of code (i.e. the function). You can now call `test_toLowerCase()` to immediately know whether `toLowerCase` is correctly implemented or not.


<!--If you write tests like `test_toLowerCase()` for everything in your codebase, can put them all in a `test.py` and automate testing with a tool called `pytest`.
-->

### Lesson 10: Forgive yourself for (but also learn to avoid) silly mistakes
These are innumerable and field-dependent. But there is always that same type of bug that not only gets you every time, but also makes you feel incredibly foolish.

Are you loading the wrong data? Does your loop never exit? Are you overwriting a variable? Are you feeding in the wrong default parameters? Are you missing an edge case? Are you always entering the edge case? Are you setting the wrong random seed?

Even seasoned programmers can make such mistakes. Whatever it is, learn to bear your humiliation at your own hands with grace.

Finally, if your code is hopelessly broken, there's always one last resort before you give up and move to Cuba.
**Go over the entire codebase. Double-, triple-, quatruple-check each line of code, making sure it does exactly what you expect.**

If you genuinely, truly do this and still can't fix the bug, you may just have violated the Turing completeness of your language.

Good luck! Progamming is a powerful, versatile and elegant skill and I hope you are excited to pick it up.
