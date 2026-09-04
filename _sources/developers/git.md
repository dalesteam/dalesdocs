(sec:git)=
# Working with git

## Checking out the code

The following example shows how to get the DALES code set up.  This
generally needs to be done only once (except the submodule steps will
also be needed when you switch to the dev branch, or rarely when the
specific versions of the submodules which DALES uses are updated.

```
# Get a local copy of the DALES source code.
# This version requires having public key authentication set up for GitHub.
git clone git@github.com:dalesteam/dales.git

# alternative without public key
# git clone https://github.com/dalesteam/dales.git

cd dales

git checkout dev

git submodule init
git submodule update
```

##  Creating a new branch and pushing it

```
# change to the directory where you installed DALES previously
cd dales

# go to the branch you want to start from, make sure you are up to date
git checkout dev
git pull

# create the new branch
git checkout -b <name-of-new-branch>

# change code, test your change

# look at the changes you have made
# good moment to clean up spelling and extra spaces
git diff

# tell git which new files you want to commit
git add file1.f90 file2.f90

# commit and push
git commit -m "short message explaining the change"
git push

# If you're on a newly created branch, git will tell you
# the command to create a new upstream branch to match your branch
# Run that command.
```

Now your code is available to others in the branch you created. They can get it with
```
git fetch
git checkout name-of-new-branch
```
If you want the new code to be merged into the dev branch, you can create a *pull request* (PR)
on GitHub. Remember to select the correct target branch in the drop-down box, probably `dev`.

Note, for the push step to work, you need to have public key authentication set up with GitHub,
and use the repository address in the form `git@github.com:dalesteam/dales`, i.e. not `https://...`.
Check with `git remote -v`.

Also, this requires push access to the DALES repository. If you don't have that,
you will get an error about permissions. You can push the new branch to your own fork instead,
by setting your fork as another remote repository and change the push command to point there.
