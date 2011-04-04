# How to claim and work on bugs

So you'd like to start contributing, but you don't know where to start? This is a fairly large project,
which means it's useful to begin with a small fix so you can get used to the codebase. We have an issue/bug tracker  
(we often use issue and bug interchangably) at <a href="http://bugs.joindiaspora.com" target="_blank">bugs.joindiaspora.com</a>. 

This page covers how to be assigned a bug, and what to do once you fix it.

1. Make an account on the <a href="http://bugs.joindiaspora.com/account/register" target="_blank">Diaspora bug tracker</a>.
* Make sure you've been added to list of people who can modify bugs on the tracker. This is called the 'Assignee' list.
    * Someone on IRC should be able to help you out. See [webchat](http://diasporatest.com/index.php/Beginning_IRC_with_Diaspora) for how to get into IRC.
* Claim an issue
    1. If you [[created a bug|Report-a-bug]] that you want to work on, assign it to yourself.
      * You can see a list of your activity by following <a href="http://bugs.joindiaspora.com/my/page" target="_blank">My Page</a> when you're logged in to the bug tracker.
      * Click on an issue
      * Scroll to the bottom of the page and click 'Update'
      * In the list of 'Assignee' people, select your username.
    * Finding a bug to work on
        * You can peruse the <a href="http://bugs.joindiaspora.com/projects/diaspora/issues" target="_blank">list of all issues</a>
        * On the right hand side of the Issues page, there are some handy queries that might help narrow down the list.
          * <a href="http://bugs.joindiaspora.com/projects/diaspora/issues?query_id=7" target="_blank">Quickfixes for new developers</a>
          * <a href="http://bugs.joindiaspora.com/projects/diaspora/issues?query_id=5" target="_blank">Confirmed bugs</a>
* Updating the issue's progress

   * To change any fields mentioned: Scroll to the bottom of the issue page and click 'Update'. From there you can edit the issue.
 
   1. If someone has duplicated the issue, mark the issue status 'Confirmed'
    * Ideally, there should be a spec written for this bug. If you've done this, mark the issue 'Isolated'
    * If a pull request has been made for the issue, senior members will set the status 'Awaiting Acceptance'
    * As you make progress, particularly if the fix is more than a day, please set the '% Done' to something meaningful
 * Don't forget about the all important [git](https://github.com/diaspora/diaspora/wiki/Git-Workflow)