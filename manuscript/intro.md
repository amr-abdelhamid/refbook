{frontmatter}

# Introduction and Background

## Why am I writing this book?

The story is back in 2009 when I first started helping organizations and coaching teams to adopt Agile. While working with several teams, we have faced lots of problems working with team and organization trying to adopt agile while their code is extremely poor. This was a show stopper, and teams suffered from frequent code failures and intermittent outages. Such code was literally stopping them from transitioning to shorter iterations or even smaller releases.

## Why refactoring matters?

In the 90’s, 70 billion of the 100 billion expenditure on software products are spent on maintenance; and 60% of which is consumed to locate defective code [1]. Using simple algebra, reducing the amount of time to locate defective code by 30% would reduce the overall expenditure on software by 15%, which is huge improvement.

## How to refactor - the "old" way

Over the course of several years, I have struggled with teams to refactor their application code to be easier to understand and cheaper to modify. These attempts followed one or more of the old-way patterns described below. I have to say that most of these attempts failed or achieved very little value.

**Re-write the whole**

This solution looms like the easiest solution from both technical and management point of views. If code is cluttered and causing lots of trouble, trash it and start from scratch. The question is: If you start from scratch, what makes you confident that you'll not hit the same wall again?

> *The question is: If you start from scratch, what makes you confident that you'll not hit the same wall again?*

It's like the next picture of two accidents. Car capabilities are different, one is ordinary sedan car while the other is an expensive sports car. However, the accidents are similar, because the *driving habits* are also similar.

![Bad driving habits may cause very bad accidents, and bad *development habits* may also lead to very poor code with lots of technical debt](images/car_accidents.png)

So, if you would like to start over again and not hit the same wall, you have to change your development habits, rather than change the code. But, why not change development habits while still maintaining the same code? This would definitely save us millions of investment, and this book gives you a roadmap how to do that.

**Technical hero**

**As per the book**

**Try-retry**

---

While these are all good efforts, there has to be a better way. The issue is that such haphazard efforts are usually faced with so many circumstances and risks which may lead to immature results and, sometimes, complications to the original problems.

## Observations from failed attempts to refactoring

I have documented some observations (or rather surprises) from these failures in an experience report published several years ago [4]. This is a summary of the most important ones:

* Covering your code with automated tests is not the first step in refactoring!
* Working on refactoring for a long time is like working on bug fixing for a long time. It simply sucks! There has to be another way which is more sustainable.
* One of the impediments to successful refactoring is the development team's focus on refactoring to patterns. This shifts their focus from making code simpler and understandable to making the code intelligent and "stylish"!
* In many cases, managers do believe that continuous refactoring is necessary to keep the code healthy. However, we noticed that they do not sponsor refactoring effort. Why? The answer is that _managers will not sponsor something they cannot track or control._
* For refactoring to succeed, several qualification should exist in the development environment. Qualifying the environment may take sometime to be done; but once done, refactoring is 90% complete!

## Why refactoring fails?

In the same report [4], I've listed some key factors or reasons why refactoring failed in early experiments:

#### 1. Vague and hazy objectives

Attempts to refactor had hazy and unclear objectives. It was the gut feeling of the engineering staff that determined what to do

#### 2. Covering poor code with fragile tests

We always thought that an automated test suite is a safety net for any side effects or regression issues which refactoring may cause. Without such safety net, most of the time, we didn't have the courage to change the code and integrate it to the mainline. However, in all three projects, this turned out to be not possible due to many factors. For example, product code lacked clear system interfaces and suffered from scattered business logic in all layers, including the database. In other case, writing automating tests incurred very high costs, not only in development, but in toolset and training. In time, I felt that some team members viewed automated tests as the first impediment to refactoring!

#### 3. It's non of the managers' business!

Technical teams had the attitude that refactoring was “none of the managers’ business”. Also, they did not spend any effort to involve busy managers and get their support. This attitude created a counter effect from managers towards refactoring. The refactoring effort was viewed by senior managers as a non-value adding activity and was only allowed due to pressure from the development teams. Once the planned time for refactoring elapsed, management became more and more resistant to spending any more effort on refactoring.

> *Managers will not sponsor something they cannot track or control.*

#### 1. The Technical Glut Trap

Some teams indulged in deep technical reviews and merciless code refactoring with no limits to their technical imagination and creativity. As shown in Fig. 1 below, this formed an endless positive feedback loop, where refactoring could never end. This dynamic intensified when the other dashed loop pushed in the same direction. That is when the enhancements did not enhance the code but only created more cluttered code. This happened when the team concentrated solely on refactoring to patterns, making the code more robust with respect to patterns, but less readable and more complex to maintain!

![The dashed lines are another positive feedback loop. If changes make the code less clean (or less readable and more complex), this may escalate the effect of the inner positive feedback loop. Signs indicate whether an action has a positive or negative effect. The ± sign indicates that changing code may produce either more clean or less clean code based on the kind of change](images/technical_glut_trap.png)

#### 5. Unsustainable Development Pace:

The development pace was not sustainable by developers, managers, or customers. Teams were developing new features, fixing bugs, and supporting customers on one branch while on another branch, they were applying large refactorings, experimenting with design patterns, doing architectural spikes, and other refactoring fixes.

Managers and customers started to receive delayed fixes and prolonged plans for new features. To accommodate release fixes, the development team had to apply the same fix twice, once on every branch. After a while, team members couldn’t sustain the huge effort of maintaining multiple versions of the code, and managers stopped supporting them, because they didn't see tangible results out of it. Eventually, the refactoring effort was discontinued and after a while, the refactored code branch was abandoned altogether.

## Is there a better way?

After these failures, I realized that there has to be another way of doing it. This triggered my research and experiments with some volunteering teams till we reached what we believe to be a better way.

This "new" approach of refactoring is put in a roadmap format, and is explained in detail in the rest of this book. But, before we do that, these are some guiding principles which governed my thinking when I designed this roadmap.

##### Simplicity

I always considered myself an average developer. I wasn't that genius/geek software guy who does everything with several clicks on the keyboard. In most of my development live, I always detested complex (and probably genius solution), mainly because I believe that simple and effective solutions need not be explained in more than 10 minutes. If you can't do that, then let's start over and think again with a fresh mind. Guess what, in all cases which I can remember in my 12+ years of development, this worked, and we managed to find a simple solution.

Now, I find this fact, that I'm an average developer who doesn't feel good towards complex solutions; I find this a gift really. Because, when I started thinking of a better way of refactoring legacy applications, I always thought of things which "average" developers (like me) can understand, do, and appreciate.

This is why I have to add this disclaimer: Most of the information in this book is simple, and probably this is why it is very effective.

##### Sustainability

Refactoring should always be sustainable for developers, managers, and users.

It's unsustainable for developers when they cannot find support from management to sponsor it and have to stay up at night to do it. It's unsustainable for managers when they wait for months till they see or "feel" the value out of it. It's unsustainable for users and customers when they have to wait for months with no updates or fixes and they have to live with the old code as is till development teams finish with refactoring.

The refactoring roadmap in this book is designed to be sustainable for all; for developers, managers, customers, and end-users.
