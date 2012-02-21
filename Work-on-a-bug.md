# How to work on bugs

Thank you for helping us make Diaspora awesome! ;)  
So you'd like to start contributing, but you don't know where to begin? This is a fairly large project,
which means it's probably best to get your hands dirty with a small fix so you can get used to the codebase. 

1.  **Make an account on [Github][github]**  
    (If you don't already have one.) This is necessary in order to contribute the code you write.
2.  **Find a bug to work on**  
    One possibility is to look at the [issues list on Github][issues]
    and look for bugs with the "quickfix" label. Those bugs often cover a wide selection of topics for 
    various skillsets and interests (Ruby/Rails, HTML/CSS, JavaScript...).  
    Another way is to look for the weekly *Bug Mash* entries on the [Devblog]. 
    Those contain a list of easy-to-start-with bugs that come with an outline of how to attack them.
3.  **Claim the issue**  
    If you selected your bug directly from the issue tracker, just leave a note in the comments to that bug, 
    saying you started to work on it.  
    The *Bug Mash* posts contain their own information of how to claim them. Typically it's by responding 
    to the email on the [mailing list][discuss] or leaving a comment on the [blog post][devblog].
4.  **Before you start**  
    Now, there are a few things we want you to keep in mind before you start to work on the bug.
    *   You will need to [[set up a development environment|Installing and Running Diaspora]]
    *   Please skim through our page on [[how we use git|Git-Workflow]] so that there will be no problems merging your fix.
    *   You migh also be interested in an [[introduction to the source code|An Introduction to the Diaspora Source]] and
        the [[FAQ for developers]].

    If you need help with any of that, run into a problem or get stuck somewhere, don't hesitate to ask, 
    either on the [mailing list][discuss] or come to our [[IRC chatroom|How we use IRC]].
5.  **Write awesome code**  
    We strive to encourage *TDD* ([Test Driven Development][tdd]) or *BDD* ([Behaviour Driven Development][bdd]) 
    when possible. In short that means before you write some actual code, you should write a test firsts that fails
    because of the bug, then fix the bug, which in turn also makes the test pass.
6.  **Submit your fix**  
    Now it's time to push your git branch to your fork on Github and make a pull request for it. Look 
    [[here|Git Workflow]] or [here][pull] for specific instructions on how to do that. Also, it's best to let us know
    that you are finished with a quick email to the [mailing list][discuss] or comment on the issue, 
    pointing to the pull request.
7.  **Done**  
    *Great! You just fixed a bug. We really can't thank you enough for it. <3*


[github]: https://github.com
[issues]: https://github.com/diaspora/diaspora/issues
[devblog]: http://devblog.joindiaspora.com/
[discuss]: http://groups.google.com/group/diaspora-discuss
[tdd]: https://en.wikipedia.org/wiki/Test_Driven_Development
[bdd]: https://en.wikipedia.org/wiki/Behavior_Driven_Development
[pull]: http://help.github.com/send-pull-requests/