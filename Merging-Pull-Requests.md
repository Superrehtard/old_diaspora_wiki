This is a casual description of how I (Raphael Sofaer) work when pulling in a pull request:

1. See a pull request in the Pull Requests page.

2. Check workflow to make sure it will be pullable:  eg. Check that there are no commits from the Diaspora trunk in the pull request, and that the pull request is from a branch other than master.  If workflow is not correct, I will comment, but often still try to pull it in.

3. Review the changes by eye.  Github provides a pretty nice 'files changed' feature.  If there are obvious problems, comment.  Check that specs were added for new functionality, and hopefully were added for bugfixes as well.

4. Fetch the remote repo so I can review it locally.

    1. Assuming the pull request is from user pullrequester8000, I would go to my repo and 
```bash
  'git remote add pr8000 git://github.com/pullrequester8000/diaspora.git'
```
  
    2. Now fetch the remote repo so it is copied locally:
```bash
  'git fetch pr8000'
```
5. If the change is large, checkout a merge branch to test on:
```bash
  'git checkout -b pr8000-merge'
```
6. Merge the remote branch into the current one (assume remote branch is named 823-cause-victory):
```bash
  'git merge pr8000/823-cause-victory'
```
7. If there were conflicts, I often fix small conflicts with clear fixes, but other conflicts, I leave a comment on github and ask the requester to rebase onto master.

8. Run ```'rake'``` to make sure there are no test failures.

9. Push back to master