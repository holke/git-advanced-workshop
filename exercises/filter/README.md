# git attributes and filters

Attributes and filters are a further way to automate our git process.
With filters, we can modify file contents on-the-fly before we commit them or check them out. 

### Why is it useful?

Some coding workflows require us to include code or comments that we do not want to appear in the repository.

"print-debugging" is an example. During debugging we insert lots of `print` statements in the code, but those statements are only valuable for the 
debugging programmer and are most often not wanted to be commited and end up in the remote repository.
Instead of manually removing the print statements before each commit (and needing to add them again afterwards), or tediously using `git add -p` to select the lines without debugging statement, we can simply use a filter to effectively make the print lines invisible for git.

### How does it work?

Let us write a filter `gitignore` that filters out all lines of a python file with the string '#gitignore'.
This way, we can easily mark any code line that we do not want to be committed.

First, we need a script command that removes these lines from a file:
```bash
sed "/#gitignore/d"
```

We then add this command in `.git/config` as a new filter by adding the following lines:

```bash
[filter "gitignore"]
	clean = sed "/#gitignore/d"
	smudge = cat
```

This defines a new filter 'gitignore' that applies our command to all files when they are added with `git add`.
Under the 'smudge' line we could add an extra command to apply when we check the files out.
For more complicated commands we can also put them in an extra script and specify the script in the `.git/config` file.

We now need to specify on which files the new filter should be applied.
Let us say we want to apply the filter to all '\*.py' files.
We do this in `.git/info/attributes` by adding the line
```bash
*.py filter=gitignore
```

This finishes the installation process of the filter.
If we now mark any line in a python file with the string '#gitignore' then this line will not be seen by git.
Note, that the line will not be removed from the file.


### exercise

Checkout the branch `practice-filters`.

We will work with the file `exercises/filters/helloworld.py`.

1. Edit the file and add the line
```python
print ("Debug") #gitignore
```
  after the Hello world line.
  
2. Use `git diff` and verify that you see the changes.

3. Now follow the instructions in [scripts/gitignore_filter.scp](https://github.com/holke/github-advanced-techniques-holke/blob/practice-filters/scripts/gitignore_filter.scp) to install the gitignore filter.

4. Use `git diff` again. Your changes should no longer appear in the file.

5. Play around with the filter and add some code surrounded by
```python
#gitignorebegin
# Add some code here
# This code will never show up in you git history now.
#gitignoreend
