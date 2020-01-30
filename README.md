# CS 552 git Tutorial

In this tutorial, we will describe what git is, why you should use it, and some
examples of the commands and errors that you will encounter.
This tutorial can be broken into the following parts:

1. Conceptual Overview
2. Typical Workflow
3. Commands
4. Configuration
5. Common Errors and Solutions

## Conceptual Overview of git
git is a Version Control System
([VCS](https://www.atlassian.com/git/tutorials/what-is-version-control)).
A VCS is a tool for tracking changes to source code (or other files) and, at its core,
allows for developers to roll back to previous versions of code, particularly
when bugs are introduced.

git projects revolve around
[repositories](https://www.geeksforgeeks.org/what-is-a-git-repository/).
A repo is a collection of related files that form a project.
For example, in this class, you would use a single repo to track all of the Verilog
source code for your processor design.
Then, your team would contribute to that single repo.
