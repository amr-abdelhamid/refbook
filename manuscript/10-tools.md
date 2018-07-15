# Tools of Great Help

As a general rule for all refactoring effort, the more you rely on toolset to carry out the refactoring the safer you are. Not only it will save you a lot of effort chasing and fixing manual errors and unseen side effects, but it will help you see opportunities of refactoring which you may never notice or think about.

In this chapter, I will list some tools which I used and found invaluable while working on refactoring projects. Although these tools may have already been mentioned before in the book, I still see value in listing all tools in one place as a reference, and keep updating this chapter every now and then.

I'm pretty sure that there are tens of other very useful tools which I've never used or even heard about. So, don't limit yourself to this set of tools. These are just examples!

### Calculating code metrics

In any improvement activity, measurement is everything, and refactoring is no exception. If you don't measure, it's like traveling in the deserts without a compass. May be you'll travel very far, but no guarantee you're in the right direction.

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

### Dead code - Static code analysis

There are tons of tools to do static code analysis. I've always used Eclipse and Visual Studio for this purpose. Other tools like SonarQube, FindBugs, and PMD are also very popular. For every other technology, you'll find many tools which embody some interesting code recommendations.

### Dead code - Dynamic code analysis

Tools in this category can monitor a live application running in a test or production environment and build a production code usage report exactly similar to the test coverage report.

**[Clover](https://www.atlassian.com/software/clover)**

Clover is typically used for test code coverage. However, it can be used for dynamic code analysis.

**[Coverband](https://github.com/danmayer/coverband)**

For Ruby on Rails, Coverband is a nice and easy tool. It produces output which is [SimpleCov](https://github.com/colszowka/simplecov) compatible and thus can be formatted the same way as test coverage reports produced by SimpleCov.

**[OpenCover](https://github.com/OpenCover/opencover)**

For .Net applications you can use OpenCover for code usage analysis[^opencoverblog].

[^opencoverblog]: [This blog post](https://fuqua.io/blog/2016/08/finding-dead-csharp-code-in-aspnet/) describes step by step how to do dynamic dead code detection using OpenCover. Accessed May 13, 2018.

### Duplicate code

Discover duplicate code is one of the very important and early steps towards clean code. Without a good tool, your ability to pinpoint and remove duplicates is very minimal.

**[ConQAT](https://www.cqse.eu/en/products/conqat/overview/)**

ConQAT is probably the best tool I've evaluated to detect code clones. It has a very powerful algorithm which detects type 1, 2 and 3 of code clones. It also has a nice plugin to analyze and visualize duplicates on eclipse, even if you're analyzing languages other than java.

The second best thing about ConQAT is that it is extremely powerful when working with large code bases. I have used it to analyze duplicates for a code base of 5 million LOC. Guess what? it took less than 3 minutes to finish!

The only downside of ConQAT is the very limited community using and supporting this tool. Also, ConQAT 2015 is the last released open-source version of the tool.

**[Visual Studio](https://msdn.microsoft.com/en-us/library/hh205279.aspx)**

Visual Studio has a very powerful code detection tool which works live while you're typing! Still there are two downsides I found. The first one is that it only detects exact and similar code clones (type 1 and 2). The second drawback is that it doesn't work for large code bases. I've tried it for 400k lines of code, it took around 35 minutes to complete the code analysis and generate the duplicates list.

**[SonarQube](https://www.sonarqube.org/)**

SonarQube has a basic component for detecting code clones. I found ConQAT much more powerful in detecting clones. However, with the vast documentation and huge user community, sonarqube may become a good choice for enabling continuous detection of clones.

### Structural analysis and componentization

**[ConQAT](https://www.cqse.eu/en/products/conqat/overview/)**

ConQAT help define and test existing dependencies in your code. First, you start with visually creating the blueprint of your components, assign code parts (classes or packages) to them, and finally define access rules. ConQAT help you test whether these rules are really followed and pinpoint any access violations[conqat_blog].

[^conqat_blog]: This is a step by step guide from my blog about [How to detect architectural violations using ConQAT](http://amr-noaman.blogspot.com/2013/04/detect-architectural-violations-using.html). Accessed July 15, 2018.

**[Stan4J](http://stan4j.com/)**

Stan4J does a great job in visualizing dependencies in your code. You can very quickly see dependencies in a nice directed graph with the number of calls put on each arrow:

![Directed graphs of dependencies drawn by Stan4J](\images\dependency-towards-stability.png)

What it does really good, is organizing code in a way which help you resolve violations. So, if there is an **circular dependency**, Stan4J isolates the interacting classes/packages in what is called **Tangles** in a way which minimizes the number of arrows which need to be inverted:

![Notice the circular dependency between `Font` and `FontFactory` in this example.](\images\font-tangle.png)
