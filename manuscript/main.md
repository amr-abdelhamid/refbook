
{mainmatter}

# Sustainable Refactoring Roadmap Overview

## Quick-wins: simple and least risky enhancements

## Divide & Conquer code into components

## Inject quality in using automated tests

# Quick-wins

## Dead code, the time bomb

It is fairly intuitive to assume that as code grows in size, it needs more maintenance [2]. This can be attributed to three factors:

1. More time needed to analyze code and locate bugs.
2. Larger code implies bigger amount of functionality, which, in turn, requires more maintenance.
3. There is high correlation between size and complexity of software. In the meanwhile, analysis of maintenance effort of business applications shows that highly complex software incurs more costs in maintenance [3]. This indicates that size may also be correlated with maintenance cost.

## Code duplicates, the root of all evil in software

## Reduce method size & enhance identifier naming

## Considerations related to the quick-wins stage

### Reliance on tools support

### Are these refactorings safe?

### Should we do them in order?

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

## which conventions should be put under continuous inspection

## Example: Code clones continuous inspection using Jenkins and ConQAT
