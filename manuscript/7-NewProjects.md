# Measuring Code Quality and Reporting Progress

Measuring code gives you visibility and helps you set clear objectives. As put by Deming:

{icon:quote-left}
B> *“Without data you’re just another person with an opinion.”*
B>
B> \- *- W. Edwards Deming*

But, before we measure anything, we need to know that metrics are very powerful tools. If these tools are used in the wrong direction, they drive negative behaviors, demotivate individuals, and undermine team values. So, first, we need to agree on the purpose of measuring code.

## Why measuring code?

**Code metrics should only be used for enhancing code, not for anything else.** You should never use them to judge personal capabilities, individual performance, or team productivity, especially if the team is enhancing poor code with already lots of problems. Using code metrics in evaluation and ranking people and teams creates a very unsafe environment and drives negative behaviors. Eventually, you may get better code metrics, but less maintainable code:

{icon:quote-left}
B> *“People with targets and jobs dependent upon meeting them will probably meet the targets – even if they have to destroy the enterprise to do it.”*
B>
B> \- *- W. Edwards Deming*

One example for that is the code size. As a general rule, you should keep your code base simple and remove any unnecessary code. If you reward the behavior of reducing code size (or punish the otherwise behavior), probably you'll get code like this first code sample below, which is three line of code, instead of the second self-explanatory code sample, which is fourteen lines of code:

{format: java,line-numbers: true}
```
public double getSalary(Employee employee) {
	return employee.getBasicSalary() + employee.getChildren().count * (employee.getBasicSalary() * utils.getEduAllownacePer(employee)) + utils.getTransFees(employee.getAddress());
}
```

Instead of this one:

{format: java,line-numbers: true}
```
public double getSalary(Employee employee) {
  double basicSalary = employee.getBasicSalary();
  double educationAllowance = getEducationAllowance(employee);
  double transportationAllowance = utils.getTransFees(employee.getAddress());

  double totalSalary = basicSalary + educationAllowance + transportationAllowance;
  return totalSalary;
}

private double getEducationAllowance(Employee employee){
  int numberOfChildren = employee.getChildren().count;
  double allowancePercentage = utils.getEduAllownacePer(employee);
  return numberOfChildren * (employee.getBasicSalary() * allowancePercentage);
}
```

This behavior is widespread, and appears in every single organization. This was noted by Eli Goldratt, the father of Theory of Constraints, who said:

{icon:quote-left}
B> *“Tell me how you measure me, and I will tell you how I will behave!”*
B>
B> \- *- Eli Goldratt*

A> #### My story with peer reviews
A>
A> A few years ago, I led product development of a business process management suite. My main responsibilities were technical design, supervision, and mentorship. I used to do code reviews for team members and record what we called *review issues* on our issue tracking tool. Recording issues was very healthy because every now and then we used to collect similar issues, think about their root causes, and take prevention actions to stop them from re-occurring.
A>
A> **Everything was ok till the organization designed an employee appraisal system to be held twice a year.** Employee's performance is determined by a complicated formula or many contributing measures, and one of these measures was the number of review issues opened on the person's work! After this point of time, I almost stopped reporting review issues on the issue tracking system. Instead, I was leaving my notes on a piece of paper with the person, or sending them by email.
A>
A> Although this broke the continuous improvement cycle I described above, my Inner self convinced me that these issues were very minor and not worth the time of reporting them on the issue tracking, especially if these issues would negatively impact my colleagues' evaluation!
A>
A> **This is an example of the negative subtle effect of metrics in organizations when used for individual evaluation. You may not detect their downsides until the damage is already done in the organization.**

## Useful code metrics

Code metrics are useful when they indicate the amount of **code smells** you have in code. It's not enough to measure code, what's important is the kind of smell these metrics detect or surface.

The idea of tracing code metrics to code smells is a fundamentals and crucial issue in code refactoring. Because you may become very easily overwhelmed with the amount of metrics about your code, while you only need a couple of them in this stage in refactoring.

For example, if your code has lots of dead code, you may concentrate on code size and nothing else. If you have so many conditionals, you may focus on the cyclomatic complexity and nothing else. If you are covering code with tests, you may focus more on code coverage, and probably one or types of coverage metrics only.

The next table summarizes some important code metrics, when they may be important, and what code smell they may expose:

*TABLE 2. A listing of useful code metrics*

{widths: "10,*,10"}
|============|============|============|
| Header A   | Header B   | Header C   |
|============|============|============|
| Content A1 | Content B1 | Content C1 |
|------------|------------|------------|
| Content A2 | Content B2 | Content C2 |
|------------|------------|------------|
| Content A3 | Content B3 | Content C3 |
|============|============|============|

{widths: "10,*,30,10"}
|=======|================================|=================|====================|
|Metric |Description                     |Usage            |Related code smells |
|=======|================================|=================|====================|
|Code size |Can be measures either in lines of code or number of statements. Lines of code excludes whitespace and preferably excludes comments. Number of statements is a better metric because it is not affected by grouping multiple statements on the same line.|Used throughout the product lifecycle. However, in case of refactoring poor legacy code, we target to reduce this metric till it reaches a stable lower limit. |Large methods. Large Classes. Unused code. Unnecessary code. Extra features.|
|----|-----|------------|-----|
|Methods with size > 10 LOC |Lengthy methods is a sign of poor code. When a method exceed the threshold of 10 lines of code, most probably they have violation the Single Responsibility Principle (SRP). Also, methods are no longer self explanatory and much less maintainable accordingly. It results in multitudes of problems just because of the lengthy methods.   |Should be controlled throughout the project. However, it is so much needed in the Quick-wins Stage and specifically in the step: Reducing method size. |Big Methods. Too many conditionals.
|Duplication level |% of code duplicated. There are several ways to calculate this number. The idea is to use the same tool and the same set of parameters every time. Basically, this measure takes into account exact and similar clones only.       |Used heavily in the quick-wins stage. We rely on it to assess whether we need to continue working on *removing code duplicates* or not.       |Duplication is the enemy of clean code
|Cyclomatic complexity (CC)  |Cyclomatic complexity is an indicator of how execution paths one method has. The more execution paths, the more logic and complexity the method contains.   |Mainly, it's used during the quick-wins to pinpoint big and complex methods which needs to be refactored. Usually, you may find that CC and method length are both high. So, sometimes I prefer to look at the method length first before the CC  |Long Method. Too many conditionals. Switch statement |
|Code Coverage   |% of code covered by automated tests.   |Used mainly in the *Inject Quality In* stage.    |It helps identify part of code which are not covered by any test. |
|Build time   |Time elapsed to build, package and deploy the product on the target environment.   |This number is used throughout the project lifecycle. First, it may be several days of manual effort to package, test, and deploy. The target is to reach a less-than-an-hour process end-to-end   |Long build time. Manual repetitive work.  |
|Method parameters   |number of parameters in methods.  |Used in the *Divide and Conquer* stage while reviewing and improving the components interfaces.    |Long Parameter List. |
|Class coupling   |A measure of how many calls back and forth between two classes or packages. It pinpoints high coupling between classes and packages.   |This measure is used mainly in the *Divide and Conquer* stage. It guides you while resolving dependencies and reducing overall coupling in your code   | Feature Envy. Inappropriate Intimacy. Middle Man. |

## Making sense of code metrics

**A metric is not an indicator**. It doesn't indicate anything. What if I told you that your code size is 1.5 million lines of code. Is this good or bad? Is it a big or small number? No body knows, and it is incorrect to start reasoning using single readings of any metric.

To make sense of code metrics, you need to do other things like:

{widths: "30,70"}

|Type of indicators |Examples  |
|-------------------|-----------|
|**Compare metrics to known benchmarks** |Methods size is bad when it exceeds 10 LOC|
|**Compare metrics to desirable targets** |Code coverage should be 100% |
|**Relate metrics to each other** | Build time relation to module code size |
|**Show the metric trend over time** | Burn down for the level of duplication over time|

This is the difference between a metric and an indicator. A metric is just a number, while the indicator is a graph which tells a story around these numbers.


# Starting a New Project? Important Considerations

#### Start with your production environment ready!

One of the most important things you should care about when you start a new project is to get your production environment (or at least a staging one) ready from day one!

Previously, we used to code then deploy. Just before deployment, we start thinking about how we are going to do it. We start answering questions like: Which server? shall we start with a testing environment or create a staging one? Do we need to create a production-like environment or just care about the core services? What would a typical production environment look like in the first place? etc. And usually, we have to wait for some good amount of time till these logistics are sorted out.

In contrary, to start right, flip things around and start by setting up and preparing a production environment, link it to you development environment using proper automatic integration and deployment toolset and scripts.

#### Dedicate 10% of your time to refactoring

Refactoring is not an activity to do when code become cluttered and full of technical debt. Rather, it is a continuous maintenance activity which keeps the code clean and protects it from deterioration.

Usually, 10% of your time is justified and can be sponsored by higher management, especially if they are educated about the concept of technical debt and the deadly cycle of adding features and accumulating technical debt.

#### Setup your continuous integration server to check coding conventions and development guidelines

You may chose to do peer reviews to check coding conventions and development guidelines. However, when code grows in size, it may not be efficient nor effective to do it by peer reviews. What you should do is to automate as much as you can, then do peer reviews to pick defects a machine cannot check or detect.
