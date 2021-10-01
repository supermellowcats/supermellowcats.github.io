---
layout: post
title: "Two types of code failures and how to fix them"
categories: DataScience
comments: true
published: true
excerpt: Dealing with loud and soft failures ...
tags: code teaching programming Python
last_modified_at: 2021-09-23
---

## Two types of code failures and how to fix them

Code typically fails in only 2 ways - either it breaks (aka an error that stops execution), or it runs without protest but produces garbage results. These are also called **failing loudly** and **failing silently**.

Most often, beginner programmers will encounter code that fails loudly.

1. How to fix **errors that break your code**

- **Read the error traceback** starting from the top. In Python, 'Most recent call last' means that the top-most message corresponds to your own code and the bottom-most likely corresponds to some standard/imported library you might be using.
- Identify the **type of error** (like ValueError, AttributeError, etc.). Develop an intuition as to what this means. When in doubt, Google something like 'ValueError explained' and read a top blog or watch a highly rated video!
- **Understand what failure modes are occurring and fix them one by one**. This is a learning process - sometimes the reason is simple, sometimes it is more nefarious. Read articles like [this](https://realpython.com/python-traceback/) to get started.
- **Learn where to find answers - quickly!** Google different parts of the error message. Online forums like StackOverflow are an excellent resource. Github issue pages, or even more obscure internet forums can also sometimes be helpful. Learning what parts of the error message to search is a metaskill that you will pick up along the way.
- **Don't bruteforce a fix.** Try to understand the solution before blindly attempting it. Notice things like upvotes on a solution, read the comments, and try to match people's experiences to your own.
- **If you still need help from a human - ask for it comprehensively.** Anybody helping you needs to ideally reproduce, or at the very least, understand your problem to solve it. **Generally, we will need relevant parts of your traceback message as well as information about the code that caused the error.** Please include these in your requests for help. **You will get a faster response if your code is cleanly written and if you can offer guesses that help root-cause the bug**. Sifting through somebody else's code is difficult.

Remember - any human will have to go through the same steps above to help you debug. As a rule of thumb, try to **spend at least 20 minutes debugging in two separate sessions (sometimes looking at your code for too long makes it impossible to fix it)** - before reaching out for help.


2. How to fix **code that runs correctly but produces incorrect results**
This is a more nefarious problem with your code. It is also rarer. It means all the parts of your code work well together, but they have been designed to do the wrong thing (e.g. you assembled a functional car but it only goes in reverse). It could involve you using incorrect logic, bad hyperparameters, forgetting to set a random seed, or many other factors.

For example, your function `sort_list(list1)` may always be returning an empty list `[]`. Or your ML algorithm could always be predicting the `0` class in a binary classification problem.
 
It can be significantly harder get external help if you're at this stage. Since the logic is your own, you will learn a lot from finding the error yourself. As the author, you will also be best equipped to do this. Exhaustively speaking, you need to go over each line and ensure that it does exactly what you expect it to do. StackOverflow, Github issue pages, and random blogs on the internet can be extremely helpful.

This becomes tedious but necessary in a codebase of 100s or even 1000s of lines. (Hint: this is another reason why organizing code into sensible functions is extremely important. A function is the most effective unit of code to test.)
