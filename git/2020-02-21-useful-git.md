---
layout: post
title: Useful Git Commands
subtitle : Commands I use (almost) everyday
tags: [Git, TIL]
author: Jaewon Chung
comments : True
---
Git is hard: there are many benefits of using a version-control system (VCS), but I found learning git to be unapproachable. So I compiled a list of commands I use often and a description of what they do.

## Cloning a repository
Git repositories are often stored online, such as [GitHub](https://github.com/), [GitLab](https://gitlab.com), [Bitbucket](https://bitbucket.org). In order to make changes locally, you will need to make a clone of the reposity on your computer. 

{% highlight shell %}
git clone <url>
# To clone to a specific folder
git clone <url> <folder-name>
{% endhighlight %}


## Changing branches
Branches represent an independent line of development. If you are trying to contribute to a repository, then the best practice is to make a branch and make changes in that branch. 
{% highlight shell %}
# If your branch already exists
git switch <branch>
# To create new branch
git switch -c <branch>
{% endhighlight %}

## Saving changes
In order to make a "snapshot", or save, the changes you have made, you need to first stage the changed files and then make a commit. 

### Viewing files that you have changed or added
{% highlight shell %}
git status
{% endhighlight %}

### Staging files
Before you can make a commit, you have add files to the staging area.
{% highlight shell %}
# This will stage all files in current directory
git add .
# This will stage specific files
git add <file>
# This will stage files matching extension
git add *.<extension>
{% endhighlight %}

### Unstaging files
If you staged a file by mistake, you can also unstage the file.
{% highlight shell %}
git restore --staged <file>
{% endhighlight %}

### Making a commit
Once you have all the files you want saved in the staging area, you are ready to make a commit.
{% highlight shell %}
git commit -m "<commit message>"
{% endhighlight %}

### Pushing your commit
If you use online services like GitHub, then you will want to push your changes to your repository.
{% highlight shell %}
git push origin
{% endhighlight %}

## Undoing your latest commit
Sometimes you make a mistake and want to undo a commit.
{% highlight shell %}
# This will move all commited files back to staged.
git reset HEAD~ --soft
# This will remove all changes made and revert back to previous commit.
git reset HEAD~ --hard
{% endhighlight %}