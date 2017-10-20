
{mainmatter}

# Before You Start - Prepare a Healthy Environment {#beforeYouStart}

## Work item tracking

Work item tracking (aka issue tracking) is an important topic in configuration management and a basic constituent in a healthy software development environment. However, many software teams may not have a clear idea about what work item types and workflows they should track, what state information they should collect, what kind of relations to be maintained between work items.

One sign of a mature development team is that they start creating new work item types and adjust their workflows to suite their specific needs. They also use it not only to track their everyday development activities, but also to generate rich progress reports, build traceability networks, do impact analysis, etc.

## End-to-end traceability

Traceability is a dynamic network of relationships which you can use to trace or relate software artifacts to each other:

![Example traceability network which should possible links between a group of work items](\images\traceability.png)

As you can see, any artifact can be related to so many other concepts, artifacts, or work items. A user story can trace to many other work items (like tasks of defects), trace to other information (like iteration or release), or trace to physical artifacts (like code or design document).

Traceability enables the team to study the impact of a change and assess its costs, risks, and root-causes. In the scenario below, a defect reported by a customer is traced back to the defected code. This code is traced back to the reason of its existence, which is found to be a change requested by the customer sometime ago. In turn, more information about the change request can be deduced:

![Defected code is traced back to the reason of change and hence the root cause behind the defect](\images\root-cause-analysis.png)

This is particularly important for a team trying to refactor and enhance existing code. The team should be able to distinguish regression bugs resulting from refactoring old code from other bugs resulting from developing new features or fixing bugs.

## Continuous integration

A Continuous Integration (CI) server is the companion of the development team. There are so many repetitive tasks which can be automated one way or another and a CI server will relieve the team pains of running and monitoring such tasks. As a result, the team will keep their focus on development and refactoring efforts.

## Starting with characterization (aka pin-down) tests

Micheal Feathers first introduced the idea of **characterization tests**, which are **tests "that characterize the actual behavior of a piece of code"** [18]. The aim of adding these tests are not to discover bugs, but rather to understand how the system behaves. This is why characterization tests are particularly useful if you are maintaining code which you have little very little experience with and know very little about its behavior. **Adding some tests around the area you're refactoring helps you understand and preserve the actual behavior of the code.**

Here are some examples for tests that you may consider:

* Does calling a specific interface requires logged in user?
* Does it work with null parameters?
* Should objects be initialized?
* What possible values which will not throw errors?
* What return values are expected?
* Does this combination of parameters raise an error?

Adding some of these tests will help you *characterize* the code under investigation and will help you *pin-down* and preserve some of its key behaviors.

While you're introducing these tests, you may find weird behaviors or even bugs. In this case, be cautious when fixing bugs or changing things because you still can't anticipate side effects. Moreover, some bugs might have become featured in the system, and fixing/changing them may not be accepted by end user! My advice is to wait until you have good understanding of the code.

## Ground rules for sustainable refactoring {#ground-rules}

Before you start, this is a final step in preparing a healthy refactoring environment: to agree on this set of ground rules. These ground rules are necessary to alleviate some of the issues discussed earlier in the ['Why refactoring fails?'](#whyrefactoringfails) section:


A> **Rule 1: Refactorings are committed daily on the development mainline, not a dedicated branch.**
A>
A> Maintaining two branches of code is a nightmare. Development teams cannot sustain manually integrating and merging features, fixes, and patches from one branch to the other, especially after applying profound refactorings in one of them.
A>
A> This is why I deliberately advice teams to integrate refactorings on their mainline of development not on a dedicated branch. This is not easy, but it is possible.

A> **Rule 2: Timebox an agreed upon percentage of the development effort to refactoring.**
A>
A> Agree on this in advance. Negotiate this with senior management if necessary. Let refactoring be a development habit rather than an unpleasant mandatory task to do.

A> **Rule 3: Refactoring effort and outcome should be visible to everybody, including management.**
A>
A> This is crucial to keep the momentum and maintain the sponsorship of refactoring as an expensive activity, especially when refactoring old systems with tons of technical debt.

A> **Rule 4: Any change *must* be reviewed.**
A>
A> Review can take place either by pair programming the change, mob programming the change, or peer reviewing it later on.

A> **Rule 5: Large refactorings are not allowed throughout the roadmap.**
A>
A> Example large refactorings is to introduce an architectural enhancement which may require changes in many places in code. These refactorings need special care and must be handled differently.
A>
A> These are two rules of thumb to detect whether or not a refactoring is large:
A>
A> * A refactoring is large if you spend much time thinking and designing before you start working on it
A> * A refactoring is large if it takes more than 20-30 minutes for you to get things up and running
