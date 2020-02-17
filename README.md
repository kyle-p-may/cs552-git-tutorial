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

## Example and Typical Workflow

### Create a single remote repo
First, we will walk through how to create a remote repo on GitHub, using their web interface.
As seen in the following figure, there is a button to create a new repo in the top right corner of the website.

![Create Repo](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/github_create_repo.png)

After starting to create a new repo, the system will prompt you for more information describing the repo. The key parts of this are 1) repository name and 2) whether it is public or private.
**Anytime you use a GitHub repo for a class project it should be private!!**
This is very important, as making your code public is **academic misconduct**.
The following figure shows an example repo setup.

![Repo Naming](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/github_name_repo.png)

After naming your repo and setting the privacy, click the *Create repository* button.

This step will be performed once per team per project.

### Cloning the remote repository
Now, in order to gain access to your repository, you must clone your repository. This will be done everytime you want to start working on your project on a new computer.

When looking at a repository from the web interface, there is typically a place where you can select whether or not you would like to use `ssh` or `https` to authenticate, and it will give you the required address for you to clone the repo, as seen in the following figure.

![Cloning Addr](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/github_cloning_info.png)

After finding this, go to the command line, navigate to a directory where you would like
the repository to be cloned to, and run the following command.

    git clone <address>

where you would replace \<address\> with the address you found previously. For example, I would run `git clone git@github.com:kyle-p-may/cs552-example.git` in order to clone my project on to my computer. After running this command, this should occur.

![Clone Empty](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/git_clone_empty.png)

A couple things to note from this: 1) this is for `ssh`, but the output will be different if you use `https` and 2) it is okay that you have the empty repo warning, and this will disappear when you add files to the repo.

### Typical Workflow
Now, we will describe a typical workflow while using git.

1. `git status` and `git pull`
2. `git status` and `git add`
3. `git status` and `git commit`
4. `git status` and `git push`

#### Checking status and pulling
When you first start working, you always want to check the current state of your repository and then pull any changes from the remote. This step allows you to get the most up-to-date version of code, and before doing that, it is important to check if you have left any uncommitted changes in your repository. If you have uncommitted changes, the pulling action will result in errors if there are conflicts.

![status clean](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/git_status_clean.png)

This should be the output of running `git status`. If there are uncommitted changes, like

![status dirty](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/git_status_clean.png)

then you should proceed to the section about adding and committing changes before continuing.

Once you have a good status, you can run `git pull origin master` in order to get the most up to date code from the remote. In this command, you specify both the specific remote that you are working with (e.g. origin) and the branch on that remote (e.g. master). You can have multiple remote repositories, but you will not need to for the purpose of this class. Branches are a way to manage collaboration, but will not be covered by this tutorial.

#### Make changes
The fun part, write some code.

#### Checking status and add changes
After making changes, running `git status` will show you all of the files and directories that you changed. After making a sizable change that works correctly, it is usually a good idea to save those changes.

In order to do this, you can use a combination of the commands `git add` and `git commit`. A commit is a logical group of changes to the repo. You can add the changes that you want to be a part of a single commit by using the `git add` command. For example, make two files `foo.c` and `foo.h`.

After running `git status` in your repo, the output should look something like:

![add pt 1](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/add_1.png)

Now, running `git add foo.c` and `git add foo.h` will "stage" this files for commit. Run `git status` again to understand what changed in the repo.

Now, in order to fully commit these changes to the repo (e.g. for the changes to be tracked), you must run `git commit`. The suggested form of the command is as follows: `git commit -m "informative commit message"`. The `-m` option says that you want to specify the commit message (which is how commits are identified) on the command line.

#### Checking status and pushing to remote
At this point, your repo has local commits that have not been shared with the remote repository.

In order to save your changes to the remote (and share your changes with your collaborators), run

`git push origin master`

## Advice
This is meant to serve as an introduction to git, and it does not even come close to describing all of its features. git can produce some odd errors, but searching on google will usually provide a simple solution for errors. Furthermore, you can always reach out to TAs with git questions.

Most errors are produced because of the synchronization between remote and local repos. When commits conflict, you must manually sort out the differences. Again, searching for how to deal with merge cnflicts would be fruitful.

