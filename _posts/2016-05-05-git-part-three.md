---
layout: post
title: Git Part Three: Github
---

In this post, we'll discuss how to create a repository on GitHub and link it to our local git repository.  By creating a GitHub repo, we're able to have a copy of our work in the cloud and can also collaborate with others.

We're going to be picking up where we left off in part two, having initialized git in our folder and made our first commit.  The next step is to create a remote repository on GitHub - you do this simply by linking on the new repository link.  You will name the repository the same name you've used for the folder you're working in.  **It's very important that you do NOT select the "initialize this repository with a README" option.  We have already initialized git on our local machine, so we do not need GitHub repeating that step.**

We now have a repo on GitHub, but we need to link our local repo with the remote GitHub repo - which we do through the command `git remote add *name of remote repo* *url of remote repo*`.

The convention for the name of the remote is `origin`.  You'll see this used constantly to mean the remote copy of the work you have on your local machine.  It's important to remember this and follow the convention because as you begin to work with others, you will have additional remotes and will need to keep the names straight.

The url we use will come from GitHub, but follows the format of `git@github.com:*your user name*/*the repo name*.git`.

We can check that everything worked properly by running `git remote -v`.  This will tell us any remote repos that are connected to this local repo.  We should see the output:

{% highlight shell}
$git remote -v
origin git@github.com:*username*/*reponame*.git (fetch)
origin git@github.com:*username*/*reponame*.git (push)
{%endhighlight%}

The fetch and push mean this repo is set up for us to copy code down from as well as push our code up to.

To push our code up to GitHub, we'll first use the command `git push -u origin master`  To break this down, the `-u` is telling git to remember where we're pushing our code to so in the future it will already know.  The `origin` is the location of the remote repo and `master` is the name of the git branch we're pushing.  Since this is the first push to GitHub we should be on master, but that won't always be the case.

Since we used `-u`, if we're pushing from the master branch again, we just have to type `git push` and it will know where to send the code.

That's the basics of git.  We've only just barely scratched the surface of what it can do, however.  It's a great tool and is essential for developers - so use it, learn it, and love it.
