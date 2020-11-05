# Git bisect

With git bisect you can perform a binary search through your commit history in order to identify a 'bad' commit.
What a bad commit is, is up to you.

### Why is it useful?

Imagine you work on a huge coding project with multiple collaborators.
After merging in various pull requests from your colleagues the code does not compile anymore.

Who caused the bug and in which commit?

### How does it work?

Use
```bash
git bisect start
```

to begin the bisection process.

We then mark a commit as bad that we know is bad (i.e. the code does not compile):
```bash
git checkout SOME-BAD-COMMIT
git bisect bad
```

And now we mark a commit as good that we know is good (i.e. the very first commit):
```bash
git checkout SOME-GOOD-COMMIT
git bisect good
```

From now on, git will binary search the history, query us with commits and ask us to mark them good or bad:
```bash
# Wait for git
git bisect good/bad
```

As soon as git knows which is the first bad commit, it will tell you.

### exercise

Checkout the branch `practice-bisect`.

1. In `exercises/bisect` you find a `Makefile` and you recognize that `make` fails.

2. Use git bisect to identify the first commit that causes `make` to fail.


