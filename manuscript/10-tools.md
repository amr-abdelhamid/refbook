# Tools You Cannot Do Without

As a general rule for all refactoring effort, the more you rely on toolset to carry out the refactoring the safer you are. Not only it will save you a lot of effort chasing and fixing manual errors and unseen side effects, but it will help you see opportunities of refactoring which you may never think of.

In this chapter, I will list some tools which I used and found useful to carry out refactorings described in this book. While these tools may have already been mentioned before in the book, I still see value in listing all tools in one place as a reference, and keep updating in this every now and then.

I'm pretty sure that there are tens of other very useful tools which I've never heard about. So, don't limit yourself to this set of tools. They are just examples!

#### Code measurement

In any improvement activity, measurement is everything, and refactoring is no exception. If you don't measure, it's like traveling in the deserts without a compass. May be you'll travel very far, but no guarantee you're on the right direction.

* [Eclipse Metrics plugin](https://sourceforge.net/projects/metrics/) is very useful plugin for java-based projects.

| |

* Another excellent tool I used is [Nitriq code analysis](). Nitriq uses the familiar LINQ query language to extract metrics from your code. Something like:

        {lang="SQL"}
				~~~~~~~~
        from m in Methods
        where m.Calls.Contains(m)
        select new { m.MethodId, m.Name, m.FullName };
				~~~~~~~~

    This lists all recursive methods in your code! I have used Nitriq measure some very interesting metrics. For example, measure the amount of business logic lines of code and rule out all auto-generated and UI code. Another example is to count all the public methods which are neither constructors nor setters or getters.

#### Dead code - Dynamic code analysis

* [Clover](https://www.atlassian.com/software/clover) for dynamic code analysis. This can listen to the running code and build a coverage report exactly similar to the test coverage report.

| |

* For Ruby on Rails, there is a nice tool called [Coverband](https://github.com/danmayer/coverband), which does the same thing, and uses the coverage tool of Rspec to do the job.

| |

* For .Net applications, you can also do it using [OpenCover](https://github.com/OpenCover/opencover) [^opencoverblog]

[^opencoverblog]: This blog post: https://fuqua.io/blog/2016/08/finding-dead-csharp-code-in-aspnet/ describes step by step how you can do dynamic dead code detection using OpenCover. Accessed Mat 13, 2018.
