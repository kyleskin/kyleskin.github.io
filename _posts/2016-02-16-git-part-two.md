---
layout: post
title: Git Part Two
---

Before we go into a little more detail, let's quickly review the three steps to the Git workflow.  Like I said in the last post, once you understand and memorize these three steps, the whole Git process will make much more sense and will become second nature.

The workflow is:

1. Modify - making changes to file.
2. Stage - you add a file to Git's staging area, telling Git you're ready for it to store the file.
3. Commit - this is where you actually tell Git to store the file as the latest version.

We're going to start with step 1 - Modify.  Let's first create a directory to store our files in and then a Readme file.  I'm going to be working within the command line and assume you have a basic understanding of how to use the terminal.   *If you don't know anything about using your computer's terminal, I recommend [Zed A. Shaw's Command Line Crash Course](http://cli.learncodethehardway.org/book/) to get you started.*


{% highlight shell %}
Kyle:Desktop $ mkdir blog
Kyle:Desktop $ cd blog
Kyle:blog $ touch Readme.md
{% endhighlight %}
>Here we've made a folder called 'blog' and created the Readme file inside it.

We've now modified a file so next we move on to step 2 - Stage.  First, though, we need to start Git in our directory.  To do this, we run the command `git init`.  This is a one-time command we run at the very beginning just to get Git up and running.

{% highlight shell %}
Kyle:blog $ git init
Initialized empty Git repository in /Users/skinner/Desktop/blog/.git/
{% endhighlight %}
>Git is now running so we're ready to continue to step 2.

As you see in the output line, Git started an empty repository.  The repository is where Git will store its files and save our changes.

To move our modified file into Git's staging area, we will first check the status of the file with the `git status` command.

{% highlight shell linenos%}
Kyle:blog $ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	Readme.md

nothing added to commit but untracked files present (use "git add" to track)
{% endhighlight %}
>Git status run on unstaged file.

This output gives us a lot of information, but what we're most interested in is lines 6 and 9.  Line 6 tells us that there is a modified file (called untracked files here) and line 9 tells us that the name of the modified file is Readme.md (this should appear red in your terminal).  We're now going to tell Git to mark that file as one that we want it to move to the staging area with `git add *filename*`.

{% highlight shell %}
Kyle Skinner
Kyle:blog $ git add Readme.md
{% endhighlight %}
>When run correctly, there is no output with this command.

Git has now staged our Readme file, so let's check its status again with `git status`.

{% highlight shell linenos %}
Kyle Skinner
Kyle:blog $ git status
On branch master

Initial commit

Changes to be committed:
 (use "git rm --cached <file>..." to unstage)

 new file:   Readme.md
 {%endhighlight %}
 >Git status on *staged* file.

 Again, this status gives us a lot of information, but we're going to focus on lines 7 and 10.  Line 7 says there is a file that is now ready to be saved (file to be committed) and line 10, just like line 9 from before, tells us the name of the file.  We're now done with step 2 and ready for step 3 - Commit.

 In order to commit the change to Git's repository, we run `git commit -m "*message*"`.  The message in the command line is a short description of what change we made.

 {% highlight shell linenos %}
 Kyle Skinner
Kyle:blog $ git commit -m "Initial commit and create Readme.md"
[master (root-commit) 6ddd001] Initial commit and create Readme.md
1 file changed, 0 insertions(+), 0 deletions(-)
create mode 100644 Readme.md
{% endhighlight %}
>We've successfully committed our change!

And that's it.  We have now Modified, Staged, and Committed.

The next post will be on how to link our local Git repository to the website [GitHub.com](github.com) so we can have a remote repository that will allow us to collaborate with others.
