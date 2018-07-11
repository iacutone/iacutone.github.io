---
layout: post
title: Artificial Intelligence Nano Degree
date: 2017-06-05 17:33:41 -0400
comments: true
categories: python
---

I started the [Udacity](https://www.udacity.com/course/artificial-intelligence-nanodegree--nd889) Artificial Intelligence Nano Degree last month.

### Project 1, Sudoku Solver
* [This](http://norvig.com/sudoku.html) post helpful for building a sudoku solver.
* [My Solution](https://github.com/iacutone/AIND-Sudoku)

The Sudoku Solver was a fun project. I enjoy learning by writing code to make tests pass, like Ruby Katas.

### Project 2, Isolation Game
* [This](http://inst.eecs.berkeley.edu/~cs61b/fa14/ta-materials/apps/ab_tree_practice/) application is useful for visualizing alpha beta pruning.
* [This](https://www.youtube.com/watch?v=STjW3eH0Cik) is a good introduction to the minimax function and alphabeta pruning.
* [Artificial Intelligence, A Modern Appoach](http://stpk.cs.rtu.lv/sites/all/files/stpk/materiali/mi/artificial%20intelligence%20a%20modern%20approach.pdf) chapter 5 helped clarify ideas expressed in the Udacity videos.
* [My Solution](https://github.com/iacutone/AIND-Isolation)

Creating an algo to play an Isolation Game is much more complicated than the first project and was very frustrating to build. Unlike the first project, there is barely a test framework to follow. You push your methods to a remote Udacity server which gives feedback about test failures. This development cycle is difficult to debug because you can neither use a debugger nor get `print` output. After the minimax and alphabeta functions pass the remote tests, things get interesting. You are asked to write custom heuristics in order to decide the best move for your player. I wish Udacity gave more hints as to game heuristics.

Writing a synopsis for a research paper is also required for the project. I chose to write about [AlphaGo](https://storage.googleapis.com/deepmind-media/alphago/AlphaGoNaturePaper.pdf). Due to my lack of knowledge reading math proofs, it took several attempts to understand the paper.

### Project 3, Implement a Planning Search
Using planning problems—states, actions, and goals for planning algorithms. I enjoyed this unit of the course and it was interesting to study the development of Graph Plan algorithms since the 1970's.

* [This](https://www.youtube.com/watch?v=ySN5Wnu88nE) Computerphile video is a good reinforcement for A-Star search.
* [Artificial Intelligence, A Modern Appoach](http://aima.cs.berkeley.edu/2nd-ed/newchap11.pdf) Chapter 11
* [My Solution](https://github.com/iacutone/AIND-Planning)

### Project 4, Hidden Markov Models
The objective of this unit is to create an algo that can correctly guess sign language words. Hidden Markov models are interesting and remind me of deep learning algos. They both seem to solve a similar problem set.

* [My Solution](https://github.com/iacutone/AIND-Recognizer)

### Overall Thoughts
I am not sure this course is worth the $800 price. The class was interesting, but very time consuming. The benefits to taking the Udacity course over self learning on the internet is the Slack channel, forum and project feedback. I used the forum when I was stuck on a problem. I did not take advantage of the Slack channel. The project feedback was helpful. I will not be taking the next unit in the Artifical Intelligence Nano Degree. Instead, I am progressing through the [Fast AI](http://course.fast.ai/) classes. I plan on writing a post after I finish the course.
