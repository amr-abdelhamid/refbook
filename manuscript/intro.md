{frontmatter}

# Introduction and Background

The story of this book started long ago in 2009 while helping organizations and coaching teams to adopt more agile ways of work. I have faced lots of problems working with teams whose code is extremely poor. This was a show stopper, and teams suffered from frequent code failures and intermittent outages. Such code was literally stopping them from transitioning to shorter iterations or even smaller releases.

## Why am I writing this book?

This book is an assimilation of the large amount of good advice available in books and online; summarized and organized into a roadmap. It is an easy read for juniors and seniors who are responsible for maintaining software regardless of their business domain or technology.

This book is **not** a reference for all clean coding techniques and refactoring best practices. I have intentionally left over many of the good and necessary advice and tools, sometimes because they are not universal and may only apply in some cases, or because I had to chose only a small set of techniques to help teams kick off the refactoring effort and not overwhelm them with so many ideas.

## Why refactoring matters?

In the 90’s, 70 billion of the 100 billion expenditure on software products are spent on maintenance; and 60% of which is consumed to locate defective code [1]. Using simple algebra, reducing the amount of time to locate defective code by 30% would reduce the overall expenditure on software by 15%, which is a huge improvement.

## How to refactor - the "old" way

Over the course of several years, I have struggled with teams to refactor their application code to be easier to understand and cheaper to modify. These attempts followed one or more of the old-way patterns described below. I have to say that most of these attempts failed or achieved very little value.

**1. Re-write the whole**

This solution looms like the easiest solution from both technical and management point of views. If code is cluttered and causing lots of trouble, trash it and start from scratch. The question is: If you start from scratch, what makes you confident that you'll not hit the same wall again?

{icon=bookmark}
G> *The question is: If you start from scratch, what makes you confident that you'll not hit the same wall again?*

It's like the next picture of two accidents. Car capabilities are different, one is ordinary sedan car while the other is an expensive sports car. However, the accidents are similar, because the *driving habits* are also similar.

![Bad driving habits may cause very bad accidents, and bad *development habits* may also lead to very poor code with lots of technical debt](images/car_accidents.png)

So, if you would like to start over again and not hit the same wall, you have to change your development habits, rather than change the code. But, why not change development habits while still maintaining the same code? This would definitely save us millions of investment, and this book gives you a roadmap how to do that.

**2. Technical hero**

This is a very common workaround. Hire a highly qualified technical person and give him full authority and power. People usually expect that, such a person is magically capable (someway or another) to fix things up and turn around the code from being cluttered and spaghetti to structured, readable, understandable and changeable.

There are two fundamental problems with this approach:

1. Such persons are very expensive and may not be affordable by many organizations. If this is the only way to go, then refactoring has become a luxury product only attainable by big and wealthy organizations. For all others, hard luck. What we aspire, instead, is to enable all software people to refactor their own code themselves; regardless of their seniority or technical expertise. This is more sustainable, although requires some more time and effort.

2. Beware that a technical hero may well take wrong refactoring decisions, especially if nobody is reviewing his work. Such decisions may lead the whole team into a dead end and waste so much time. Having full power and authority may also lead very easily to what I called a [Technical Glut Trab](#thetrab) described below.

**3. As per the book**

This is another pattern in which people tend to follow advice *as per the book*. There are so many excellent books which teaches you a lot about clean coding, refactoring, design patterns, etc. While all these books are extremely helpful, they might not be used as step-by-step guide to refactoring poor code. Rather, they are like a buffet of so many good advice and proven practices and techniques; however, they don't tell you which of them is higher priority than other in *your* case.

**4. Try-then-retry**

This is another pattern which I see very frequently. If you have large amounts of technical debt, try to fix a part of the product once there is a change request related to this part. If it works, then fine. If not, retry with the next change request.

Although this is the most pragmatic approach to handling technical debt, but it incorporates a lot of risks postponed and accumulating over time. At one point of time, the system may reach what we call *system collapse point* where refactoring the code due to a simple change may become so expensive and not affordable.

---

I have documented some observations (or rather surprises) from these failures in an experience report published several years ago [2]. This is a summary of the most important ones:

* Covering your code with automated tests is not the first step in refactoring!
* Working on refactoring for a long time is like working on bug fixing for a long time. It simply sucks! There has to be another way which is more sustainable.
* One of the impediments to successful refactoring is the development team's focus on refactoring to patterns. This shifts their focus from making code simpler and understandable to making the code intelligent and "stylish"!
* In many cases, managers do believe that continuous refactoring is necessary to keep the code healthy. However, we noticed that they do not sponsor refactoring effort. Why? The answer is that _managers will not sponsor an activity which they cannot track or control._
* For refactoring to succeed, several qualification should exist in the development environment. Qualifying the environment may take sometime to be done; but once done, refactoring is 90% complete!

These observations may somehow be correlated to reasons why refactoring fails. In the same report [2], I have elaborated more on them and listed some key factors why refactoring failed in early experiments. This is the topic of the next section. 

## Why refactoring fails? {#whyrefactoringfails}

#### 1. Vague and hazy objectives

Attempts to refactor had hazy and unclear objectives. It was the gut feeling of the engineering staff that determined what to do

#### 2. Covering poor code with fragile tests

We always thought that an automated test suite is a safety net for any side effects or regression issues which refactoring may cause. Without such safety net, most of the time, we didn't have the courage to change the code and integrate it to the mainline. However, in all three projects, this turned out to be not possible due to many factors. For example, product code lacked clear system interfaces and suffered from scattered business logic in all layers, including the database. In other case, writing automating tests incurred very high costs, not only in development, but in toolset and training. In time, I felt that some team members viewed automated tests as the first impediment to refactoring!

#### 3. It's non of the managers' business!

Technical teams had the attitude that refactoring was “none of the managers’ business”. Also, they did not spend any effort to involve busy managers and get their support. This attitude created a counter effect from managers towards refactoring. The refactoring effort was viewed by senior managers as a non-value adding activity and was only allowed due to pressure from the development teams. Once the planned time for refactoring elapsed, management became more and more resistant to spending any more effort on refactoring.

{icon=bookmark}
G> *Managers will not sponsor an activity which they cannot track or control.*

#### 4. The Technical Glut Trap {#thetrab}

Some teams indulges in deep technical review and merciless code refactoring with no limits to their technical imagination and creativity.

As shown in the next figure, this may form an endless positive feedback loop, because of which refactoring might never end. This dynamic intensifies when the outer dashed loop pushes in the same direction and the changes deteriorates the code rather than enhance it.

![The more you change the code, the more ideas you'll get to further change the code. Unless you keep your changes small and integrated frequently, the cycle will keep escalating and the code will be finally thrown away. The dashed lines are another positive feedback loop. If changes make the code less clean (or less readable and more complex), this may escalate the effect of the inner positive feedback loop.](images/technical_glut_trap.png)

One very frequent example of the technical glut trap is when the team focuses more and more on refactoring to patterns to make the code more "robust" with respect to patterns, but less readable and more complex to maintain!

#### 5. Unsustainable Development Pace

The development pace was not sustainable by developers, managers, or customers. Teams were developing new features, fixing bugs, and supporting customers on one branch while on another branch, they were applying large refactorings, experimenting with design patterns, doing architectural spikes, and other refactoring fixes.

Managers and customers started to receive delayed fixes and prolonged plans for new features. To accommodate release fixes, the development team had to apply the same fix twice, once on every branch. After a while, team members couldn’t sustain the huge effort of maintaining multiple versions of the code, and managers stopped supporting them, because they didn't see tangible results out of it. Eventually, the refactoring effort was discontinued and after a while, the refactored code branch was abandoned altogether.

## Is there a better way?

After these failures, I realized that there has to be another way of doing it. This triggered my research and experiments with some volunteering teams till we reached what we believe to be a better way.

This "new" approach of refactoring is put in a roadmap format, and is explained in detail in the rest of this book. But, before we do that, these are some guiding principles which governed my thinking when I designed this roadmap.

##### Simplicity

I always considered myself an average developer. I wasn't that genius/geek software guy who does everything with several clicks on the keyboard. In most of my development live, I always detested complex (and probably genius solution), mainly because I believe that simple and effective solutions need not be explained in more than 10 minutes. If you can't do that, then let's start over and think again with a fresh mind. Guess what, in all cases which I can remember in my 12+ years of development, this worked, and we managed to find a simple solution.

Now, I find this fact, that I'm an average developer who doesn't feel good towards complex solutions; I find this a gift really. Because, when I started thinking of a better way of refactoring legacy applications, I always thought of things which "average" developers (like me) can understand, do, and appreciate.

This is why I have to add this disclaimer: Most of the information in this book is simple, and probably this is why it is very useful!

{icon=bookmark}
G> *Disclaimer: Most of the information in this book is simple, and probably this is why it is very useful!*

##### Sustainability

Refactoring should always be sustainable for developers, managers, and users.

It's unsustainable for developers when they cannot find support from management to sponsor it and have to stay up at night to do it. It's unsustainable for managers when they wait for months till they see or "feel" the value out of it. It's unsustainable for users and customers when they have to wait for months with no updates or fixes and they have to live with the old code as is till development teams finish with refactoring.

The refactoring roadmap in this book is designed to be sustainable for all; for developers, managers, customers, and end-users.
