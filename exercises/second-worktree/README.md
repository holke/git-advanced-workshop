# Additional worktrees

Additional worktrees are a good way to work simultaneously on multiple branches.

### Why is this useful?

There are many situation in which you need to work on multiple branches at the same time, for example:
  - You are multitasking and working on multiple features in parallel.
  - You need to review a merge request, but are currently developing on another branch.
  - Your code takes long time to compile. So while you wait for feature A to compile, you want to checkout feature B to work on it.
  
### Why not just have a second clone?

You could just clone the repository a second time.
However, this has two main disadvantages:
1. Cloning will create a second copy of all your repo files. Especially the `.git` folder which can become large.
2. Nothing prevents you from committing to the same branch from two different folders, essentially creating a merge conflict with yourself.

Using git worktrees overcomes these disadvantages.

### How does it work?

Create a new worktree with

```bash
git worktree add PATH BRANCH
```
This will create a new worktree in the folder `PATH` and check out the branch `BRANCH`.


```bash
git worktree list
```
will print a list of all current worktrees.

If you do not need a worktree anymore, remove the folder and tell git with
```bash
git worktree prune
```

### Exercise

1. Checkout the main branch.
2. Create a second worktree that checks out a branch of your choice.
