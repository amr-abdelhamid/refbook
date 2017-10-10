{frontmatter}

# Introduction and Background

The story of this book started long ago at 2009, while helping organizations and coaching teams to adopt more agile ways of work. I faced lots of problems working with teams whose code is extremely poor. Such teams suffered from frequent code failures and intermittent outages. Code was literally stopping them from transitioning to shorter iterations or even smaller releases.

This book is an assimilation of the large amount of good advice available in books and online; summarized and organized into a roadmap. It is an easy read for juniors and seniors responsible for maintaining software regardless of their business domain or technology.

This book is **not** a reference for all clean coding techniques and refactoring best practices. I have intentionally left over many of the good advice and tools, because in some cases they are not universal and may only apply in special cases, or because I had to chose only a small set of techniques. This small set of techniques will help teams kick off refactoring rather than overwhelm them with so many ideas.

## Why refactoring matters?

In the 90’s, 70 billion of the 100 billion expenditure on software products were spent on maintenance; and 60% of which was consumed to *locate defective code* [1]. Using simple algebra, reducing the amount of time to locate defective code by 30% would reduce the overall expenditure on software by 15%, which is a huge improvement.

## How to refactor - the "old" way

Over the course of several years, I have struggled with teams to refactor their application code to be easier to understand and cheaper to modify. These attempts followed one or more of the old-way patterns described below. I have to say that most of these attempts failed or achieved very little value.

**1. Re-write the whole**

This solution looms like the easiest solution from both technical and management point of views. If code is cluttered and causing lots of trouble, trash it and start from scratch. The question is: If you start from scratch, what makes you confident that you'll not hit the same wall again?

{icon=bookmark}
G> *The question is: If you start from scratch, what makes you confident that you'll not hit the same wall again?*

It's like the next picture of two accidents. Car capabilities are different, one is ordinary sedan car while the other is an expensive sports car. However, the accidents are similar, because the *driving habits* are also similar.

![Bad driving habits may cause very bad accidents, similar to bad *development habits*, which may also lead to very poor code with lots of technical debt](images/car_accidents.png)

So, if you would like to start over again and not hit the same wall, you have to change your development habits, rather than change the code. But, why not change development habits while still maintaining the same code? This would definitely save us huge costs of development, and this book gives you a roadmap how to do that.

**2. Technical hero**

This is very common: Hire a highly qualified technical person and give him full authority and power. People usually expect that such a person is magically capable (someway or another) to fix things up and turn the code from cluttered and spaghetti to structured, readable, understandable and changeable code.

There are two fundamental problems in this approach:

1. Such persons are very expensive and may not be affordable by many organizations. If this is the only way to go, then refactoring will become an expensive activity monopolized by some wealthy teams. What we aspire, instead, is to enable all software people to refactor their own code themselves; regardless of their seniority or technical expertise. This is more sustainable, though requires more time and effort.
2. Beware that a technical hero may well take wrong refactoring decisions, especially if nobody is reviewing his/her work. Such decisions may lead the whole team to hit the wall and waste so much time and effort. Having full power and authority may also lead very easily to what I call a [Technical Glut Trab](#thetrab) - described below.

**3. As per the book**

This is another pattern of people who tend to follow advice *as per the book*. There are so many excellent books about clean coding, refactoring, design patterns, etc. While all these books are extremely helpful, they might not be used as step-by-step guide to refactoring poor code. Rather, they are like a buffet of so many good advice and proven practices and techniques; however, they don't tell you which of these practices and techniques has of higher priority and would realize higher value in *your* specific case.

**4. Try-then-retry**

This is another pattern which I see very frequently. If you have large amounts of technical debt, try to fix a part of the product once there is a change request related to this part. If it works, then fine. If not, retry with the next change request.

Although this is the most pragmatic approach to handling technical debt, but it incorporates a lot of risks postponing and accumulating technical debt over time. At one point of time, the system may reach what we call a *system collapse point* when refactoring code due to one change may become so expensive and not affordable:

![Cost of change increases exponentially due to accumulating technical debt](\images\collapsepoint.png)

## Why refactoring fails? {#whyrefactoringfails}

I have documented some of the root causes of these failures in an experience report published 2013 [2]:

#### 1. Vague and hazy objectives

Attempts to refactoring had hazy and unclear objectives. What "good code" means and how to measure "goodness" was not specified or taken care of by the engineering staff. It was the gut feeling of them which determined what to do next in order to make the code "better".

#### 2. Covering poor code with fragile tests

We always thought that an automated test suite with high test coverage is a safety net for a team refactoring poor code, because it picks any side effects or regression issues caused by refactoring old code. Without such safety net, most of the time, we didn't have the courage to change the code and integrate it to the mainline. However, in all three projects, covering poor code with automated tests turned out to be not possible due to many factors. For example, product code lacked clear system interfaces and suffered from scattered business logic in all layers, including the database. In another case, writing automated tests incurred very high costs, not only in development, but in toolset and training. In time, some team members viewed automated tests as an impediment to refactoring!

#### 3. It's non of the managers' business!

Technical teams had the attitude that refactoring was “none of the managers’ business”. They did not spend any effort to involve busy managers and get their support. This attitude created a counter effect from managers towards refactoring and refactoring effort which was viewed by senior managers as a non-value adding activity and was only allowed due to pressure from the development teams. Once the planned time for refactoring elapsed, management became more and more resistant to spending any more effort on refactoring.

{icon=bookmark}
G> *Managers will not sponsor an activity which they cannot track or control.*

#### 4. The Technical Glut Trap {#thetrab}

Some teams indulges in deep technical review and merciless code refactoring with no limits to their technical imagination and creativity.

As shown in the next figure, this may form an endless positive feedback loop, because of which refactoring might never end. This dynamic intensifies when the outer dashed loop pushes in the same direction and the changes deteriorates the code rather than enhance it.

![The more you change the code, the more ideas you'll get to further change the code. Unless you keep your changes small and integrated frequently, the cycle will keep escalating and the code will be finally thrown away. The dashed lines are another positive feedback loop. If changes make the code less clean (or less readable and more complex), this may escalate the effect of the inner positive feedback loop.](images/technical_glut_trap.png)

One very frequent example of the technical glut trap is when the team focuses more and more on refactoring to patterns to make the code more "robust" with respect to patterns, but less readable and more complex to maintain!

#### 5. Unsustainable Development Pace

In early refactoring failed experiments, the development pace was not sustainable by developers, managers, or customers. Teams were developing new features, fixing bugs, and supporting customers on one branch while on another branch, they were applying large refactorings, experimenting with design patterns, doing architectural spikes, and other refactoring fixes.

Managers and customers started to receive delayed fixes and prolonged plans for new features. To accommodate release fixes, the development team had to apply the same fix twice, once on every branch. After a while, team members couldn’t sustain the huge effort of maintaining multiple versions of the code. Managers, on the other hand, stopped supporting them because they didn't see tangible results. Eventually, the refactoring effort was discontinued and the refactored code branch was abandoned altogether.

## Is there a better way?

After these failures, I realized that there has to be another way of doing it. This triggered my research and experiments with some volunteering teams till we reached what we believe to be a better way.

This new approach of refactoring is put in a roadmap format, and is explained in detail in the rest of this book. But, before we do that, these are some guiding principles which governed my thinking when I designed this roadmap.

##### Simplicity

Throughout my development live, I always detested complex (and probably genius) solutions. One reason is that I always considered my self an average developer who is spending hard time trying to understand complex solutions developed by other "geeks". Another reason is that I believe simple and effective solutions need not be explained in more than 10 minutes. If you can't do that, then probably your solution is complex and it might be better to throw it away and look for another solution with a fresh mind. Guess what, in all cases which I can remember in my 16+ years of development, this worked and I managed to find simpler and more innovative solutions.

Now, I find this fact, that I'm an average developer who doesn't feel good towards complex solutions; I find this a gift really. Because, when I started thinking of better ways of refactoring poor code, I thought of things which "average" developers (like me) can understand, do, and appreciate.

This is why you may find most of the information in this book is simple, and probably this is why it is very effective!

{icon=bookmark}
G> *Disclaimer: Most of the information in this book is simple, and probably this is why it is very effective!*

##### Sustainability

Refactoring should always be sustainable for developers, managers, and users.

It's unsustainable for developers to always stay up at night to carry out refactoring tasks. It's unsustainable for managers not to see or "feel" the value of refactoring. It's unsustainable for users and customers to wait for months with no updates or fixes till developers finish a refactored version of the product.

The refactoring roadmap in this book is designed to be sustainable for all; for developers, managers, customers, and end-users.
