---
layout: page
title: Version control with Git  
subtitle: Looking at history and differences
---

> ## Learning objectives {.objectives}
> * Be able to view history of changes to a repository
> * Be able to view differences between commits
> * Understand how and when to use tags to label commits

### Looking at differences

Let us suppose we've made a change to our file and not yet committed it. We can
see the changes that we've made:

~~~{.bash}
$ git diff journal.txt
~~~

This shows the difference between the latest copy in the repository and the
changes we've made. 

* `-` means a line was deleted.  
* `+` means a line was added.  
* Note that a line that has been edited is shown as a removal of the old line and an
addition of the updated line.

Looking at differences between commits is one of the most common activities.
The `diff` command itself has a number of [useful
options](http://git-scm.com/docs/git-diff.html).

There is also a more elaborate front end to the `diff` command - [`git
difftool`](). `git difftool` is used for comparing and editing files that
changed between commits using common diff tools. There is a range of such
tools, including emerge, kompare, meld, and vimdiff.

There is a range of GUI-based tools supporting looking at differences and
editing files. For example:

* [Diffmerge](https://sourcegear.com/diffmerge/) (Free, cross-platform)
* [WinMerge](http://winmerge.org/) - open source tool available for Windows;
* GitHub [Compare
view](https://help.github.com/articles/comparing-commits-across-time)


The choice of GUI for viewing differences depends on the context in which you
are working and your own preferences related to choosing tools and
technologies.



### Looking at our history

To see the history of changes that we made to our repository (the most recent
changes will be displayed at the top):

~~~{.bash}
$ git log
~~~

The output shows: the commit identifier (also called revision number) which
uniquely identifies the changes made in this commit, author, date, and your
comment. *git log* command has many options to print information in various
ways, for example:

~~~{.bash}
$ git log --relative-date
~~~

Git automatically assigns an identifier (*COMMITID*) to each commit made to the
repository. In order to see the changes made between any earlier commit and our
current version, we can use  `git diff`  providing the commit identifier of the
earlier commit:

~~~{.bash}
$ git diff COMMITID
~~~

And, to see changes between two commits:

~~~{.bash}
$ git diff OLDER_COMMITID NEWER_COMMITID
~~~

Using our commit identifiers we can set our working directory to contain the
state of the repository as it was at any commit. So, let's go back to the very
first commit we made,

~~~{.bash}
$ git log 
$ git checkout COMMITID
~~~

We will get something like this:

~~~{.output}
Note: checking out '21cfbdec'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b new_branch_name
  
HEAD is now at 21cfbde... Initial structure and headings for the journal paper
~~~

*HEAD* is essentially a pointer which points to the branch where you currently
are. We said previously that *master* is the default branch. But *master* is
actually a pointer - that points to the tip of the master branch (the sequence
of commits that is created by default by Git). You may think of *master* as two
things: one as a pointer and one as the default branch. 

When we checked out one of the past commits HEAD is pointing to that commit but
does not point to the same thing as master any more. That is why git says You
are in 'detached HEAD' state and advises us that if we want to make a commit
now, we should create a new branch to retain these commits. 

If we created a new commit without creating a new branch Git would not know
what to do with it (since there is already a commit in master branch from the
current state which we checked out c4354a...). We will get back to branches and
HEAD pointer later in this tutorial. 



If we look at `journal.txt` we'll see it's our very first version. And if we
look at our directory,

~~~{.bash}
$ ls
~~~
~~~{.output}
journal.txt
~~~


then we see that our `references` directory is gone. But, rest easy, while it's
gone from our working directory, it's still in our repository. We can jump back
to the latest commit by doing:

~~~{.bash}
$ git checkout master
~~~

And `references` will be there once more,

~~~{.bash}
$ ls
~~~
~~~{.output}
common journal.txt
~~~
So we can get any version of our files from any point in time. In other words,
we can set up our working directory back to any stage it was when we made
a commit.


### Using tags as nicknames for commit identifiers

Commit identifiers are long and cryptic. Git allows us to create tags, which
act as easy-to-remember nicknames for commit identifiers.

For example,

```{.bash}    
$ git tag PAPER_STUB
```

We can list tags by doing:

```{.bash}    
$ git tag
```

Now if we change our file,

```{.bash}    
$ git add journal.txt 
$ git commit -m "..." journal.txt
```

We can checkout our previous version using our tag instead of a commit
identifier.

```{.bash}    
$ git checkout PAPER_STUB
```

And return to the latest checkout,

```{.bash}    
$ git checkout master
```

> ##Top tip: tag significant events {.callout}
>
> When do you tag? Well, whenever you might want to get back to the exact
> version you've been working on. For a paper, this might be a version that has
> been submitted to an internal review, or has been submitted to a conference.
> For code this might be when it's been submitted to review, or has been
> released.

Previous: [Setting up a local repository](02-local.html) Next: [Committing changes](04-commit-advice.html)