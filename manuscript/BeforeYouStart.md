
{mainmatter}

# Before You Start - Prepare a Healthy Environment {#beforeYouStart}

## Workitem tracking

Workitem tracking is an important topic in Configuration Management and a basic constituent in a healthy agile environment. However, many agile team may not have a clear idea about what workitem types they should track, what state information they should collect, kind of relations to be maintained between workitems, and what workflows for every workitem type should look like.

One sign of a mature agile team is that they start creating new workitem types which suites their own environment and store specific state information about every type. They also use it not only to track their everyday development activities, but also to generate progress reports, build traceability networks, do impact analysis, etc.

## End-to-end traceability

Traceability it is a dynamic network of relationships which you can use to trace or relate software artifacts to each other:

![Example traceability network which should possible links between a group of workitems](\images\traceability.png)

As you can see, any artifact can be related to so many other concepts, artifacts, or workitems. A user story can trace to many other workitems (white), trace to other information (light blue), or trace to physical artifacts (gray).

Traceability enables the team to study the impact of a change and assess its costs, risks, and root-causes. In the scenario below, a defect reported by a customer is traced back to the defected code. This code is traced back to the reason of its existence or change, which is found to be a change requested by the customer sometime ago. Other information about the change request can be deduced:

![](\images\root-cause-analysis.png)

This is particularly important for a team trying to refactor and enhance existing code. The team should be able to distinguish issues resulting from refactoring from other issues resulting from developing new features or fixing bugs.

## Production/staging environment readiness

< under development >

## Continuous integration

< under development >

## Starting with pin-down (aka characterization) tests

< under development >

## Ground rules for sustainable refactoring {#ground-rules}

Before you start, this is a final step in preparing a healthy refactoring environment: to agree on this set of ground rules. These rules are put to account for some of the root causes discussed earlier in ['Why refactoring fails?'](#whyrefactoringfails) section:

* Refactorings are committed daily on the mainline, not a dedicated branch
* Timebox an agreed upon percentage of the development effort to refactoring
* Refactoring effort and outcome should be visible to everybody, including management
* Any change *must* be reviewed. This can be done by either pair/mob programming the change or peer review it later on
* Large refactorings are not allowed. Example large refactorings is to introduce an architectural enhancement which may require changes in multiple places in code. These are some rules of thumb how to detect whether or not a refactoring is large:
  * Think a lot before you start working on it
  * Take more than 10-30 minutes to get things running
