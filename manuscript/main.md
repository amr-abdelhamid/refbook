
{mainmatter}

# Before you start - Prepare a healthy environment

## Workitem tracking

## End-to-end traceability

## Production/staging environment

## Continuous integration

# Refactoring Roadmap Overview

## Quick-wins: simple and least risky enhancements

## Divide & Conquer code into components

## Inject quality in using automated tests

# Quick-wins

## Pin-down tests

## Dead code, the time bomb

{icon=quote-left}
G> *Deleting dead code is not a technical problem; it is a pro8blem of mindset and culture*
G>
G> *\- Kevlin Henney  *

It is fairly intuitive to assume that as code grows in size, it needs more maintenance [2]. This can be attributed to three factors:

1. More time needed to analyze code and locate bugs.
2. Larger code implies bigger amount of functionality, which, in turn, requires more maintenance.
3. There is high correlation between size and complexity of software. In the meanwhile, analysis of maintenance effort of business applications shows that highly complex software incurs more costs in maintenance [3]. Accordingly, we can fairly deduce that size is also correlated with maintenance costs.

### What's evil about dead code?

There are many reasons why dead code is bad. First of all, it increases the code size, and thus, as described above, increases the maintenance effort. Have you ever kept staring at a piece of code trying to understand why it is commented out? Did you or anyone of your teammates wasted hours of work trying to locate a bug in a piece of code which turned out to be dead?

While these are very good arguments, there is another reason which makes removing dead code more compelling. [Fortune magazine tells a story](http://fortune.com/2012/08/02/why-knight-lost-440-million-in-45-minutes/) about Knight Capital Group (KCG), which "nearly blew up the market and lost the firm $440 million in 45 minutes". After investigation, it turned out that the code mistakenly set a flag which enabled the execution of a piece of dead code.

This piece of dead code "had been dead for years, but was awakened by a change to the flag’s value. The zombie apocalypse arrived and the rest is bankruptcy" [5].

### How to detect dead code?

So, there are plenty of ways to detect dead code. It is as put by Kevlin, "Deleting dead code is not a technical problem; it is a problem of mindset and culture."

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

All these cases are simple and straight forward to catch using compilers and static analyzers. More examples of tools in the ![Catalogue of practices and techniques](#catalogue). However, if you program allows for dynamic code changes, reflection, or dynamic loading of libraries and late binding; in such cases, static analyzers may not help.

#### Files not touched for so long

Search for files that has never changed since a while. "There are many reasons code may be stable:
* it’s just right,
* it’s just dead,
* it’s just too scary

but unless you investigate you’ll never know." [5]

#### Dynamic program analysis

Runtime monitoring and Dynamic program analysis may be used to rule out parts of the code which are **not** dead code. This effectively reduces the amount of code under investigation.

The idea is the same as measuring test coverage. In test coverage, tools help you pinpoint lines of code which are *not covered by any test*. In dynamic program coverage, tools help you pinpoint lines of code which are *never run by users*, either because the features themselves are never used, so you may remove them altogether from your product, or the code is just dead.

---

Removing dead code is a quick win by all means. It doesn't take time and gives a big relief for the team. In my experience, teams take no more than 2-3 days removing crap and end up with this feeling of achievement! On average, in this small period of time, teams manage to remove 4% to 7% (and sometimes 10%) or dead code [4].

## Code duplicates, the root of all evil in software

## Reduce method size

## Enhance identifier naming

## Considerations related to the quick-wins stage

### Reliance on tools support

### Are these refactorings safe?

### Should we do them in order?

Yes, with little bit of overlap. This is logical and practical. For example, removing dead code, removes about 10% of your code duplicates [^foot]:This is one of our findings. We noticed that a good portion of duplicated code are duplicated and then abandoned. Refer to [4] for more discussion about our findings.

Another example is working on reducing method size before removing duplicates. This actually is a bad practice. Because, you may split a method apart while it is actually a duplicate of another. In this case, you have lost this similarity and may not be able to detect this duplication anymore.

### How to determine whether or not we are done?

# Divide & Conquer

## Software design is all about components and their relationships

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
