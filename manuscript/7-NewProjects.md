# Measuring Code Quality and Reporting Progress

Measuring code gives you visibility and helps you set clear objectives. As put by Deming:

{icon=quote-left}
G> *“Without data you’re just another person with an opinion.”*
G>
G> \- *- W. Edwards Deming*

But, before we measure anything, we need to know that metrics are very powerful tools. If these tools are used in the wrong direction, they drive negative behaviors, demotivate individuals, and undermine team values. So, first, we need to agree on the purpose of measuring code.

## Why measuring code?

**Code metrics should only be used for enhancing code, not for anything else.** You should never use them to judge personal capabilities, individual performance, or team productivity, especially if the team is enhancing poor code with already lots of problems. Using code metrics in evaluation and ranking people and teams creates a very unsafe environment and drives negative behaviors. Eventually, you may get better code metrics, but less maintainable code:

{icon=quote-left}
G> *“People with targets and jobs dependent upon meeting them will probably meet the targets – even if they have to destroy the enterprise to do it.”*
G>
G> \- *- W. Edwards Deming*

One example for that is the code size. As a general rule, you should keep your code base simple and remove any unnecessary code. If you reward the behavior of reducing code size (or punish the otherwise behavior), probably you'll get code like this first code sample below, which is three line of code, instead of the second self-explanatory code sample, which is fourteen lines of code:

{lang="java",linenos=on}
~~~~~~~~
public double getSalary(Employee employee) {
		return employee.getBasicSalary() + employee.getChildren().count * (employee.getBasicSalary() * utils.getEduAllownacePer(employee)) + utils.getTransportationFees(employee.getAddress());
}
~~~~~~~~

Instead of this one:

{lang="java",linenos=on}
~~~~~~~~
public double getSalary(Employee employee) {
  double basicSalary = employee.getBasicSalary();
  double educationAllowance = getEducationAllowance(employee);
  double transportationAllowance = utils.getTransportationFees(employee.getAddress());

  double totalSalary = basicSalary + educationAllowance + transportationAllowance;
  return totalSalary;
}

private double getEducationAllowance(Employee employee){
  int numberOfChildren = employee.getChildren().count;
  double allowancePercentage = utils.getEduAllownacePer(employee);
  return numberOfChildren * (employee.getBasicSalary() * allowancePercentage);
}
~~~~~~~~

This behavior is widespread, and appears in every single organization. This was noted by Eli Goldratt, the father of Theory of Constraints, who said: "Tell me how you measure me, and I will tell you how I will behave!"

A> #### My story with peer reviews
A>
A> A few years ago, I led the product development of an business process management suite. My main responsibilities were technical design, supervision and mentorship. I used to do code review for team members and record what we called "review issues". Recording issues was very healthy because every now and then we used to collect similar issues, think about their root causes, and take prevention actions which may stop them from re-occurring.
A>
A> Everything was ok till the organization designed the employee appraisal system. One of the elements affecting this complicated formula was the number of review issues opened on the person's work. After this point of time, I almost stopped reporting review issues on the issue tracking system. Instead, I was leaving my notes on a piece of paper with the person, or sending them by email.
A>
A> Although this broke the continuous improvement cycle I described above, my Inner self convinced me that these issues were very minor and not worth the time of reporting them on the issue tracking, especially if these issues would negatively impact my colleagues' evaluation!
A>
A> This is an example of the negative subtle effect of metrics in organizations when used for personal evaluation. You may not detect their downsides except after they do the damage for the organization.

## Useful code metrics

Code metrics are useful when they indicate the amount of **code smells** you have in code. It's not enough to measure code, what's important is the kind of smell these metrics detect or surface.

The idea of tracing code metrics to code smells is a fundamentals and crucial issue in code refactoring. Because you may become very easily overwhelmed with the amount of metrics about your code, while you only need a couple of them in this stage in refactoring.

For example, if your code has lots of dead code, you may concentrate on code size and nothing else. If you have so many conditionals, you may focus on the cyclomatic complexity and nothing else. If you are covering code with tests, you may focus more on code coverage, and probably one or types of coverage metrics only.

The next table summarizes some important code metrics, when they may be important, and what code smell they may uncover:

*TABLE 2. A listing of useful code metrics*

|Metric     |Related code smells|Used in stage|
|-------------------------------------------------------|
|Code size            | Lines of code with or without comments. Number of statements or number of lines.       |        |
|Method length        |    |     |
|Duplication level |      |        |
|Cyclomatic complexity   |   |   |
|Coverage   |   |   |
|Build time   |   |   |

#### Code size

Lines of code with or without comments. Number of statements or number of lines.

#### Methods with LOC > 10

#### Duplication level

#### Cyclomatic complexity

#### Coverage

Which coverage?

* Code coverage
* Path coverage
* Branch coverage

#### Build time

## Making sense of code measures

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
