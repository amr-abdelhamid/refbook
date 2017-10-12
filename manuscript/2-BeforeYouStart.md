
{mainmatter}

# Before You Start - Prepare a Healthy Environment {#beforeYouStart}

## Workitem tracking

Workitem tracking is an important topic in configuration management and a basic constituent in a healthy agile environment. However, many agile teams may not have a clear idea about what workitem types and workflows they should track, what state information they should collect, what kind of relations to be maintained between workitems.

One sign of a mature agile team is that they start creating new workitem types and adjust their workflows to suite their specific needs. They also use it not only to track their everyday development activities, but also to generate rich progress reports, build traceability networks, do impact analysis, etc.

## End-to-end traceability

Traceability is a dynamic network of relationships which you can use to trace or relate software artifacts to each other:

![Example traceability network which should possible links between a group of workitems](\images\traceability.png)

As you can see, any artifact can be related to so many other concepts, artifacts, or workitems. A user story can trace to many other workitems (white), trace to other information (light blue), or trace to physical artifacts (gray).

Traceability enables the team to study the impact of a change and assess its costs, risks, and root-causes. In the scenario below, a defect reported by a customer is traced back to the defected code. This code is traced back to the reason of its existence, which is found to be a change requested by the customer sometime ago. In turn, more information about the change request can be deduced:

![Defected code is traced back to the reason of change and hence the root cause behind the defect](\images\root-cause-analysis.png)

This is particularly important for a team trying to refactor and enhance existing code. The team should be able to distinguish regression bugs resulting from refactoring old code from other bugs resulting from developing new features or fixing bugs.

## Continuous integration

A Continuous Integration (CI) server is the companion of the development team. There are so many repetitive tasks which can be automated one way or another and a CI server will relieve the team pains of running and monitoring such tasks. As a result, the team will keep their focus on development and refactoring efforts.

## Starting with characterization (aka pin-down) tests

Micheal Feathers first introduced the idea of *characterization tests*, which are tests "that characterize the actual behavior of a piece of code" [18]. Pin-down or characterization tests are particularly useful in case of maintaining legacy code which you have little information about its behavior. Adding some tests around the area you're refactoring helps you understand and preserve the actual behavior of the code. Here are some examples for tests that you may consider:

* Who is authorized to do what?
* Does it work with null parameters?
* Should objects be initialized?
* What possible values which will not throw errors?
* What results or return values are expected?
* Does this combination of parameters raise an error?

While you're introducing these tests, you may find weird behavior or even bugs. In this case, be cautious when introducing changes to the actual behavior of the system, or better wait until you have good understanding to it.

## Ground rules for sustainable refactoring {#ground-rules}

Before you start, this is a final step in preparing a healthy refactoring environment: to agree on this set of ground rules. These ground rules are necessary to alleviate some of the issues discussed earlier in the ['Why refactoring fails?'](#whyrefactoringfails) section:

* Refactorings are committed daily on the development mainline, not a dedicated branch
* Timebox an agreed upon percentage of the development effort to refactoring
* Refactoring effort and outcome should be visible to everybody, including management
* Any change *must* be reviewed. This can be done by pair/mob programming the change or peer review it later on
* Large refactorings are not allowed. Example large refactorings is to introduce an architectural enhancement which may require changes in many places in code. These are two rules of thumb to detect whether or not a refactoring is large:
  * Think a lot before you start working on it
  * Take more than 10-30 minutes to get things running
