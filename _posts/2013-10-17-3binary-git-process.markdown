---
layout: post
title:  "3binary Git Process"
date:   2013-10-17 11:28:23
categories: Web Development
---

I've spent the past couple days, which happened to be my first couple days as a 3binary apprentice, learning about git, github, and 3binary's general git processes.  The biggest thing I learned in this time was about branches and how critical branching is to the process of grabbing code, updating it, and then eventually integrating it back into master.  At the Starter League, where I spent 11 weeks going from no Rails experience to someone who could build functioning sites, I never had to branch.  I always just built my own sites, was the sole contributor, and wasn't worried about messing something up since I could always just easily fix it as the sole owner of the code and noone else would have cared if there was a bust in the code.  Here is a high level overview of the git process 3binary uses (and I'm sure hundreds of other dev shops use).

###The Situation
Let's say there is a project up on github that you are going to add some awesome feature to and currently that code isn't on your local drive.

###Getting Started
First you need to git clone the code to your desktop.  Simply git clone "url".  Let's assume this was the master branch on github.

Now, once you have master saved locally, you can create your feature_branch which is going to track all the changes you make to master.

git checkout -b feature_branch

You now have a local branch where you can make your changes to the new feature.

####Ongoing Development
As you work in this branch, it's important to frequently commit changes.  That is, committing them locally, not to github.  This stamps them at points in time that you can always go back to.  Here are your options.

git add . (this reads all existing files but does not take into consideration new files)

OR

git add -A (this does git add . but also creates/deletes all new/old files)

THEN

git commit -m "message about what you did"

####Wrapping Up Local Changes / Rebasing
Let's say you are getting towards the tail end of your feature updates and are getting ready to 'apply' them back into master.  Conceptually, we're going to refresh our local master with the latest github code, and then 'rebase' the master code into the feature_branch to see if there were any conflicts.  A common conflict is if the same file had changes applied in the feature_branch AND the latest files pulled from github.

The first thing you want to do is make sure that your local master branch has all the latest updates from github.  To do this switch your local branch back to master from feature_branch.

git checkout master

Then, bring down the changes that have been applied to master since the original pull.

git fetch origin

Now, switch back to feature_branch.

git checkout feature_branch.

Now, we are going to 'rebase' master into feature_branch.  What this is going to do is unwind all the changes you made in feature_branch, bring in all the changes that have been made to master (what you pulled from fetch), and then re-layer in the feature_branch updates one at a time.  If git finds a conflict, such as two of the same files having changes that conflict, it will stop this and tell you.  It is not until you manually fix it that it will move on with the rebase.  This makes it very easy to navigate conflicts.

Finally, once it's completely rebased and there are no conflicts, you can push feature_branch up to github.

####Back to Github
To push your local feature_branch to github...

git push origin feature_branch

This creates a 'branch' which you often see on github repos.  Now I know what that is.

Once it's there, you can submit a pull request.  This is basically asking the repo owner 'can i merge my changes into master?'.  The repo owner has to approve the changes for them to actually be applied.

At 3binary, they do code reviews where they will review the change together to make sure everything makes sense and is good to go.  Once that's done and the code review is complete, the branch is no longer - it's officially master.

####Cleaning up locally
Now that this branch is complete, you no longer need it on your local git.  To delete it, you can simply delete it by going;

git branch -D feature_branch

####Other Tidbits
1) You can have multiple branches locally for multiple features you are working on.  You are not limited to one branch / feature at a time in addition to master.

2) Rebase and merge basically do the same thing, but rebase has its advantages.  Merge takes the changes from the git fetch origin and sticks them into your branch and orders the commits based on date.  Rebase moves all the fetch origin changes and puts them at the top then re-adds your local branch commits, making it nice and organize and easy to analyze.  Also, if there were conflicts with merge, it plops them all in one file where in Rebase it physically stops the rebase until a conflict is fixed.

3) If you want to switch between master, branch 1, branch 2, etc... locally and see the code for each of them this is easy to do.  Just go

git checkout branch_name

and look at your code.  It will be this branches code.


[jekyll-gh]: https://github.com/mojombo/jekyll
[jekyll]:    http://jekyllrb.com
