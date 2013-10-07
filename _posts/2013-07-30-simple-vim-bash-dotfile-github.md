---
layout: post
title:  "A simple github repo for vim and bash dotfiles"
date:   2013-07-30 19:55:05
description: "Tutorial about howto make a simple github repo to handle your dotfiles"
categories: ['linux','web']
image: "dotfiles.png"
article: yes
tags: ['github', 'bash','vim','dotfiles']
---

* my toc
{:toc}

# Context

There are plenty of github dotfiles framework on [github dotfiles][github_dotfiles]
They are complicated and I don't want to add commands or aliases that I don't actually master.
This is for like minded person that I write this tutorial

# Create a dotfiles github repo

It's pretty straightforward and this tutorial isn't about git so. If you don't feel comfortable
with it just take a look at the awesome [github doc][github_doc]


# Organise your stuff

Basically I need to store on github only my  `.bashrc` and vim configuration files.
Including plugins.

The easier way to do it is maybe to store everything as it is stored on my `/home` in the repo.
The install script will then symlink the repo's content to my `/home`.

## Repo organisation

{% highlight text linenos=table %}.
repo_root
|_install.sh
|_dotfiles
    |_.bashrc
    |_.inputrc
    |_.vimrc
    |_.vim
        |_ bundle
        |_ colors
        |_ synthax
        |_ ...
{% endhighlight %}

Content of  `.vim/bundle` should be subrepositories in order to always have under the hand the
latest plugins version.

We will see that in [this article last chapter](#working_with_submodules).

# Think about the install script

I don't want to change my install script each time I add a dotfile, a vim plugin or whatever.
For my personnal purpose I think that a simple sequence like:

1. Get latest change `git pull`
1. Eventually commit local changes `git push`
1. Resinstall everything `./install.sh`

Will work great for my need.

Indeed `./install.sh` can make the git pull/push for me ;)

## An exemple install script


{% highlight bash linenos=table %}.

#!/bin/bash

echo 'Dotfiles -  http://gustavepate.github.io/'

HOME_DIR=~  #For testing purpose
SYNC_ROOT=./dotfiles
SYNC_FILES=$SYNC_ROOT/*

SCRIPT_FULL_PATH=$(readlink -f $0)
REPO_PATH=$(dirname $SCRIPT_FULL_PATH)

echo $REPO_PATH

# If Git is not installed...
if [[ ! "$(type -P git)" ]]; then
    sudo apt-get -qq install git
else
    echo "git installed.....ok"
fi


#Command must be run from the repo root
if [ ! -e $SYNC_ROOT'/.bashrc' ]
then
    echo "install must be execute from the repo root"
    exit 1
else
    echo "command run from secure path.....ok"
fi

#Pull latest change

git pull

#Symlink all files and directory in the dotfiles directory to the home folder

#allowi bash  to find .dotfiles
shopt -s dotglob

#for all files and directory in the dotfiles directory
for f in $SYNC_FILES
do
    # take action on each file. $f store current file name
    echo "Processing $f file..."

    #strip the left part
    FILE=`basename $f`

    #create /home equiv for repo file
    H_FILE=$HOME_DIR'/'$FILE

    #create backup path
    TIMESTAMP=`date +%d%m%Y-%H%M%S`
    H_FILE_BACKUP=$HOME_DIR'/'$FI'.'$TIMESTAMP'.bck'

    echo "Checking $H_FILE"

    #check equivalent in the homedir

        #if a link, delete it
        if [ -L $H_FILE ];then
            rm $H_FILE
            echo $H_FILE" rmed...........ok"

        #if a dir or a file => mv
        elif [ -e $H_FILE ]; then
            echo $H_FILE" backuped...........ok"
            mv $H_FILE $H_FILE_BACKUP
        # weird
        else
            echo "WARNING: I don't know what to do with this file"
        fi

    #create a new link
    ln -s $REPO_PATH'/'$f $H_FILE
    echo $H_FILE' symlinked to '$REPO_PATH'/'$f

done

{% endhighlight %}

[Here is the current source][install_sh]


# Working with sub-repos

For the moment, my git repo only contains clones of others repository.

Here are my needs:
* I wan't to automatically keep my vim plugins up-to-date.
* On a fresh computer I don't want to manually add all my vim plugins

One can use [submodule][git_submodules] or [subtree][git_subtree].

__Edit 2013-08-13: __ giving up submodules

Submodule is the common way to go but they are a pain to delete.

I tried these a little before giving up. Indeed I was changing my vim plugins list quickly
at this time and it was not a sustainable solutions. See [the necessary steps to delete a
submodule]({{page.url}}#delete_a_submodule) and you will agree with me ;)

So I tried to use subtree but my ubuntu 12.04 don't come with the necessary git version.
I gave up and stay with git clone for the moment.

For the record, you will find notes on the git subtree and git submodules below.


# Conclusion

Et voilà ! Here is a link to [my own github dotfiles repo][dotfiles_repo]
where you can find the latest version of the [install script][install_sh] I currently use.
It took me a little more than a hour to make it working (including typing this article).

I hope this will help you !


## Appendix I: Setup a subtree


### Adding a remote

{% highlight bash linenos=table %}
git remote add -f minibufexpl.vim https://github.com/fholgado/minibufexpl.vim
{% endhighlight %}

### Fetch a repo as a subtree

{% highlight bash linenos=table %}
git subtree add --prefix dotfiles/.vim/bundle/minibufexpl.vim minibufexpl.vim master --squash
{% endhighlight %}

### Update a subtree

{% highlight bash linenos=table %}
git fetch minibufexpl.vim master
git subtree pull --prefix dotfiles/.vim/bundle/minibufexpl.vim minibufexpl.vim master --squash
{% endhighlight %}

### Delete a subtree

{% highlight bash linenos=table %}
#TODO
{% endhighlight %}

## Appendix II: Setup a submodule

I warn you about it subtrees have proven to be more relevant to my needs than submodule.
But, anyway, here's some submodule documentation if you need it.

### Add a submodule

First add a submodule to your git repo:

{% highlight bash linenos=table %}
git submodule add  https://github.com/tpope/vim-fugitive dotfiles/.vim/bundle/vim-fugitive
{% endhighlight %}

Run it __without__ a trailing slash !

It clones `https://github.com/tpope/vim-fugitive`
in the directory `dotfiles/.vim/bundle/vim-fugitive`

{% highlight bash linenos=table %}
# OK: adds the submodule
git add local/path

# NO !!!! adds all the files in the submodule directly into your repository
git add local/path/
{% endhighlight %}

Then you can:

{% highlight bash linenos=table %}
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#   new file:   .gitmodules
#   new file:   dotfiles/.vim/bundle/vim-fugitive

{% endhighlight %}

Notice how the change log only show you the directory of the submodule without all its content.

### Secure the submodule

Notice the `.gitmodule` file.
Let's take a look.


{% highlight text linenos=table %}
[submodule "dotfiles/.vim/bundle/vim-fugitive"]
	path = dotfiles/.vim/bundle/vim-fugitive
	url = https://github.com/tpope/vim-fugitive
{% endhighlight %}

pretty straightforward hum ?

You can tweak in in order to ignore changes made by you in the submodules.

{% highlight text linenos=table %}
[submodule "dotfiles/.vim/bundle/vim-fugitive"]
  path = dotfiles/.vim/bundle/vim-fugitive
  url = https://github.com/tpope/vim-fugitive
  #ignore changes in the submodule while commiting
  ignore = dirty
{% endhighlight %}

### Untracked change

If, on commit, git notifies you with a Untracked Content Error

{% highlight bash linenos=table %}
# On branch master
# Changes not staged for commit:
# (use "git add ..." to update what will be  committed)
# (use "git checkout -- ..." to discard changes in working directory) # (commit or
# discard the untracked or modified content in submodules) #
# modified: bundle/snipmate
# (untracked content)
{% endhighlight %}

Then the solution is to:

{% highlight bash linenos=table %}
cd submodule/path
git add /previouly/detected/untrack/content
git commit -a -m "untracked"
{% endhighlight %}


### Refreshing submodules

To refresh get uptodate submodules all you have to do is to type:

{% highlight bash linenos=table %}
git submodule init
git submodule update
git submodule foreach git pull origin master
{% endhighlight %}


Then you will wan't to commit the changes which lead us to the following function

{% highlight bash linenos=table %}
# get latest submodules
function submodules_latest {
    # init update and retrieve last head of each submodule
    git submodule init
    git submodule update
    git submodule foreach git pull origin master

    # Commit the submodule changes
    git add .
    git commit -a -m "Update submodules to the latest version"
    git push origin master
}
{% endhighlight %}

### Delete a submodule

Because, one day, you will wan't to do it. Here are the steps to perform in ordrer to remove
a submodule.

1. Delete the relevant section from the .gitmodules file.
1. Stage the .gitmodules changes git add .gitmodules
1. Delete the relevant section from .git/config.
1. Run git rm --cached path_to_submodule (no trailing slash).
1. Delete the entry in .git/modules. Run rm -rf .git/modules/submodule_name
1. Commit.
1. Delete the now untracked submodule files. Run rm -rf path_to_submodule


[github_doc]: https://help.github.com/articles/create-a-repo "create a repo github doc"
[github_dotfiles]: http://dotfiles.github.io/ "github dotfiles doc"
[dotfiles_repo]: https://github.com/GustavePate/dotfiles "my github dotfiles repository"
[git_submodules]: http://speirs.org/blog/2009/5/11/understanding-git-submodules.html "detail explanations on submodule"
[git_subtree]: http://blogs.atlassian.com/2013/05/alternatives-to-git-submodule-git-subtree/ "howto subtree"
[install_sh]: https://github.com/GustavePate/dotfiles/blob/master/install.sh "dotfiles install script"
