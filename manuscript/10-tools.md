# Tools You Cannot Do Without

As a general rule for all refactoring effort, the more you rely on toolset to carry out the refactoring the safer you are. Not only it will save you a lot of effort chasing and fixing manual errors and unseen side effects, but it will help you see opportunities of refactoring which you may never think of.

In this chapter, I will list some tools which I used and found useful to carry out refactorings described in this book. While these tools may have already been mentioned before in the book, I still see value in listing all tools in one place as a reference, and keep updating in this every now and then.

I'm pretty sure that there are tens of other very useful tools which I've never heard about. So, don't limit yourself to this set of tools. They are just examples!

### Code measurement

In any improvement activity, measurement is everything, and refactoring is no exception. If you don't measure, it's like traveling in the deserts without a compass. May be you'll travel very far, but no guarantee you're on the right direction.

**[Eclipse Metrics plugin](https://sourceforge.net/projects/metrics/)**


This is a very useful eclipse plugin for java-based projects. I always use it for several purposes:

* Count the total lines of code
* Pinpoint lengthy methods, which usually accumulate large technical debt and are first candidates for refactoring
* Get a feeling about coupling between classes and packages in the system. This is just a high level view. Later on, you may use more tools to better guide you while breaking the system apart and reducing coupling between components

**[Nitriq code analysis](http://www.nitriq.com/)**


This is another excellent tool. Nitriq uses the familiar LINQ query language to extract metrics from your code. For example:

{lang="SQL", line-numbers=off}
~~~~~~~~
from m in Methods
where m.Calls.Contains(m)
select new { m.MethodId, m.Name, m.FullName };
~~~~~~~~

Which lists all recursive methods in your code! I have used Nitriq to measure some very interesting metrics. For example, measure the amount of business logic lines of code and rule out all auto-generated and UI code. Another example is to count all the public methods which are neither constructors nor setters or getters.

### Dead code - Dynamic code analysis

Tools in this category can monitor a live application running in a test or production environment and build a production code usage report exactly similar to the test coverage report.

**[Clover](https://www.atlassian.com/software/clover)**


Clover is typically used for test code coverage. However, it can be used for dynamic code analysis.

**[Coverband](https://github.com/danmayer/coverband)**


For Ruby on Rails, Coverband is a nice and easy tool. It produces output which is [SimpleCov](https://github.com/colszowka/simplecov) compatible and thus can be formatted the same way as test coverage reports produced by SimpleCov.

**[OpenCover](https://github.com/OpenCover/opencover)**


For .Net applications you can use OpenCover for code usage analysis[^opencoverblog].

[^opencoverblog]: This blog post: [https://fuqua.io/blog/2016/08/finding-dead-csharp-code-in-aspnet/](https://fuqua.io/blog/2016/08/finding-dead-csharp-code-in-aspnet/) describes step by step how to do dynamic dead code detection using OpenCover. Accessed May 13, 2018.
