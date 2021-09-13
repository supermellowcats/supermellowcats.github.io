---
layout: post
title: Tips for beginner programmers
categories: DataScience
comments: true
excerpt: Reflections from teaching 2 quarters of 'Computer Science with Social Science Applications' at the University of Chicago
tags: code teaching programming Python
---

## How to learn to code

As you learn to think programmatically, you're bound to run into lots of errors. Think of trying to build a car from its parts, except you're also simultaneously trying to drive it - you have to learn two different sets of skills, at two different scales. The failure may be in **building** (how you connected two things together, your understanding of one component, how you designed or wrote your code), but also in **using**  (incorrect syntax, incorrect function calls, variable name reuse/overwrite). Or it could be some messy mix of the two.


Sometimes this can be frustrating - especially when you don't understand what's going wrong. For this I would advise you to have patience - everything you struggle with now will be something that comes more intuitively to you the next time. Do try to synthesize your trials into learning rather than bruteforcing a 100 fixes until the thing stops breaking.


The learning process can also be stressful because of its complexity and the aforementioned need to develop parallel skillsets. In a course environment, I encourage you to **start all your programming assignments well in advance**, **read the problem carefully and make notes/mind maps** of the steps or specifics you might have to keep in mind, and **work in multiple disjoint sessions**. In general, make sure to **take breaks**, ask for help, and sometimes - know when to go back to the drawing board and recheck all the minutiae of the problem.


Depending on your learning style, documenting your understanding (as well as the specifics of programming assignments) with notes, diagrams, or to do lists may also be extremely helpful.


## Code problems and how to fix them

Code typically fails in only 2 ways - either it breaks (aka an error that stops execution), or it runs without protest but produces garbage results. These are also called **failing loudly** and **failing silently**. Here are some first steps that you should get into the habit of trying while fixing bugs.

1. **How to debug errors that break your code (somewhat Python-specific)**

- **Read the error traceback** starting from the top. 'Most recent call last' means that the first message corresponds to your own code.
- Identify the **type of error** (like ValueError, AttributeError, etc.). Try to develop an intuition as to what these mean.
- **Understand the possible failure modes and fix them one by one**. This is a learning process - sometimes the reason is simple, sometimes it is more nefarious. Read articles like [this](https://realpython.com/python-traceback/) one to get started.
- **Learn quickly!** Google different parts of the error message. Online forums like StackOverflow are an excellent resource. Try to understand the solution before blindly attempting it. Notice things like upvotes on a solution, read the comments, etc. As you learn to code, you will gain a lot from attempting to understand the basic troubleshooting of programming issues. Learning what parts of the error message to search is a metaskill that you will pick up along the way.
- **If you still need help from a human - ask for it comprehensively.** Anybody helping you needs to ideally reproduce, or at the very least, understand your problem to solve it. **Generally, we will need relevant parts of your traceback message as well as the code that caused the error.** Please include these in your requests for help. **You will likely get a faster response if your code is cleanly written and if you can narrow down the root cause.** Sifting through somebody else's code is difficult.

That being said, we will likely have to go through the same steps as what I mentioned above to help you debug. It is highly recommended that you **spend at least 20 minutes debugging in two separate sessions (sometimes looking at your code for too long makes it impossible to fix it)** - before reaching out for help.
 
2. **How to debug code that runs correctly but produces incorrect results**
This is a more nefarious problem with your code. It means all the parts of your code work well together, but they have been designed to do the wrong thing (e.g. you assembled a functional car but it only goes in reverse). It could involve you using incorrect logic, bad hyperparameters, forgetting to set a random seed, or many other factors.
 
Typically, it is harder to ask for external help if you're at this stage. Since the logic is your own, you will learn a lot from finding the error yourself. As the author, you will also be best equipped to do this. Some things you can try are:
- Write **clean, well-commented code** so you can easily find and recheck your logic. [Here](https://blog.alexdevero.com/6-simple-tips-writing-clean-code/) are some tips to write clean code.
- **Check your logic.**
- **Check for silly mistakes.** Are you loading the right data? Are you reusing a variable name? Are you setting the right parameters? Are you doing exactly what the requirement states? Have you conceptually understood the task your program is supposed to be doing? 
- **Use print statements at intermediate points** in your program to make sure your variables are as you expect them to be.
- Ensure you have set a **random seed** if required.


Good luck! Progamming is a powerful, versatile and elegant skill and I hope you are excited to pick it up.
