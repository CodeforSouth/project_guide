The following was lifted completely from [Rspec Wiki - Topic Branches](https://github.com/dchelimsky/rspec/wiki/Topic-Branches) then converted into Markdown. There might be some specifics that don't apply.

# Topic Branch

by Wincent Colaiuta

### Using topic branches when contributing patches

A "topic" branch is a separate branch that you use when working on a single "topic" (a bug fix, a new feature, or an experimental idea). Working on a topic branch instead of directly on top of "master" is recommended because:

* it allows you to work on multiple, separate topics simultaneously without having them all jumbled together in a single branch
* topic branches are easily updated, which means that if the remote "master" evolves while you are working on your topic it is easy to incorporate those changes into your local topic branch before submitting (which in turn will make your topic apply cleanly on top of the current "master")
* if you receive feedback on your patches, having all the related changes grouped together in a topic makes it easy to tweak them and resubmit
* working on a topic branch makes it easy to refactor a patch series into a logical, clear, clean sequence, which in turn makes your contribution easier to review and more likely to be included

So, for all of these reasons it's recommended to use a topic branch for preparing submissions even for simple contributions like single-commit bugfixes and the like.

### Basic topic branch workflow

Let's assume you are working in a clone of the official repository. If  
you set this up using a standard `git clone` then your local "master"  
branch will be set up to track the remote "master" branch automatically.

```bash
 # make sure that you're on your local "master" branch
 git checkout master

 # integrate latest changes from upstream
 git pull

 # create and switch to new topic branch named "my_topic"
 # (obviously you'd pick a more descriptive name)
 git checkout -b my_topic

 # Work on your submission, committing along the way
 # when ready, make a patch series for attachment to a lighthouse ticket
 git format-patch master
```

That last command will create a numbered series of patch files, one for each commit on your topic branch (ie. all the commits since you branched off from "master").

### Advanced topic branch workflow

If your topic is long-lived then you can use `git rebase` to keep it  
up to date and massage it into shape. Imagine that the remote master  
has three commits at the time you start working:

```
A--B--C
```

You make a topic branch with three new commits:

```
A--B--C
       \
        X--Y--Z
```

Meanwhile, the remote "master" continues to evolve and has three  
commits of its own:

```
A--B--C--D--E--F
       \
        X--Y--Z
```

Without `git rebase`, the only way to get your changes into the  
"master" branch is for the integrator to perform a merge, creating a  
new merge commit, M:

```
A--B--C--D--E--F--M
       \         /
        X--Y--Z-
```

With `git rebase` your changes can be "rebased" on top of the current  
remote "master". Git actually removes your three commits (X, Y and Z),  
"fast forwards" your topic branch to incorporate the latest commits  
from upstream (D, E and F) and then replays your commits on top of the  
new HEAD.

```
 A--B--C--D--E--F
                 \
                  X'--Y'--Z'
```

In this way when you submit your patches they can be applied using a  
simple "fast forward" merge which yields a nice, linear history:

```
  A--B--C--D--E--F--X'--Y'--Z'
```

The content of the commits is the same but I've used the X', Y', Z'  
notation to indicate that the SHA-1 hashes for them will be different  
(because any change in their ancestry will bubble up and change their  
identifying hashes).

There is no merge, so your patches are guaranteed to apply without  
merge conflicts. If there are any merge conflicts you will have  
already resolved them when you ran "git rebase". (This appropriately  
shifts the burden of resolving merge conflicts away from the central  
integrator onto the contributor; in this way the project can scale to  
have many contributors without the integrator becoming a bottleneck.)

To make use of all this, all you have to do is:

```
 # integrate the latest upstream changes into your "master"
 git checkout master
 git pull

 # make sure that you're on your topic branch
 git checkout my_topic

 # do the rebase
 git rebase master
```

`git rebase` can do more than just keep your topic branch up-to-date.  
Given our example commits X, Y and Z you might want clean up the  
history to make it easier to review (because the easier to review, the  
more likely it is to be accepted, you'll receive better feedback, and  
bugs are more likely to be caught). By running `git rebase -- interactive` you can:

* remove a commit from a series (you realize that commit "Y" doesn't really belong in the series)
* amend a commit (you realize that the commit message for "X" isn't quite right)
* insert a commit into the series (you realize that you actually need a "W" commit before "X")
* "squash" several commits into one (you realize that "X" and "Y" just 
make the same change in two different files, and so they logically  
belong in a single commit)
* re-order commits (you realize that the series will be easier to  
understand if "Z" comes before "X" and "Y")

When you run "git rebase --interactive" you'll see an editor window  
that allows you to perform all of the above operations in a simple  
fashion; you are presented with a list of commits which you can  
transform as follows:

- delete a line to drop that commit
- use "pick" (the default) for commits which you want to appear in the  
rebased history
- change "pick" to "edit" for each commit you wish to amend (or  
perform additional commits) along the way
- change "pick" to "squash" for each commit which you want melded into  
the previous commit
- reorder the lines to reorder the commits

The rebase will stop automatically for all commits you've marked as  
"edit", and will provide you with an opportunity to edit the commit  
message for commits melded together with "squash". It will also stop  
if there are any merge conflicts to resolve. When you are ready to  
resume the rebase you just do:

```
 git rebase --continue
```
