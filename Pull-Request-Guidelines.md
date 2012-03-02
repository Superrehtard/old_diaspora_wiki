So! You've written an awesome new feature, and you're all ready to make a pull request after working hard on it. Here's some things to keep in mind.

### 1. Is all of your code license-compatible?

According to our [CLA](https://github.com/diaspora/diaspora/wiki/New-CLA--12-13-10), it's really important that we stick to the AGPL. By extension, your code should be, too. For example, if you're including javascript from an external resource, it's absolutely vital that you ensure that the code you've used and worked on is equally compatible with the AGPL. Otherwise, we can't use it.

### 2. Is all of your code compatible with the current main branch?

The main repository is updated on a near-daily basis, and lots of fixes and modifications are pulled in constantly. Occasionally, this means that some things will break. If you're working on something that is updated quite constantly (such as application.sass, which deals with all of the CSS of the application), then it's important that you check it daily to ensure that you don't cause any regressions.

### 3. Is your feature a really big feature?

Some features take a lot of time and discussion to determine how we can fit it into our current system's design and use case. Adding really big features without consulting the team about implementation can occasionally lead to some problems, as that puts the requirement on the core team to help maintain new code while they develop the rest of the system. Additionally, some really big features require changing the way the system works to accommodate it. Communication is key here.

### 4. Is it a feature that currently fits into our design?

Occasionally, someone will make a really great pull request for something, and it might not be something we're sure about including right away. Sometimes there are privacy implications that we have to think about, and other times we have to consider how a new feature would interact with the stream. Occasionally, we'll get a pull request that we don't think really fits into the main codebase because of problems that would arise from this. 

That doesn't mean that we don't think your feature is great, or that you shouldn't continue developing it. It just means that perhaps your feature isn't something we can incorporate at the very moment. To be clear: just because we reject a pull request once doesn't mean that we won't ever consider it in the future.