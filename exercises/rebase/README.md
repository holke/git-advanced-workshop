# Interactive rebasing

WARNING: Use interactive rebasing with care. You will rewrite your git history. 
If you force push rewritten history into branches that other people work on, you will make enemies for life.
Only use interactive rebasing if you are certain that you are the only person working on that branch.

Interactive rebasing with `git rebase -i` allows you to go back in your git history and change commits, delete them, squash them together, even add them.

### Why is it useful?

First of all, it is as important to maintain a clean git history as it is to maintain clean code.
To keep your history clean, you can use `git rebase -i` to correct spelling errors in commit messages, split up commits that should have been seperated from the start, and much more.

A case that often needs `git rebase -i` is when you realize that you commited files that you should not have committed. 
In the worst case, internal secret documents that should not be uploaded to the repository.
Of course, you can just delete the file and add the deletion commit on top. But this way the files will still end up in your history and in the remote repository.
Thus, an attacker can checkout the old commits and see all your secrets.

With `git rebase -i` you can clean them completely so that they will not appear anywhere in you git repo.

### How does it work?

All you should need is
```bash
git rebase -i HEAD~N
```

where `N` is a positive number.

This command will open a dialog that shows you the last `N` commits on the current branch and provides you with options on how to modify them.

Here is a picture of the dialog for `git rebase -i HEAD~3` for the current state of this repositories `main` branch.

![](https://github.com/holke/github-advanced-techniques-holke/blob/main/exercises/rebase/images/rebase3.png "git rebase -i HEAD~3")


The github provided help message already tells you about your options.
Basically, you change the `pick` line of a commit (or multiple commits) to an action that you want to do with this commit.
As soon as you close the dialog window, git will continue commit by commit and let's you apply the changes that you wanted.

We will discuss the most important options that you have in detail:

#### Change the order of commits

Just reorder the lines to change the order in which the commits are applied.

#### Delete a commit

Simply remove the complete line of a commit in this dialog in order to erase it from you history.
You can also change `pick` to `drop`.

#### Reword a commit's message

Change `pick` to `reword` to edit the commit's commit message.

#### Edit a commit

Change `pick` to `edit` and you can modify this commit at will.
This is the most powerful feature of `git rebase -i`.

Your workflow after the dialog closes and when it is this commit's turn to be edited should be:
1. `git reset --soft HEAD~` to reset the HEAD in the state right after you added everything to the commit.
2. Modify your files at will to match the state that you want to commit.
3. `git add` all new changes.
4. `git commit -c ORIG_HEAD` to commit the new changes, edit the old commit message and continue with `git rebase`.

#### squash

Change `pick` to `squash`. This will use this commit but meld it together with the previous commit.
The commit message of the new commit will consist of both the commit message of the previous commit and the commit message of the current commit.

#### fixup

Change `pick` to `fixup`. Like squash, but the commit message of this commit will be forgotten.
I often use this for commits that only correct a typo.



### Merge conflicts

When rebasing merge conflicts can occur. Especially when you change the order of commits or edit commits.
Git tries to apply each change from the dialog in order and stops whenever a conflict appears.
If git find merge conflicts, it will tell you and you must resolve them by hand:
1. Find the conflicts, git marks them with the usual `<<<<<<<`, `>>>>>>>`  and `=======`.
2. Resolve the conflicts by editing the files.
3. Use `git add` to add the conflicting files.
4. Use `git rebase --continue` to tell git that the conflicts are resolved and the rebasing should continue.

Note: Sometimes the same merge conflicts will appear multiple times and you will have to perform the same edits each time.

### Pushing changes

If you rebase changes that you already pushed, git will not allow you to simply `git push` rebased changes.
Git notices that you rewrote you history and will not let you modify the history of the remote branch.

If (and only if!) you are certain that you can safely modify the history on the remote branch, use
```bash
git push -f 
```
to force-push your changes to the repository.

### The exercises

For the exercises, switch to the branch `practice-rebasing`.

#### Part A - warm-up

Use `gitk` or `git log` or a similar tool to look at this branch's history.

Oh no, you committed the document `secret_document2.txt` in commit `fd7f8fc9eda..`.
Also, you realize that commit `18599ed7..` has a spelling error in its commit message, the message says 'Add hellowold fILE' instead of 'Add helloworld file'.

Use `git rebase -i` to delete commit `fd7f8fc9eda..` and edit the commit message of commit `18599ed7..`.

#### Part B

After successfully doing Part A, you realize another terrible mistake:
In commit `df7d2aa04..` you accidentally committed `secret_document1.txt`.
However, this commit also contains contributions to some code that you definitely do not want to delete.

Use `git rebase -i` to edit the commit, such that the changes to the code stay, but the secret document is not committed anymore.
