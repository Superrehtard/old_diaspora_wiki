----

###301 MOVED PERMANENTLY###

We're currently **moving this wiki over to our new project site**. The contents of this page have  already been carried over, so _any new changes here will not be reflected in the new wiki_.  
New link: http://wiki.diasporafoundation.org/Git_Workflow

----

If you're a developer who wants to work on the Diaspora source code and submit your changes for consideration to be merged into core Diaspora* code, here's how.  Thanks to [ThinkUp](https://github.com/ginatrapani/ThinkUp) for their awesome developer guide, which inspired ours.

# [Branching model] (#branching-model) <a name="branching-model">

Firstly the most important part, the branching model that Diaspora follows. Our branching model is based on [a post on Git branching models](http://nvie.com/posts/a-successful-git-branching-model/) by [nvie](http://nvie.com/about/).

In short, the model is explained nicely in this picture:
![nvie git branching model](http://nvie.com/img/2009/12/Screen-shot-2009-12-24-at-11.32.03.png)

To make following this model easier we use [git-flow](https://github.com/nvie/gitflow) which contains high level extensions for this Git branching model. Please start by [installing the git-flow extensions](https://github.com/nvie/gitflow/wiki/Installation) as per installation instructions on their project page. There is also [a blog post](http://jeffkreeftmeijer.com/2010/why-arent-you-using-git-flow/) explaining in short how git-flow basics work.

Please note that the usage of git-flow extensions for Diaspora* does not stop the usage of vanilla git commands! If you are not able to use git-flow or do not feel comfortable with using it, please feel free to use normal git commands. But we do enforce the branching model so please read the branching model post carefully and follow the guidelines closely.

Discussion on improving this branching model happens on [Loom.io](http://loom.io/discussions/628).

# [Quickfire Do's and Don't's] (#dos-and-donts) <a name="dos-and-donts">

If you're familiar with git and GitHub, here's the short version of what you need to know. Once you fork and clone the Diaspora code:

*  **Never, ever, do anything in master branch. The branch develop is the head of development, master is for stable releases!**

*  **Don't develop on the develop branch.** Always create a feature branch specific to [the issue](https://github.com/diaspora/diaspora/issues) you're working on. Name it by issue # and description. For example, if you're working on Issue #359, an aspect naming fix, your feature branch should be called 359-aspect-names. If you decide to work on another issue mid-stream, create a new branch for that issue--don't work on both in one branch.

* **Do not merge** the upstream develop with your feature branch; **rebase** your branch on top of the upstream develop.

* **A single feature branch should represent changes related to a single issue.** If you decide to work on another issue, create another feature branch from develop.

# [Step-by-step (the short version)] (#step-by-step-short) <a name="step-by-step-short">

1. Fork on GitHub (click Fork button)
2. Clone to computer (`$ git clone git@github.com:you/diaspora.git`)
3. Don't forget to cd into your repo: (`$ cd diaspora/`)
4. Set up remote upstream (`$ git remote add upstream git://github.com/diaspora/diaspora.git`)
5. If you use git-flow initialize it: `git checkout master && git flow init -d && git checkout develop`
6. Start working on a new issue or feature (`$ git flow feature start 100-description`, naming convention is *issuenumber-description*, if you don't have a bug report no worries just skip the number)
7. Develop on feature. (`$ git add . ; git commit -m 'commit message'`)
8. Push branch to GitHub (`$ git flow feature publish 100-description`, just to stop you losing local changes on the event of hardware failure)
9. Fetch upstream (`$ git fetch upstream`)
10. Update local develop (`$ git checkout develop; git pull --rebase upstream develop`)
11. Switch back to feature (`$ git flow feature checkout 100-new-feature ; git rebase develop`)
12. Repeat steps 8-11 till dev is complete
13. Rebase develop in to feature branch (`$ git flow feature rebase 100-description`)
14. Publish feature branch to Github (`$ git flow feature publish 100-description`)
15. Issue pull request for develop branch (Click Pull Request button) 

Note! Do not do `git flow feature finish` for your branch. Submit the whole feature branch as a pull request instead without finishing it. It will be merged to _develop_ by a reviewer.

Note! If you feel this feature should instead be a hotfix, do it still as a feature but mention in the pull request that this pull request would be good as a hotfix directly to master. Don't forget to explain why! :)

# [Step-by-step (the long version)] (#step-by-step-long) <a name="step-by-step-long">

If you're new to git and GitHub, here's the longer version of these instructions.

## [Install git and git-flow] (#install-git-and-git-flow) <a name="install-git-and-git-flow">

1. [Install Git for your platform](http://git-scm.com/downloads)
2. [Install git-flow extensions](https://github.com/nvie/gitflow/wiki/Installation) IF available for your platform
3. (Optional but highly recommended) [Install git-flow completion](https://github.com/bobthecow/git-flow-completion) for some bash auto-complete magic

## Create an account on GitHub and fork the Diaspora repository.

1. Create a free account on GitHub, and log in.
2. Go to [the main Diaspora page on GitHub](http://github.com/diaspora/diaspora).
3. Click the "Fork" button near the top of the screen. This will get you your own copy that you can make changes to.
![](http://img.skitch.com/20101018-j5xmmrs6ccn9ku2gic12y13cf9.png)

## Establish connectivity between your GitHub account and your development machine.

1. Generate an SSH key on your development machine. Here's a [good guide](http://help.github.com/key-setup-redirect) that gives you specific directions for whatever OS you're accessing the page with.
2. Make sure you've got an SSH public key on your machine and recorded in your GitHub account. You can see your SSH Public Keys on the Account Overview section of your GitHub account.
![](http://img.skitch.com/20101018-nayqmm1ne8qdyafpjb7tib4fi4.png)
3. To test the GitHub authentication run:

~~~
$ ssh git@github.com
~~~

If all is well, you should see something like this:

~~~
PTY allocation request failed on channel 0
ERROR: Hi username! You've successfully authenticated, but GitHub does not provide shell access
Connection to github.com closed.
~~~

## Clone your GitHub fork to your development machine

Run a clone command against your GitHub fork. It will look something like this except that it will use your GitHub account name instead of "you":

~~~
$ git clone git@github.com:you/diaspora.git 
~~~

That downloads your copy of Diaspora to a git repository on your development machine. Change directory into the new diaspora directory.

Check that you have [git-flow](https://github.com/nvie/gitflow) installed and run

~~~
$ git checkout master
$ git flow init -d 
$ git checkout develop
~~~

Now you need to install everything you need to run it - which is quite a lot of stuff. We have a guide to [installing and running Diaspora](http://github.com/diaspora/diaspora/wiki/Installing-and-Running-Diaspora) which should help. Pop into #diaspora on IRC (freenode) if you have problems.

You'll know you're done when you can run specs (in two stages - the cucumber features, which are selenium acceptance tests, and the rspec tests, which are unit tests) by doing this:

~~~
$ rake spec
~~~

**We try to make sure these always succeed.** Our [continuous integration server](http://ci.joindiaspora.com) will tell you if there any current failures. If you have any test failures that you don't see on CI, come ask in the #diaspora-dev IRC channel.

## Figure out what to work on

Maybe you have a feature addition in mind, but if not, check out our [issue tracker](https://github.com/diaspora/diaspora/issues), or come ask in IRC what needs doing.

## Create an Issue-Specific Feature Branch

Before you start working on a new feature or bugfix, create a new feature branch in your local repository that's dedicated to that one change. Name it by issue number (if applicable, if there's no issue just skip it) and description. For example, if you're working on issue #359, a aspect naming bugfix, create a new branch called 359-aspect-names, like this:

~~~
$ git flow feature start 359-aspect-names
~~~

## Write awesome code

You must write unit tests for all bug fixes, no matter how small. We can help you! 

Edit and test the files on your development machine. When you've got something the way you want and established that it works, commit the changes to your branch on your development server's git repo.

~~~
$ git add <filename>
$ git commit -m 'Issue #359: Some kind of descriptive message' 
~~~

You'll need to use git add for each file that you created or modified. There are ways to add multiple files, but I highly recommend a more deliberate approach unless you know what you're doing.

Then, you can push your new branch to GitHub, like this (replace 359-aspect-names with your branch name):

~~~
$ git flow feature publish 359-aspect-names
~~~

You should be able to log into your GitHub account, switch to the branch, and see that your changes have been committed. Then click the Pull button to request that your commits get merged into the Diaspora development trunk.  

## Keep Your Repository Up to Date

In order to get the latest updates from the development trunk do a one-time setup to establish the main GitHub repo as a remote by entering: 

~~~
$ git remote add upstream git://github.com/diaspora/diaspora.git
~~~

Verify you've now got "origin" and "upstream" remotes by entering: 

~~~
$ git remote
~~~

You'll see upstream listed there.

## <a name='wiki-gitrebase'></a> Rebase Your Development Branch on the Latest Upstream

To keep your development branch up to date, rebase your changes on top of the current state of the upstream master. Check out *[What's git-rebase?](#gitrebase1)* below to learn more about rebasing.

If you've set up an upstream branch as detailed above, and a development branch called 100-retweet-bugfix, you'd update upstream, update your local master, and rebase your branch from it like so:

```
$ git fetch upstream
$ git checkout develop
$ git pull upstream develop
$ git flow feature checkout 100-retweet-bugfix
[make sure all is committed as necessary in branch]
$ git rebase develop
```
You may need to resolve conflicts that occur when a file on the development trunk and one of your files have both been changed. Edit each file to resolve the differences, then commit the fixes to your development server repo and test. Each file will need to be "added" before running a "commit." 

Conflicts are clearly marked in the code files. Make sure to take time in determining what version of the conflict you want to keep and what you want to discard. 

```
$ git add <filename>
$ git commit 
```

To push the updates to your GitHub repo, replace 100-retweet-bugfix with your branch name and run:

```
$ git flow feature publish 100-retweet-bugfix
```

## Send Diaspora a pull request on GitHub

Github will notify us and we'll review your patch and either pull it in or comment on it

Helpful hint: You can always edit your last commit message by using:

```
$ git commit --amend
```

## Some gotchas

***Be careful not to commit any of your configuration files, logs, or throwaway test files to your GitHub repo.*** These files can contain information you wouldn't want publicly viewable and they will make it impossible to merge your contributions into the main development trunk of Diaspora. 

Most of these special files are listed in the *.gitignore* file and won't be included in any commit, but you should carefully review the files you have modified and added before staging them and committing them to your repo. The git status command will display detailed information about any new files, modifications and staged.

```
$ git status 
```

***One thing you may not want to do is to issue a git commit with the -a option. This automatically stages and commits every modified file that's not expressly defined in .gitignore, including your crawler logs.***

```
$ git commit -a 
```

## <a name='wiki-gitrebase1'></a> What's git-rebase?

Using `git-rebase` helps create clean commit trees and makes keeping your code up-to-date with the current state of upstream easy. Here's how it works.

Let's say you're working on Issue #212 a new plugin in your own branch and you start with something like this:

```
          1---2---3 #212-my-new-plugin
         /
    A---B #develop
```

You keep coding for a few days and then pull the latest upstream stuff and you end up like this:

```
          1---2---3 #212-my-new-plugin
         /
    A---B--C--D--E--F #develop
```

So all these new things (C,D,..F) have happened since you started. Normally you would just keep going (let's say you're not finished with the plugin yet) and then deal with a merge later on, which becomes a commit, which get moved upstream and ends up grafted on the tree forever.

A cleaner way to do this is to use rebase to essentially rewrite your commits as if you had started at point F instead of point B. So just do:

```
git rebase develop 212-my-new-plugin
```

git will rewrite your commits like this:

```
                      1---2---3 #212-my-new-plugin
                     /
    A---B--C--D--E--F #develop
```

It's as if you had just started your branch. One immediate advantage you get is that you can test your branch now to see if C, D, E, or F had any impact on your code (you don't need to wait until you're finished with your plugin and merge to find this out). And, since you can keep doing this over and over again as you develop your plugin, at the end your merge will just be a fast-forward (in other words no merge at all).

So when you're ready to send the new plugin upstream, you do one last rebase, test, and then merge (which is really no merge at all) and send out your pull request. Then in most cases, a reviewer has a simple fast forward on her end (or at worst a very small rebase or merge) and over time that adds up to a simpler tree.

More info on the git man page here: 
[Git rebase: man page](http://schacon.github.com/git/git-rebase.html)