
{mainmatter}

# Before You Start - Prepare a Healthy Environment {#beforeYouStart}

## Workitem tracking

< under development >

## End-to-end traceability

< under development >

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
