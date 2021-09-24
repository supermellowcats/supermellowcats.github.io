---
layout: post
title: Think like a debugger: how to get better faster
categories: DataScience
comments: true
excerpt: Reflections from teaching 2 quarters of 'Computer Science with Social Science Applications' at the University of Chicago
tags: code teaching programming Python
last_modified_at: 2021-09-23
---

I helped a cohort of computational social scientists learn to code in Python over two quarters at the University of Chicago. In 6 months, we went from defining our first functions and writing conditional logic, all the way to building a social science simulation from scratch. That experience led to the following advice for beginner programmers.

## Thinking like a debugger: how to get better faster

As you learn to code, you're bound to run into lots of errors. It's like trying to build a car from its parts, except you're also simultaneously trying to drive it. You have to learn two sets of skills at the same time - but each depends on the other.

Think about it. When things break, the failure may be in how you **built** something (how you connected two things together, your understanding of some component, how you designed or wrote your code, etc.), but also perhaps in how you **used** it (incorrect syntax, incorrect function calls, variable name reuse/overwrite, etc.). If you're unlucky, it could be some messy mix of the two.

If you're new to programming, you probably aren't incrementally testing your code. This means your code can fail sequentially - you fix one bug, and there's a new one right after it. This type of repetitive context switching can be very tiring, especially when progress on each bug is slow. I have sees students get fatigued from this process and think they're bad at debugging. They're not - it's just a lot of brainwork to get used to.

Let me reassure you - everyone, even seasoned programmers, write buggy code. The mark of a seasoned programmer is not the ability to write bug-free code the first time - it is the ability to read an error message and instantly, even instinctively, know exactly how to fix it. More than anything, this comes from experience - it will never take you 5 hours to fix the same bug **the second time**.

The first time for each type of bug, however, can be incredibly frustrating - especially when you don't understand what's going wrong. As you program more, this will happen often. The first most important thing you need is **patience** - everything you struggle with now will be something that comes more intuitively to you the next time. Do try to synthesize your trials into learning rather than bruteforcing a 100 fixes until the thing stops breaking. Cultivate a problem root-causing attitude to understanding *why* your code failed.

The learning process can be stressful because of its complexity and the need to develop parallel skillsets. In a course environment, I encourage you to **start all your programming assignments well in advance**, **read the problem carefully and make notes/mind maps** of the steps or specifics, and **work in multiple disjoint sessions**. In general, make sure to **take breaks**, ask for help, and sometimes - know when to go back to the drawing board and recheck all the minutiae of the problem.

Depending on your learning style, documenting your understanding (as well as the specifics of programming assignments) with notes, diagrams, or to-do lists may also be extremely helpful.

## Meta-skills you need to be a good debugger

1. **Know the right resources**
  Learn how to Google an error message. Know the correct part of the traceback to search. Become familiar with StackOverFlow, Github and its Issue and PR pages. Identify Google Groups or mailing lists of communities of users for more niche libraries or languages.
2. **Problem root-causing**
  How do you know your code is failing? Is it a code snippet? That's a test. Wrap it in a function like so:

  ```

  ```

## Two types of code problems and how to fix them

Code typically fails in only 2 ways - either it breaks (aka an error that stops execution), or it runs without protest but produces garbage results. These are also called **failing loudly** and **failing silently**.

Most often, beginner programmers will encounter code that fails loudly.

1. How to debug **errors that break your code**

- **Read the error traceback** starting from the top. 'Most recent call last' means that the first message corresponds to your own code.
- Identify the **type of error** (like ValueError, AttributeError, etc.). Try to develop an intuition as to what these mean.
- **Understand the possible failure modes and fix them one by one**. This is a learning process - sometimes the reason is simple, sometimes it is more nefarious. Read articles like [this](https://realpython.com/python-traceback/) one to get started.
- **Learn quickly!** Google different parts of the error message. Online forums like StackOverflow are an excellent resource. Try to understand the solution before blindly attempting it. Notice things like upvotes on a solution, read the comments, etc. As you learn to code, you will gain a lot from attempting to understand the basic troubleshooting of programming issues. Learning what parts of the error message to search is a metaskill that you will pick up along the way.
- **If you still need help from a human - ask for it comprehensively.** Anybody helping you needs to ideally reproduce, or at the very least, understand your problem to solve it. **Generally, we will need relevant parts of your traceback message as well as information about the code that caused the error.** Please include these in your requests for help. **You will get a faster response if your code is cleanly written and if you can offer guesses that help root-cause the bug**. Sifting through somebody else's code is difficult.

Remember - any human will have to go through the same steps above to help you debug. As a rule of thumb, try to **spend at least 20 minutes debugging in two separate sessions (sometimes looking at your code for too long makes it impossible to fix it)** - before reaching out for help.


2. How to debug **code that runs correctly but produces incorrect results**
This is a more nefarious problem with your code. It is also rarer. It means all the parts of your code work well together, but they have been designed to do the wrong thing (e.g. you assembled a functional car but it only goes in reverse). It could involve you using incorrect logic, bad hyperparameters, forgetting to set a random seed, or many other factors.

For example, your function `sort_list(list1)` may always be returning an empty list `[]`. Or your ML algorithm could always be predicting the `0` class in a binary classification problem.
 
It can be significantly harder get external help if you're at this stage. Since the logic is your own, you will learn a lot from finding the error yourself. As the author, you will also be best equipped to do this. Exhaustively speaking, you need to go over each line and ensure that it does exactly what you expect it to do. StackOverflow, Github issue pages, and random blogs on the internet can be extremely helpful.

This becomes tedious but necessary in a codebase of 100s or even 1000s of lines. (Hint: this is another reason why organizing code into sensible functions is extremely important. A function is the most effective unit of code to test.)

Some steps worth trying are:
- Write **clean, well-commented code** from the get-go. This makes it easier to find and recheck bits of logic. [Here](https://blog.alexdevero.com/6-simple-tips-writing-clean-code/) are some tips to write clean code.
- **Double-, triple-, quatruple-check your logic.** If you have a flow-chart or pseudocode, check that, then check that your code translates it correctly.
- **Check for silly mistakes.** Are you loading the right data? Are you reusing a variable name? Are you setting the right parameters? Are you doing exactly what the requirement states? Have you conceptually understood the task your program is supposed to be doing? 
- **Use print statements at intermediate points** in your program to make sure your variables are as you expect them to be. With ML and DL, it is helpful to print the shapes of your arrays.
- Ensure you have set the correct **random seed** if required.

Good luck! Progamming is a powerful, versatile and elegant skill and I hope you are excited to pick it up.
