# Basic Layout

Every web application that you write will have a different layout, flow of execution, and way of behaving. It can prove helpful to follow some general strategies for organizing code and functionality to help with concerns such as maintainability, performance, scalability, and security.

As far as the average end user is concerned, web applications and web sites have a very simple architecture.

The user strictly sees a program on his computer talking to another computer, which is doing all sorts of things, such as consulting databases, services, and so on. Users largely do not think of browsers and the content they serve up as different things - it is all part of the Internet to them.

As authors of web application software, the initial temptation for us might be to put all of the code for the web application in one placewriting scripts that queried the database for information, print the results for the user, and then do some credit card processing or other things.

While this does have the advantage of being reasonably quick to code, the drawbacks become quickly apparent:

* The code becomes difficult to maintain when we try to change and upgrade the functionality. Instead of finding clear and well-known places for particular pieces of functionality, we have to go through various files to find what needs to be changed.

* The possibilities for code reuse are reduced. For example, there is no clear place where user management takes place; if we wanted to add a new page to do user account maintenance, we would end up duplicating a lot of existing code.

* If we wanted to completely change the way our databases were laid out and move to a new model or schema, we would have to touch all of our files and make large numbers of potentially destabilizing changes.

* Our monolithic program becomes very difficult to analyze in terms of performance and scalability. If one operation in the application is taking an unusually long time, it is challenging \(if not impossible\) to pinpoint what part of our system is causing this problem.

* With all of our functionality in one place, we have limited options for scaling the web application to multiple systems and splitting the various pieces of functionality into different modules.

* With one big pile of code, it is also difficult to analyze its behavior with regards to security. Analyzing the code and identifying potential weak points or tracking down known security problems ends up becoming harder than it needs to be.

Thus, we will choose to use a multi-tired approach for the server portion of our web application, where we split key pieces of functionality into isolatable units. We will use a common "3-layer" approach:

> Application Layer / User Interface &lt;--&gt; Business Logic Layer &lt;--&gt; Data Layer / Database

This architecture provides us with a good balance between modularization of our code for all of the reasons listed previously but does not prove to be so overly modularized that it becomes a problem.

Please note that even though these different modules or tiers are logically separate, they do not have to reside on different computers or different processes within a given computer. In fact, for a vast majority of the samples we provide, these divisions are more logical than anything else. In particular, the first two tiers reside in the same instance of the web server and PHP language engine. The power of web applications lies in the fact that they can be split apart and moved to new machines as needs change, which lets us scale our systems as necessary.

