# CS 552 git Tutorial

In this tutorial, we will describe what git is, why you should use it, and some
examples of the commands and errors that you will encounter.
This is not an exhaustive tutorial of git, as there are many features that
are intricate (and extremely useful).
It is left up to you to explore more of the features to find what you can use to
make your life easier.
At first, git will slow down your development; however, learning how to develop and collaborate effectively is a part of becoming a better engineer, so you should learn this now.
This tutorial can be broken into the following parts:

1. Conceptual Overview
2. Example and Typical Workflow
3. Tips and Tricks
4. Advice

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

### Generate ssh keys and add them to your GitHub account
In order to efficiently work with GitHub, you will want to generate an ssh key in order to authenticate quickly.
Follow the directions [here](https://confluence.atlassian.com/bitbucketserver/creating-ssh-keys-776639788.html) in order to generate one for your system.
After generating the key, register the public key (typically stored in ~/.ssh/id\_rsa.pub) with GitHub.
You will need to do this for each of the systems that you work on.

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

Once you have a good status, you can run `git pull origin master` in order to get the most up to date code from the remote.
In this command, you specify both the specific remote that you are working with (e.g. origin) and the branch on that remote (e.g. master).
You can have multiple remote repositories, but you will not need to for the purpose of this class.
Branches will be covered later in the tutorial.

#### Make changes
The fun part, write some code.

#### Checking status and add changes
After making changes, running `git status` will show you all of the files and directories that you changed. After making a sizable change that works correctly, it is usually a good idea to save those changes.

In order to do this, you can use a combination of the commands `git add` and `git commit`. A commit is a logical group of changes to the repo. You can add the changes that you want to be a part of a single commit by using the `git add` command. For example, make two files `foo.c` and `foo.h`.

After running `git status` in your repo, the output should look something like:

![add pt 1](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/add_1.png)

Now, running `git add foo.c` and `git add foo.h` will "stage" this files for commit.
Run `git status` again to understand what changed in the repo.
`git add` signals to git that you intend to commit the changes that you made to the files that you are adding.
When you commit, you will only save the changes that you signaled to git that you wanted to save.
This means that you can save changes and decide not save other changes yet.

Now, in order to fully commit these changes to the repo (e.g. for the changes to be tracked), you must run `git commit`.
The suggested form of the command is as follows: `git commit -m "informative commit message"`.
The `-m` option says that you want to specify the commit message (which is how commits are identified) on the command line.

A few extra tips here: when checking the status, it is common to forget what specific changes happened to each file.
You should always understand all of the changes you are adding to a commit; a command that helps is `git diff`.
`git diff` functions similarly to the Linux utility diff.
For example, suppose you make some changes to the files `foo.h` and `foo.c`, but you forgot what you did to `foo.c`.
Running the command `git diff foo.c` will provide the following output, which shows the differences of the file arguments since the last commit.

`git add` has many different options that can be viewed using `git add -h`; I would google before using any of the other complex arguments.

![Simple diff](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/simple_diff.png)

Running `git diff` will show all of your changes.

#### Checking status and pushing to remote
At this point, your repo has local commits that have not been shared with the remote repository.
Running `git status` will show some a message talking about how your branch is ahead of the remote.

![Unpushed commits](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/unpushed_commits.png)

Before saving any commits to the remote, you should always run `git pull origin master`.
This will make sure that you have the most up-to-date repo, and it will merge changes between commits in the remote and commits in your local.

In order to save your changes to the remote (and share your changes with your collaborators), run

`git push origin master`

![Successful Push](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/suc_push.png)

The above will be the output on the successful push to the remote. A common error message that you will see at this point will be a merge commit.
For example, suppose you and your partner work on the same file, and make changes that conflict.
This code be implementing the same function or changing the same lines of code in a way that git cannot automatically merge them.
Your partner pushes their changes to the remote before you do.
In this example, suppose that you both implemented a function `bar` in `foo.c`.
When performing the command `git push origin master`, you will get an error.

![Failed Push](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/failed_push.png)

In general, error messages are very descriptive, so read them, or just google them.
To solve this error, you have to pull in order to get the remote changes to your local repo.
After pulling, you will receive another error.

![merge conflict 1](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/pull_conflict.png)

From this error message, git tells you exactly what files from the commit have conflict.
As seen from the error message, there is a conflict in `foo.c`.
In order to actually commit your changes, you must go into `foo.c` and sort out the changes.
Upon opening `foo.c`, you will see the following.

![merge text](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/merge.png)

The key is that git keeps both versions of the code around, surrounding the code with `<<<..` and `>>>...` and separating the two versions with `===...`.
The local version (your most recent commit) is `<<<` -> `===` and the conflicting version would be from `===` to `>>>`.
Usually, merge conflicts are resolved by picking the version that you want to save and just deleting the other version.
To signal to git that you fixed the conflict, you follow the same directions for adding a file and commiting.
You should ***make the commit message descriptive to note what you merged***.

### Branches
Branches are a way to separate changes to your code base at a higher level than individual commits.
Commits organize small logical changes, such that other people do not see intermediate changes that show the code base in a non-stable state.
However, some changes that are not stable might require many commits before being ready to be merged into the main code base.
For example, suppose that you have an almost fully functioning pipelined processor.
Adding a cache will be a complicated process that you wouldn't want to do in 1 commit.
Therefore, you can use a branch to track all of the commits that go into implementing the cache sub-system before merging this with the main branch that is tracking your processor.
A more detailed description of branches can be found [here](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell).

#### Creating a branch

In order to use branches, you would use the following command:
`git branch <name>`
where you would replace \<name\> with an identifier associated with the branch.

Immediately after running this command, you will be on the same branch as before (most likely master).
You can check this by running `git status`, and the first line states what branch you are on.

In order to switch branches, you can run
`git checkout <name>`

For example, let's say I made a branch `kyle-dev`.

![branch](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/kyle_dev.png)

As seen, `git status` shows that I am on my new branch.
Now, you can follow the same workflow as before, but now instead of `master` for the branch, I would replace that with `kyle-dev` (e.g. `git pull origin kyle-dev`).

In order to visualize branches and commit histories in general, try the command `git log --graph`.

Furthermore, to list branches, you can run `git branch`. This is useful in order to see which branches you can switch to.

Sometimes, your local repository will not track a branch that was created in another local repo. When trying to switch to this branch using `git checkout`. To fix this, use `git fetch` to synchronize your local repo with the remote repo.

#### Merging branches
In order to merge a branch, you should checkout the branch that you want to receive the changes (most of the time `master`).
Then, you run `git merge <name>`.
An example of this is as follows:

![Branch merge](https://github.com/kyle-p-may/cs552-git-tutorial/blob/master/figures/branch_merge.png)

Again, git will try to automatically fix conflicts but you might have to resolve them similar to how we fixed the conflicting commits in the example.

## Tips and Tricks
Here we compiled some interesting features of git that you could potentially use to be more efficient.

### .gitignore
This file describes which files you do not want git to track.
For example, you could add your work directory to this so you do not accidentally track things you do not care about.
Read more about this feature [here](https://help.github.com/en/github/using-git/ignoring-files).

### Configure the default editor for git
Sometimes git will automatically open an editor so you can edit logs.
If you are not familiar with the default, this can confusing.
An example of setting the editor to vim is as follows (run in command line):
`git config --global core.editor vim`

### git aliases and bash aliases
I would recommend putting some effort into setting aliases for commonly used commands.
For example, you will run `git status` very frequently; if you can reduce the amount that you need to type, it is good.
You can do this with a combination of bash aliases and git aliases.
For example, in your `~/.bashrc` file, you could add the following line:

`alias g="git"`

After running `source ~/.bashrc` or restarting your terminal, typing the command `g` is the equivalent of typing `git`.
To run `git status`, you could now run `g status`.
You can go crazy with customization here.

For git aliases, you can do something similar.
To set an alias for the status command, you could run

`git config --global alias.st status`

Now, to run `git status`, you can run `git st`.
Combining with the bash alias, you could run `g st`.

To read more about bash aliases, go [here](https://www.digitalocean.com/community/tutorials/an-introduction-to-useful-bash-aliases-and-functions).
To read more about git aliases, go [here](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases).

### git hooks
git provides a system in which you can automatically run scripts before certain key events (e.g. committing).
These are called hooks.
An example use case would be a script that checks that all code compiles successfully before allowing a commit (because no one likes code that doesn't compile in commits).
Read more about this feature [here](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks).

### git blame
So you can figure out who ruined it all.
Read more [here](https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-blame).

### git submodule
This feature allows you to nest repositories, and is quite useful when your project depends on another project that is already version-controlled with git.
Read more [here](https://git-scm.com/book/en/v2/Git-Tools-Submodules).

## Advice
This is meant to serve as an introduction to git, and it does not even come close to describing all of its features.
git can produce some odd errors, but searching on google will usually provide a simple solution for errors.
Furthermore, you can always reach out to TAs with git questions.

