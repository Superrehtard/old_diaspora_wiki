Understanding the codebase
==========================

*This page assumes a knowledge of Rails.*

The Diaspora codebase is quite large, and the model relationships and the usage of a separate lib/ directory for holding some queries and other classes used by the system make it quite complex as well.

This is aims to be a guide to the codebase so that you can find what you're looking for, make your changes, and fix bugs quicker than you would just feeling around.

1. *Models*: The main post model is called Post. The 