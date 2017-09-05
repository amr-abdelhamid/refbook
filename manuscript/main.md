
{mainmatter}

# Before you start - Prepare a healthy environment {#beforeYouStart}

## Workitem tracking

## End-to-end traceability

## Production/staging environment readiness

## Continuous integration

## Pin-down tests

## Ground rules

Before you start, this is a final step in preparing a healthy refactoring environment: to agree on this set of ground rules. These rules are put to account for some of the root causes discussed earlier in *[Why refactoring fails?](#whyrefactoringfails)* section:

* Refactorings are committed daily on the mainline, not a dedicated branch
* Timebox an agreed upon percentage of the development effort to refactoring
* Refactoring effort and outcome should be visible to everybody, including management
* Any change must be reviewed. This can be done by either pair/mob programming the change or peer review it later on.

# Refactoring Roadmap Overview

Start with high value-add and least risky activities, then work on re-organizing code chunks into components, and finally wrap everything with automated tests. In all stages, automate checks to make sure what is fixed will remain fixed.

![](images/roadmap.jpg)

#### Quick-wins: simple and least risky enhancements

In this early stage of refactoring, we rely heavily on tools to detect and fix issues with code. As appears in the roadmap, the kind of issues we are tackling involve the whole code base. So, when we _remove dead code_ or _remove code duplicates_, we do this for the whole code base, not part of the code.

Working on the whole code base magnifies the impact and signifies the improvement. You may put it this way: It may be better to move the whole code one foot forward, rather than to move part of the code a thousand feet forward.

{icon=bookmark}
G> *It may be better to move the whole code one foot forward, rather than to move part of the code a thousand feet forward.*

From my experience, teams working on the quick wins stage for a while usually start feeling "more confident in enhancing the code and applying refactorings ideas". They also have "better grasp and ownership for the code".

#### Divide & Conquer: Split code into components

After getting rid of most of the fat during the last stage, we gradually start introducing structure into the code. The key idea is to move "similar" code together and giving room for cohesive code to form modules with clear interfaces.

Usually, code is already organized in high level modules. We will keep that and polish the existing modules. However, such modules have usually grown in size and gathered so much functionality in time. It may already be doing so many things for a middle-size code module; part of these things may well belong to another module, whether a new or existing one.

#### Inject quality in: Cover components with automated tests

#### Continuous Inspection

# Quick-wins

## Dead code - the time bomb

{icon=quote-left}
G> *Deleting dead code is not a technical problem; it is a problem of mindset and culture*
G>
G> \- *- Kevlin Henney*

Dead code is the "unnecessary, inoperative code that can be removed without affecting program’s functionality". These include: "functions and sub-programs that are never called, properties that are never read or written, and variables, constants and enumerators that are never referenced, user-defined types that are never used, API declarations that are redundant, and even entire modules and classes that are redundant." [10]

It is fairly intuitive (and was shown empirically) that as code grows in size, it needs more maintenance [2][9]. This can be attributed to three factors:

1. More time needed to analyze code and locate bugs
2. Larger code implies bigger amount of functionality, which, in turn, requires more maintenance
3. Software size has significant influence on quality. That is, as code size increases, code quality decreases. Which, in turn, has significant effect on maintenance cost [9]

### What's evil about dead code?

There are many reasons why dead code is bad. First of all, it increases the code size, and thus, as described above, increases the maintenance effort. Have you ever kept staring at a piece of code trying to understand why it is commented out? Did you or anyone of your teammates wasted hours of work trying to locate a bug in a piece of code which turned out to be dead?

While these are very good arguments, there is another reason which makes removing dead code more compelling. [Fortune magazine tells a story](http://fortune.com/2012/08/02/why-knight-lost-440-million-in-45-minutes/) about Knight Capital Group (KCG), which "nearly blew up the market and lost the firm $440 million in 45 minutes". After investigation, it turned out that the code mistakenly set a flag which enabled the execution of a piece of dead code.

This piece of dead code "had been dead for years, but was awakened by a change to the flag’s value. The zombie apocalypse arrived and the rest is bankruptcy" [5].

### How to detect dead code?

So, there are plenty of ways to detect dead code. It is as put by Kevlin Henney, "Deleting dead code is not a technical problem; it is a problem of mindset and culture." [5]

To help you start, here are ideas how to find dead code:

#### Static analyzers

Static analyzers detects unused code by semantic analysis of static code at compile or assembly time. For example:

![Examples of *unreachable code*. In the first method, code returns before the rest of the code runs. The second is a private method which nobody calls in this class ](images/deadcode/eclipse_unreachablecodeerror.png)

These are also called *Unreachable Code* and there is other long list of programming errors which may result into unreachable code, like:

* Exception handling code for exceptions which can never be thrown
* Unused parameters or local variables
* Unused default code in switch statements, or switch conditions which can never be true
* Objects allocated and probably does some internal construction logic, but the object itself is never used
* Unreachable cases in if/else statements

All these cases are simple and straight forward to catch using compilers and static analyzers. More examples of tools in the [Catalogue of practices and techniques](#catalogue). However, if your program allows for dynamic code changes, reflection, or dynamic loading of libraries and late binding; in such cases, static analyzers may not help.

#### Files not touched for so long

Search for files that has never changed since a while. "There are many reasons code may be stable:

* it’s just right,
* it’s just dead,
* it’s just too scary

but unless you investigate you’ll never know." [5]

#### Dynamic program analysis

Runtime monitoring and dynamic program analysis may be used to rule out parts of the code which are **not** dead code. This effectively reduces the amount of code to be inspected.

The idea is the same as measuring test coverage. In test coverage, tools help you pinpoint lines of code which are *not covered by any test*. In dynamic program coverage, tools help you pinpoint lines of code which are *never run by users*, either because the features themselves are never used, so you may remove them altogether from your product, or the code is just dead.

---

Removing dead code is a quick win by all means. It doesn't take time and gives a big relief for the team. In my experience, teams take no more than 2-3 days removing crap and end up with this feeling of achievement! On average, in this small period of time, teams manage to remove 4% to 7% (and sometimes 10%) or dead code [4].

## Code duplicates - the root of all evil in software!

{icon=quote-left}
G> *Duplication may be the root of all evil in software*
G>
G> \- *- Robert C. Martin*

It is interesting to read what gurus write about code duplication. It is like writing about a plague or a catastrophe which you should avoid by all means.

Andrew Hunt, one of the 17 signatories of the Agile Manifesto, and David Thomas, in their book *"The Pragramtic Programmer"*, have put down several principles for Pragmatic Programming, the first of which is: *Don't Repeat Yourself!*

[SonarQube](https://www.sonarqube.org/), the famous tool for continuous inspection of code quality, lists duplication as one of the *seven deadly sins of developers!*[^Sonar]

[^Sonar]: [Developers' Seven Deadly Sins](https://docs.sonarqube.org/display/HOME/Developers%27+Seven+Deadly+Sins)

Robert C Martin (aka uncle Bob), the famous author, speaker and developer, says that "Duplication may be the root of all evil in software"[^cleancoding]. In another article[^bobarticle], he is no longer hesitant and asserts that "Duplicate code *IS* the root of all evil in software design"

[^cleancoding]: This is mentioned in his famous book: *Clean Code: A Handbook of Agile Software Craftsmanship*

[^bobarticle]: Uncle Bob mentioned this explicitly in his article at infoq: [Robert C. Martin’s Clean Code Tip of the Week #1: An Accidental Doppelgänger in Ruby](http://www.informit.com/articles/article.aspx?p=1313447)

#### What's evil about code duplication

In the introduction, I have mentioned the results of a study about software expenditure. They found that 70 billion of the 100 billion expenditure on software products on a 10-year period were spent on maintenance; and 60% of which is consumed to locate defective code [1]:

![60% of the maintenance effort is spent on locating bugs. That is, debugging and chasing code lines till you finally point to a lines of code and say 'I found the bug'. The remaining 40% are for everything else: Fixing, testing, reviews, integration, system testing, deployment, user Acceptance,...](images/duplicatecode/costoflocatingbugs.png)

This means that duplication magnifies time of locating bugs. If you have a defective piece of code duplicated three times, then it's not enough to find the defect once (this already takes 60% of overall defect handling and resolution time). Rather, you'll need to find each and every copy of this defect anywhere else in the code, which is sometimes simply not possible. what usually happens is that we get an illusion that the bug is fixed upon fixing the first clone, ship the *fixed* software to the customer, who probably become very annoyed and backfire on us that the bug is still there.

#### Why developers copy and paste code?

Well, if code duplication is that evil. Why do we do it all the time? Throughout my career, I noticed developers follow this pattern one way or another: Copy Some code, change it to suite your new requirement, and finally test all changes.

![](images/duplicatecode/copyChangeTestCycle.png)

This is pretty natural. Actually, I myself always followed this pattern and I'm still following it. And, I've been doing excellent work with teams I worked with. So, where is the problem? The problem is that I always do a forth step which is necessary and cannot be neglected, which is refactoring. It's ok to copy and paste code only if you're going to refactor this code later on.

![](images/duplicatecode/copyChangeTestRefactorCycle.png)

Neglecting this step is a fundamental mistake which rightly is one of the "deadly sins of developers", as put by SonarQube.

#### Type of code clones

There are four types of code clones: *Exact, Similar, Gapped, and Semantic*. In the following sections, we will only shed light on each of them to help you detect and remove them mercilessly!

Note: All examples of code clones are detected by [ConQAT](https://www.cqse.eu/en/products/conqat/overview/), a **Con**tinuous **Q**u**a**li**t**y monitoring tool developed by the Technical University of Munich.

**Type 1: Exact Clones**

These are the most straight forward and the easiest to detect type of clones. Here is an example of an exact clone:

![Exact clones: Copies of the code is exactly the same](images/duplicateCode/exactClones.png)

**Type 2: Similar Clones**

Similar clones are more common than exact clones because most probably, when a programmer copies some code, he/she changes or renames some of the variables or parameters names:

![Notice that `Locator` is renamed to `AttributeSetInstance` and `PROPERTY_SEARCHKEY` is renamed to `PROPERTY_DESCRIPTION` ](images/duplicatecode/similarclones.png)

As you can see in the above example, clones are similar, except for some renames of identifiers. Note that the structure of the code is the same, and the positions of the renamed identifiers are all the same.

**Type 3: Gapped Clones (aka inconsistent clones)**

This type of clones are very interesting. It picks exact or similar code with 1-2 change lines of code. These changes are called *Gaps*. Why are they interesting? Because probably they are defects fixed in one location and wasn't fixed in another!

![Two exact clones with only one line change (or gap). With minimal review, one may discover that this was a bug fixed in the left hand clone, and not in the other.](images/duplicatecode/gappedclones.png)

![Two similar clones (with some renames), but also with one gap: `if (criteria.has("fieldName"))` check.](images/duplicatecode/gappedclones-2.png)

In both above examples, you may need to introspect the code before doing anything. It may be a valid case which should only be available in one clone and not the other.

A> #### Dormant Bugs and Gapped Clones
A> *Dormant bugs* are bugs which have lived some time on production before they are discovered. Recent studies found that 30% of bugs are dormant. This is scary, because this indicates that there are other bugs with each and every deployment which is still not detected. You have no idea when they will fire back; you have no idea what would be the side effects [6].
A>
A> Now, think about gapped clones. These are typically probable dormant bugs on production. Another study shows that the percentage of gapped clones in software systems running in large enterprises are 52%. Among these clones, 18% are system faults or defects [7]:
A>
A> ![If there are 100 code clones, 52 of them are gapped clones. If you drill into these gapped clones, you'll find 18% of them are system faults](images/duplicatecode/PercentageOfDefectsInGappedClones.png)
A>
A> This means that if you managed to remove 100 gapped clones, then congratulations! You've removed **18 dormant bugs!**

**Type 4: Semantic clones**

The forth type of clones deals with fragments of code doing the same thing but do not share similar structure. For example, implementing a routine which calculates the factorial of a number, one using for loops and another using recursion. There are lots of efforts in the academia to research whether it is possible to detect type 4 of code clones or not. Till they reach something tangible, we will work on the first three types.

#### Removing code duplicates ####

There are several refactoring techniques for removing duplicate code. The safest and most straight forward technique is to 'Extract Method', and point all duplicates to it. This is relatively a safe refactoring specially if you rely on tool support to automatically extract methods.

In all projects that I've worked on, we were very cautious while removing duplicates. These are several pre-cautions to keep in mind:

* Rely on automatic refactoring capabilities in IDE's to extract methods. Sometime, it is the most obvious mistakes which you may spend hours trying to discover. Relying on automatic refactoring support will reduce or even eliminate such mistakes.
* Any change, what so ever, must be reviewed.

Keeping these two pre-cautions in mind will save you, especially that we are refactoring on the mainline, not on a separate long living branch. More on this in this previous chapter on [how to prepare a healthy environment](#beforeYouStart) section.

## Reduce method size

{icon=quote-left}
G> *Refactoring: A change made to the internal structure of software to make it easier to understand and cheaper to modify without changing its existing behavior.*
G>
G> \- *- Martin Fowler [8]*

One thing I like about this definition is the clearly-stated objectives of refactoring: to the make software:

1. Easier to understand
2. Cheaper to modify

Having these two objectives in mind, it's possible to develop your "gut feeling" about the correct length of a method.

Let's agree that a method is *maintainable* when it fulfills these two criteria: understandability and modifiability, and need no further refactoring;  Consider this method and try to evaluate how *maintainable* it is. To help you do that, start a stopwatch and measure the time to understand the intent of the method code lines.

{lang="java"}
~~~~~~~~
public List criteriaFind(String criteria) {
  if (criteria == null)
    criteria = "";

  List criteriaList = scanCriteria(criteria);
  List result = new ArrayList();
  Iterator dataIterator = getDataCash().iterator();
  Iterator criteriaIterator = null;
  DataInfo currentRecord = null;
  List currentCriterion = null;
  boolean matching = true;

  while (dataIterator.hasNext() && !interrupted) {
    currentRecord = (DataInfo) dataIterator.next();

    criteriaIterator = criteriaList.iterator();
    while (criteriaIterator.hasNext() && !interrupted) {
      currentCriterion = (List) criteriaIterator.next();
      if (!currentRecord.contains((String) currentCriterion.get(0),
        (String) currentCriterion.get(1))) {
        matching = false;
        break;
      }
    }
    if (matching)
      result.add(currentRecord);
    else
      matching = true;
  }
  if (interrupted) {
    interrupted = false;
    result.clear();
  }
  Collections.sort(result);
  return result;
}
~~~~~~~~

This is a 28-line method. It seems to be a small method. However, you've spent some time (probably around 1-2 minutes) to grasp how the code works. So, according to our definition, Is this method *maintainable*? The answer is no.

Now, consider this enhanced version of the method:

{lang="java"}
~~~~~~~~
public List criteriaFind(String criteria) {
  if (criteria == null)
    criteria = "";

  // convert the criteria to ordered pairs of field/value arrays.
  List criteriaList = scanCriteria(criteria);
  List result = new ArrayList();

  // search for records which satisfies all the criteria.
  Iterator dataIterator = getDataCash().iterator();
  Iterator criteriaIterator = null;
  DataInfo currentRecord = null;
  List currentCriterion = null;
  boolean matching = true;

  while (dataIterator.hasNext() && !interrupted) {
    currentRecord = (DataInfo) dataIterator.next();

    // loop on the criteria; if any criterion is not fulfilled
    // set matching to false and break the loop immediately.
    criteriaIterator = criteriaList.iterator();
    while (criteriaIterator.hasNext() && !interrupted) {
      currentCriterion = (List) criteriaIterator.next();
      if (!currentRecord.contains((String) currentCriterion.get(0),
        (String) currentCriterion.get(1))) {
        matching = false;
        break;
      }
    }
    if (matching)
      result.add(currentRecord);
    else
      matching = true;
  }

  // clear results if user interrupted search
  if (interrupted) {
    interrupted = false;
    result.clear();
  }

  // Sort Results
  Collections.sort(result);
  return result;
}
~~~~~~~~

Adding some comments makes the method somehow more readable. Sometimes, they make it harder to read the code because it overloads the code with more information. But, for this example, it's a bit better.

But, wait a minute. Why are we adding comments? To make the code more readable, right? which indicates that the code is not maintainable, according to our definition. Actually, this is why *explanatory comments* are generally considered a code smell, or a sign of bad code.

Now, let's work on this method. If you notice, comments are placed at perfect places. They give you a hint of the *Boundaries of Logical Units* inside the method. Such logical units are functionally cohesive and are candidate to become standalone methods. Not only that, the comment itself is a perfect starting point for naming of the newly born method.

So, by extracting each chunk into a standalone method, we will reach this version of the method:

{lang="java"}
~~~~~~~~
public List criteriaFind(String criteria) {
  List criteriaList = convertCriteriaToOrderedPairsOfFieldValueArrays(criteria);
  List result = searchForRecordsWhichSatisfiesAllCriteria(criteriaList);
  clearResultsIfUserInterruptsSearch(result);
  sortResults(result);
  return result;
}
~~~~~~~~

This is a 5-line method which narrates a story. No need to write comments or explain anything. It is self-explanatory and much easier now to instantly capture the intent of the code.

I have done this experiment before, to give the three variants of the method above, without comments, with comments, and refactored into small method. I have measured the time it takes a person to understand the intent of the method. Results were as follows:

* Method without comments: ~ 2 minutes
* Method with comments: ~ 1 minute
* Refactored short method: ~ 10 seconds

It is stunning How much time you save by just reducing methods into smaller size with readable method names. It hits the hard of the refactoring effort: to make the code *"easier to understand and cheaper to modify"*.

A> #### Logical units of code
A>
A> Notice that the original form of the `criteriaFind` method in the above example is functionally cohesive and follows the Single Responsibility Principle (SRP) in a perfect way. However, if you look inside the method, you may notice what I call *Logical Units*, which are single steps in the logic of execution; each step doesn't implement the full job, but still implements a conceivable part towards this goal.
A>
A> Examples of logical units may be an if statement validating a business condition, a for loop doing a batch job on a group of data records, a query statement which retrieves some data from the database, several statements populating data fields on a new form, etc. In my experience, sometimes the logical unit are as small as two or three lines of code. More frequently, they are bigger (like 5 to 12 lines). On very rare occasions I see logical units which are bigger than that.
A>
A> Such logical units are perfect candidates to be extracted into *private* methods. If you adopt this practice for a while, you'll start noticing some private methods which are similar in nature or shares the same "interest". In such case, you may extract and group them into a new logical component. More about this in the [Divide and Conquer](#DivideAndConquer) stage.

## Enhance identifier naming

{icon=quote-left}
G> *You know you are working on clean code when each routine you read turns out to be pretty much what you expected[^clean-ron]*
G>
G> /- *- Ron Jeffries*

Identifiers constitute 70% of the characters of your program.

A> #### Explanatory methods and fields

-- Aside for explanatory methods and fields

[^clean-ron]: Quoted in *Leading Lean Software Development: Results Are not the Point*, by Mary and Tom Poppendieck.

## Considerations related to the quick-wins stage

#### Reliance on tools support

One important consideration in this stage is that **no manual refactoring is allowed!**. Detecting dead code, detecting and removing code clones, extracting methods to reduce method size, renaming identifier names; you can do all such activities with the assistance of strong IDE features or add-on tools.

Using automated refactoring tools contributes to safety and makes developers more confident when dealing with poor and cluttered code

### Are these refactorings safe?

### Should we do them in order?

Yes, with little bit of overlap. This is logical and practical. For example, removing dead code, removes about 10% of your code duplicates[^deadcode].

[^deadcode]: This was validated in one of our experiments. We found that removing dead code removes also 10% of duplicate code [4]. This is totally reasonable, because a good portion of duplicated code are duplicated and then abandoned.

Another example is working on reducing method size before removing duplicates. This actually is a bad practice. Because, you may split a method apart while it is actually a duplicate of another. In this case, you have lost this similarity and may not be able to detect this duplication anymore.

### How to determine whether or not we are done?

# Divide & Conquer {#DivideAndConquer}

Software design is all about components and their relationships. The better you divide your software into loosely-coupled and highly-cohesive parts, the more agile and the more responsive to change your software design becomes.

So, the goal of this stage is to enhance the organization of software and discover and polish components and their interfaces:

![](\images\introduce_structure.jpg)

## Modules, components, or services?

First, we need to answer this question: Are we splitting our code into modules, components, or services? Before we do that, let's agree on what a module, component, and service is.

#### Module

The *UML User Guide* provided a very brief and broad definition of what a module is. It is a *"software unit of storage and manipulation"*.

To elaborate on this definition, a module is any logical grouping of cohesive code functions which provides access to these functions in a uniform manner. This can be as big as a sub-system, like an accounting or HR module, or as small as a class, like a calculator or an xml parser.

#### Component

{icon=quote-left}
G> *“A component is a physical and replaceable part of a system that conforms to and provides the realization of a set of interfaces. It is intended to be easily substitutable for other components that meet the same specifications.”*
G>
G> \– *- The UML User Guide*

From this definition, we understand that a component is a physical standalone file; a jar, war, dll, gem, etc. Also, it is replaceable, meaning that it can be deployed/redeployed on its own. Also, we understand that a component may contain one or more smaller modules; and vice versa, a big module may contain one or more components.

#### Service

In essence, web services (or just services), are components. They are physical, replaceable, provides clear interfaces, and easily substitutable. However, there is value in differentiating services from components.

{icon=quote-left}
G> *“A service is similar to a component in that it's used by foreign applications. The main difference is that I expect a component to be used locally (think jar file, assembly, dll, or a source import). A service will be used remotely through some remote interface, either synchronous or asynchronous (eg web service, messaging system, RPC, or socket.)”[^fowler_article]*
G>
G> \– *- Martin Fowler*

[^fowler_article]: This quote is from Martin's article: [Inversion of Control Containers and the Dependency Injection pattern](https://www.martinfowler.com/articles/injection.html). For other interesting distinctions between components and services, please refer to Martin's famous article: [Microservices: a definition of this new architectural term](https://martinfowler.com/articles/microservices.html)

One very important difference between a component and a service is that a component cannot run on its own. It has to be integrated in a bigger whole to achieve any value out of it. Unlike a service, which is available and standalone. It can be located and used whether on its own or as part of a bigger application.

---

So, what's the value of outlining these differences between modules, components, and services?

As you may noticed from the past definitions, there are higher level of decoupling and increased formality in defining interfaces when moving from modules to components to services.

Modules may co-exist in the same (physical) deployable package; unlike components or services, which are physically standalone.

Modules and components run typically in the same process; unlike services, which runs every service in its own process. Modules and components may communicate through in-memory method calls, while services may require inter-process communication through web-service requests or remote procedure calls.

So, Services enjoys the maximum level of decoupling. You can view them as standalone applications which could be glued together in order to provide greater value for some end user.

Now, the questions is: *Do we need to divide our code into modules or components or services?* The only affirmative answer that I can provide is that: Divide the code into modules. Then, assess whether or not it is useful and safe to upgrade them to components or services.

{icon=bookmark}
G> *Divide the code into modules. Then, assess whether or not it is **useful AND safe** to upgrade them to components or services.*

A> ## What about Microservices?
The main difference between Web-services (or just services), which what I referenced in the past discussion, is that each microservice has a separate standalone datastore. Services, on the other hand, share a common datastore.
A>
A> When looking from the angle of refactoring legacy or monolithic application code bases, it is not feasible to exert any effort or even think about splitting a large backend database into smaller ones and move towards a microservices architecture. Some other challenges looms in the way like handling distributed transactions and understanding and supporting the call chain for every business transaction[^nealford].
A>
A> This is why I have intentionally omitted microservices as one of the options to which one can upgrade newly-born components. In stead, a middle-way is a course-grained service, following a service-based architecture of some kind.

[^nealford]: More discussion about why microservice architecture is not suitable when refactoring monolithic applications is at this excellent talk by Neal Ford: [Comparing service-based architectures](https://vimeo.com/163918385).

## Componentization - Moving from spaghetti/cluttered code to components

The journey from cluttered code to module, components, or services is progressive and multi-stage. Code with large amount of technical debt usually looks like this figure. No clear boundaries between modules, high level glimpses of module interfaces, unstructured or unbounded module communication, etc.

![](images/divideandconquer/modules-unstructured.png)

Gradually, we start moving methods and classes around to let modules emerge and become more apparent. Remember, only safe refactoring with support of an automated refactoring tools are allowed. In most of the cases, you can depend on the following refactorings:

* Move Method
* Move Class
* Rename
* Extract Class/Interface
* Extract Method

This results in clearer module boundaries and better manifestation of module interfaces.

![](images/divideandconquer/modules-structured.png)

Next, we should concentrate on more decoupling modules and create a solo-deployable components. At this stage, we should work more on polishing interfaces, move away un-needed interface methods and interface parameters. Some useful refactorings at this stage are:

* Change Method Signature (to remove or reorder method parameters)
* Introduce Parameter or Parameter Object
* Turn Public Methods Private

![](images/divideandconquer/components.png)

You may stop at this stage. Or, you move to the next step and turn components into services. Remind you that you may chose to do so only if you find it *valuable and safe*.

![](images/divideandconquer/services.png)

## Types of software components

## Guiding design principles

# Inject Quality In

The final stage in the roadmap is to cover components with unit tests and create what is called ‘trusted code regions’.

## Which type of tests?

There are several types of automated developer tests. The following diagram is a typical distribution of automated tests for a “healthy” product:

![Distribution of automated tests](images/test_types.png)

In case of legacy application with poor code structure, coding unit tests on method level while mocking/faking everything else would have very little ROI and would take so much time and effort before the team feels any value.

Instead, at this stage, we will concentrate on component, integration, and system tests. These are the 20% of tests which will realize 80% of the value. Also, there are some other reasons which makes such higher-level tests more appealing:

1. In the previous stage (divide & conquer), we have already prepared component interfaces, and they became ready for getting covered by tests
2. Component tests create what is called ‘trusted code regions’, and divides the overall complexity of testing among components
3. Still, the internal complexity of the component code is still high. Remember that we refrained from doing any risky refactorings so far. This is why unit tests may not be feasible at this stage

## Tracking coverage

# Continuous inspection throughout the roadmap

## Why continuous inspection is important?

## Which conventions should be put under continuous inspection?

## Example: Code clones continuous inspection using Jenkins and ConQAT

# Starting a new project? Important considerations
