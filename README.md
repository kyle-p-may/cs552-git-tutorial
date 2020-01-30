# CS 552 git Tutorial

In this tutorial, we will describe what git is, why you should use it, and some
examples of the commands and errors that you will encounter.
This is not an exhaustive tutorial of git, as there are many features that
are intricate (and extremely useful).
It is left up to you to explore more of the features to find what you can use to
make your life easier.
This tutorial can be broken into the following parts:

1. Conceptual Overview
2. Example and Typical Workflow
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
While not needed, typically there is a remote repo that is managed by someone (e.g. GitHub)
and then there is a local repo on your machine.

![Figure of Local and Remote Repo](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/reps.png)

Each of the repos (local and remote) are complete copies of the git repo.
Therefore, if you have access to any of them, then you have a complete copy of your project.
For example, suppose that you had a local repo on your computer and your computer was stolen.
If you had a repo hosted on GitHub, then you could just pull the repo and you would have a complete
copy of your project.
Suppose that instead of your computer being stolen, GitHub's servers crashed and they lost your repo,
you would still have access to all of your code through the local repo.

While collaborating with a team, the remote repo serves as the point at which sharing occurs
and changes become somewhat official.

Within each repo, [commits](https://www.atlassian.com/git/tutorials/saving-changes/git-commit)
make up the history of the source code.
Commits are snapshots of the code which enable git to roll back changes.
In order to track the sequence of changes, each commit tracks which commit immediately preceded it.

![Commits](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/commits.png)

This implies a sort of tree structure; we will discuss this more when we introduce branching.

Typically commits are used to save a complete (can be any size) contribution to the code base.
Suppose you write a module, and you want to save your progress, as well as let your team use that code.
At this point, you would want to commit your changes and push this commit to the remote repo to allow your team to see the changes.
We will talk in more detail about this in the example.
