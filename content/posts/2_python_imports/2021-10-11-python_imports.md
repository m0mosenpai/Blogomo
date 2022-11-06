---
author: "Sarthak Khattar"
title: "Python packages, modules and import statements"
date: "2021-10-11"
description: "\"ModuleNotFoundError: No module named 'Monjiro' \""
tags: ["Python"]
---

A few weeks back, I interviewed for [Deepsource](https://deepsource.io/) as a Python Developer and they asked me this problem that piqued my interest a little. I felt like it was simple problem - one which seemed to have an obvious solution, yet something that might frustrate a lot of people just starting out with Python.

Let's go over the problem first. You can follow along by cloning the repo from [here](https://github.com/srijan-deepsource/import-me).

![tree](/Blogomo/static/2_python_imports/tree.png)

> _In the following directory structure, file `test_import.py` is attempting to import a function `import_me` from `import_me/utils/package`. Upon running `python test_import.py`, you get the an error. Why does this happen and how will you solve it?_

Let's run the file first and see what error we get exactly:
![tree](/Blogomo/static/2_python_imports/error.png)

Hmmm...

_"Oh, I know! There was this one time I found this StackOverflow answer that suggested me to use `python -m` instead of `python` and do this weird dot notation thing with the files. Well idk what any of this means, but it should work right?"_
![voila](/Blogomo/static/2_python_imports/voila.png)

Hmmmmmmmm...  
So what just happened?

My first thoughts after seeing the error were that it most likely had something to do with `PYTHONPATH`. That's the environment variable Python uses to look for modules and packages. The path stored in that variable is considered the **root** and everything below it in the directory structure is accessible. Everything above it is not.

Even if my hunch was correct, I needed a way to verify this. The interviewer suggested me to use the `sys` module and print out the contents of `PYTHONPATH` in both the cases.

Importing `sys` and adding a print statement for `sys.path`,  we can notice something interesting between the two cases:
![sys](/Blogomo/static/2_python_imports/sys.png)

Well, my intuition was right.  

You can execute Python files in 2 ways: either as scripts (`python script.py`) or as modules (`python -m package.module.func`).
Scripts are generally meant to be run and **do something** whereas modules are created for the purpose of **importing them into other files** and using them as libraries.  

In the first case, Python only knows about the directory the script ran from. This means anything above that directly is inaccessible. Whereas in the second case, since Python traversed recursively through packages, all the way from the root directory to find the module we wanted it to run, it's aware of everything that lies below the root directory, and not just the directory the script ran from. This explains the `ModuleNotFound` perfectly.

So how do we fix this? We could simply run it as a module and call it a day.  
But what if we _really_ want to run the file as a script? Well then, how about adding the path of the root directory to `PYTHONPATH`? This should allow Python to see module despite the file being run a script. Let's try it out:
![import_200](/Blogomo/static/2_python_imports/import_200.png)

And there we go! We can see that our newly exported path indeed shows up in the environment and now Python is able to find the function easily.

I'm not sure situations would arise where one would have to do this in a professional setting. But I do believe it's a good way to explain the fundamental concept behind Python modules, packages and it's import system. A neat explanation of these terms can be found over at [Real Python](https://realpython.com/lessons/scripts-modules-packages-and-libraries/), a website I **highly** recommend to everyone for learning Python.

Till next time.
![tnt](/Blogomo/static/2_python_imports/tillnexttime.gif)