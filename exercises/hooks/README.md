# Hooks

A hook in git is a user written script that is executed each time that are certain git action is triggered.
The script can additionally decide whether or not the git action should be aborted.

All hooks are stored in the `.git/hooks` subfolder of each git repository and must be named according to gits naming convention.

Each git repository comes with example hooks that are named `*.sample`. Removing the `.sample` part turns them in an active hook.

In this tutorial we will look further into the `pre-commit` hook that is executed before each commit.
During execution the pre-commit hook script can gain access to the files and changes that are staged for commit.

### Why is it useful?

Hooks are an excellent way to automate your git process and to help with quality control of your committed files.

The pre-commit hook, for example, is the perfect way to ensure coding conventions:
Let us imagine that we are working in a coding project that requires certain code indentation conventions.
If we have a script that checks whether a file fulfills the convention, we can write a pre-commit hook that checks for each file that
is about to be committed whether or not the indentation conventions are fulfilled.
If they are not fulfilled, the commit is aborted.
This process forces a user to indent their code before committing, saving us valuable time otherwise spend with code review.

### How does it work?

The `pre-commit` hook is executed after you type `git commit` but before a commit message is generated.
It can be any kind of script that you want to execute. If its return value is 0 the commit continues. If it is non-zero the commit is aborted.

You can use any git command to get and modified the current changes marked for commit.
For example, to loop over all files currently marked for commit you can use the line

```for file in `git diff --cached --name-only` ```

### The exercise

Switch to the branch `practice-hooks`. 
In this branch you will find the file [CODING_CONVENTIONS](https://github.com/holke/git-advanced-workshop/blob/practice-hooks/CODING_CONVENTIONS).
The coding conventions specify that each `.c, .cxx, .h` and `.hxx` file in this repository must contain information about its author.
This information must be present in the form `### Author: [NAME]` somewhere in the file.

The script [scripts/check_code_for_author.scp](https://github.com/holke/git-advanced-workshop/blob/practice-hooks/scripts/check_code_for_author.scp)
can check a file for the precense of this author information and returns 0 if the information is prensent.

The file [scrips/pre-commit](https://github.com/holke/git-advanced-workshop/blob/practice-hooks/scripts/pre-commit) is a prepared commit hook
that performs the check for each file to be committed.

#### Part A - apply the hook

1. Copy the pre-commit script to the folder `.git/hooks`.
2. Delete the `Author` line from the file `exercises/hooks/helloworld.c`
3. Add your changes and commit them.
4. Observe

#### Part B - your own hook

Write you own pre-commit hook that let's you commit files only if the code in `exercises/hooks` compiles successfully.
That is, if `make` produces no errors.
