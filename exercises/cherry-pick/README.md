# Cherry-pick

`git cherry-pick` is a tool to apply existing commits from a different branch in the current branch.

Thus, it provides us with a way to include particular commits without merging a complete branch.

### Why is it useful?

You develop feature A on branch `feature-A`. In order to implement `feature A` you need to introduce subfeature A.1.
You add subfeature A.1 in a few commits on the `feature-A` branch.

In parallel you develop feature B on the branch `feature-B` and realize that you will need subfeature A.1 also for feature B.
Since the `feature-A` branch is not yet finished and reviewed, you do not want to merge `feature-A` into `feature-B`.

One possible solution is to use `git cherry-pick` to get all commits introducing A.1 from `feature-A` and apply them also in `feature-B`.

### How does it work?

The syntax you should remember is
```bash
git cherry-pick <commit>
```
where `<commit>` describes one or multiple commits to cherry-pick.

You can use a commits hash to reference to it. We will use the imaginary hashes `123456` and `123459` here.

Pick a single commit:
```bash
git cherry-pick 123456
```

Pick multiple commits, starting with `123456` and ending with `123459`:
```bash
git cherry-pick 123456^..123459
```

That's basically all you need to know about cherry-picking.

Of course `git cherry-pick` offers a variaty of options; see https://git-scm.com/docs/git-cherry-pick for more details.

### The exercise

The branch `practice-cherry-pick-pickfromhere` contains several commits that you need 
in the branch `practice-cherry-pick`. You'll recognize them, since they mention 'cherry-pick' in their commit message.

1. Identify the commits that you need to cherry-pick using `git log` or a tool such as [`gitk`](https://git-scm.com/docs/gitk).
2. Checkout the branch `practive-cherry-pick`
3. Use `git cherry-pick` to apply the commits. Do not change the order of the commits (pick the oldest one first).

