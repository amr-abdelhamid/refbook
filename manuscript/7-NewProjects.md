# Measuring Code Quality and Reporting Progress

This chapter is still under development. It serves two purposes:

1. How to measure code quality in every stage of the refactoring roadmap.
2. It gives you more insights about how to report progress and show improvement to upper management. 

# Starting a New Project? Important Considerations

#### Start with your production environment ready!

One of the most important things you should care about when you start a new project is to get your production environment (or at least a staging one) ready from day one!

Previously, we used to code then deploy. Just before deployment, we start thinking about how we are going to do it. We start answering questions like: Which server? shall we start with a testing environment or create a staging one? Do we need to create a production-like environment or just care about the core services? What would a typical production environment look like in the first place? etc. And usually, we have to wait for some good amount of time till these logistics are sorted out.

In contrary, to start right, flip things around and start by setting up and preparing a production environment, link it to you development environment using proper automatic integration and deployment scripts and toolset.

#### Dedicate 10% of your time to refactoring

Refactoring is not an activity to do when code become cluttered and full of technical debt. Rather, it is a continuous maintenance activity which keeps the code clean and protects it from deterioration.

Usually, 10% of your time is justified and can be sponsored by higher management, especially if they are educated about the concept of technical debt and the deadly cycle of adding features and accumulating technical debt.

#### Setup your continuous integration server to check coding conventions and development guidelines

You may chose to do peer reviews to check coding conventions and development guidelines. However, when code grows in size, it may not be efficient nor effective to do it by peer reviews. What you should do is to automate as much as you can, then do peer reviews to pick defects a machine cannot check or detect.
