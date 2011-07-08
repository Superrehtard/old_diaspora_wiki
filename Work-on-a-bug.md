# How to claim and work on bugs

So you'd like to start contributing, but you don't know where to start? This is a fairly large project,
which means it's useful to begin with a small fix so you can get used to the codebase. We have an issue tracker 
at <a href="https://github.com/diaspora/diaspora/issues" target="_blank">github.com/diaspora/diaspora/issues</a>. Note that here
and elsewhere we often use the terms "issue" and "bug" interchangably.

This page covers how to be assigned a bug, and what to do as you work on it.

1. Make an account on <a href="https://github.com" target="_blank">Github</a>.
* Make sure you've been added to list of people who can modify bugs on the tracker. This is called the 'Assignee' list.
    * Someone on IRC should be able to help you out. See [[how we use IRC|How-we-use-IRC]] for access information.
* Claim an issue
    1. If you [[created a bug|Report-a-bug]] that you want to work on, assign it by commenting on the issue to let everyone know you're working on it.
    * Finding a bug to work on
        * You can peruse the <a href="https://github.com/diaspora/diaspora/issues" target="_blank">list of all issues</a>
        * On the left hand side of the Issues page, there are some handy labels that might help narrow down the list.  Try looking at the QuickFix label for some smaller issues.
* Before you start working on the bug, read about [[how we use git|Git-Workflow]].
* Updating the bug's progress
   1. To change any fields mentioned: Scroll to the bottom of the issue page and click 'Update'. From there you can edit the issue.
    * If someone has duplicated the issue, mark the issue status 'Confirmed'
    * Ideally, there should be a spec written for this bug. If you've done this, mark the issue 'Isolated'
    * If a pull request has been made for the issue, senior members will set the status 'Awaiting Acceptance'
    * As you make progress, particularly if the fix is more than a day, please set the '% Done' to something meaningful
