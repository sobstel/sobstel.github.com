---
title: Lean / Minimalist Software Development
layout: post
---

sort of personal manifesto

---

Split into small batches. Really small user stories. Each deliverable and shippable.

It makes everyone happy. Product owners and stakeholders and developers (there's nothing more
motivating than someting that works). Constantly added value.

And if things go wrong, it's easier to sacrifice and live with waste of 3 days of work of 1-2 guys rather than 1 month of whole development team.

Kaizen philosophy -> continous improvement in small steps.


GitHub: -> separate post -> work as an open source project

* work async (nobody pulling out of the zone)
* no meetings, no deadlines, no managers, no work hours
* time flexibility - work whenever and wherever you want
* log all the things - everyone on the same page
* chatrooms - no presence needed

* work on what you want
* work where i want
* work when i want

* work like open source project

* pull requests for code review and any new code, for discussions (code, feature, strategy), for everything related feature
  * simple branching (master -> branch)
  * everyone can push, everyone can deploy
  * master is always deployable
    * you can deploy to staging or to production box
  * getting shit done without wasting a lot of time

* issues
  * keep it simple, without any meta-information
  * todo list (tasks) within issue as a list (of checkboxes)

http://zachholman.com/posts/how-github-works/
http://zachholman.com/talk/how-github-uses-github-to-build-github/

---

1. It is impossible to gather all the requirements at the beginning of a project.
2. Whatever requirements you do gather are guaranteed to change.
3. There will always be more to do than time and money will allow.

---

Minimalism is not about doing less. It's about not doing nonessential things, so there's a time
for things that truly matter.

---

Lean is:

1. Eliminate Waste
2. Build Quality In
3. Create Knowledge
4. Defer Commitment
5. Deliver Fast
6. Respect People
7. Optimize the Whole

1. Eliminate waste
3. Amplify learning
4. Decide as late as possible
5. Deliver as fast as possible
6. Empower the team
8. Build integrity in
7. See the whole

"Organizations that are truly lean have a strong competitive advantage because they respond very rapidly and in a highly disciplined manner to market demand, rather than try to predict the future."" – Mary Poppendieck


-

1.
Continuously eliminate anything that isn’t adding value and only work on what we absolutely need to be doing at this moment in time. Eliminating waste means eliminating useless meetings, tasks and documentation.

Don't look too far. Things are often changing so we often end up not needing them – or if we do, we have to rework them because conditions and our understanding has changed by then

2.
Develop in a way that builds quality into our product, because there’s no way to continuously deliver fast if we have to keep going back to clean up our messes.

4.
Software development is about learning, so structure the work to ensure we’re continuously learning. And because of that, defer decisions until the last responsible moment (because we’ll know more by then).

6.
Lean says to respect that the people doing the work are the ones that best know how to do it. Give them what they need to be effective and then trust them to do it. EMPOWERED!

7.
Lean also puts a very strong emphasis on what it calls “the system” – that is, the way that the team operates as a whole. We always need to be looking at our work from a top level to ensure we’re optimizing for the whole.



---

### Eliminate Waste

3 general forms of waste

Muda (unproductive), Mura (inconsistency), Muri (over-burden)

unnecessary code and functionality,
delay in the software development process,
unclear requirements,
avoidable process repetition (often caused by insufficient testing),
bureaucracy,
slow internal communication

Partially done coding eventually abandoned during the development process is waste.
Extra processes and features not often used by customers are waste.
Waiting for other activities, teams, processes is waste.
Defects and lower quality are waste.
Managerial overhead not producing real value is waste.

retrospective to discuss what went well. Identifying and eliminating waste should not be a rare event
- meaningful and actionable improvements that actually help you to frequently identify and, more importantly, eliminate waste (assigned to person)

partially done work
extra processes
extra features
task switching
waiting
motion
defects

### Build Quality In / Amplify learning / Create Knowledge

Software development is a continuous learning process.

The best approach for improving a software development environment is to amplify learning.

The accumulation of defects should be prevented by running tests as soon as the code is written.

Instead of adding more documentation or detailed planning, different ideas could be tried by writing code and building.

The process of user requirements gathering could be simplified by presenting screens to the end-users and getting their input.

Automation Agile development methods also encourage automated regression testing.
TDD. BDD. In-house tester. Whatever.

Code reviews
Documentation / Wiki (README driven docs!!!!)
Smartly commented code (no redundant easy-to-forget-and-miss docs)
Trainings and knowledge sharing sessions

### Defer Commitment / Decide as late as possible

decide as late as possible
particularly for decisions that are irreversible, or at least will be impractical to reverse.

you can safely leave critical decisions, the more information you will have available to make the right decision when the time comes.

Deferring irreversible decisions means you keep your options open for as long as possible.

Deciding too early, you run the very likely risk that something significant will have changed, meaning your end product might meet the spec, but it might still be the wrong product! This is one reason why so many projects fail.

As software development is always associated with some uncertainty, better results should be achieved with an options-based approach, delaying decisions as much as possible until they can be made based on facts and not on uncertain assumptions and predictions.

The iterative approach promotes this principle – the ability to adapt to changes and correct mistakes, which might be very costly if discovered after the release of the system.

delaying certain crucial decisions until customers have realized their needs better. And we understand a problem better. This also allows later adaptation to changes and the prevention of costly earlier technology-bounded decisions.

### Deliver Fast / Deliver as fast as possible

It is common to over-engineer solutions, both in terms of the software architecture, and also the business requirements.

development in small incremental steps,
through close collaboration,
developing in small iterations,

benefit: constant 2-way feedback between the Product Owner and the team to make sure we build teh irght product

Frequent Integration. Most agile methods also advocate doing regular and frequent builds. At least daily, if not hourly. Extreme Programming advocates continuous integration, with code integrated into the overall system, built and automatically unit tested as soon as it is checked in.

Speed to market is undoubtedly a competitive advantage.

Keep It Simple.  Another way to go faster, as I eluded to earlier, is to keep things simple.  Keep the requirements simple.  Keep the technical solution simple.  Find the easiest way to meet the users’ goals.  Don’t over-engineer.  Don’t spend too long considering future requirements that may never materialise.  Get something simple to market and build on it based on real customer feedback.

Managing Trade-offs
One word of warning though. Quality is only one dimension of the project – the others being time, cost and scope. This is judgemental decision, need to know where to draw the line.

### Respect People / Empower the team

Have the Right People.  (...) the idea of having ‘thinking people’.  People who are able to think for themselves, solve problems, be proactive, be flexible, take appropriate actions, make decisions.
Have them empowered.

I think it’s important to treat everyone with the same respect, whatever their job.  It doesn’t matter whether they’re the CEO, a developer, project manager, the receptionist or the cleaner, respect everyone equally.

responding to people promptly,
listening attentively,
hearing their opinions and not dismissing them even when they are different to your own
encouraging people to have their say
having empathy for their point of view
trying to see things from their perspective
...while being assertive and disagree politely (not being smart ass)

respecting people is giving people the responsibility to make decisions about their work.
...develop people who can think for themselves.
People who can think for themselves and  are experts in their area often need to be empowered to feel respected.

empowerement

### Build Integrity in

The customer needs to have an overall experience of the System – this is the so-called perceived integrity: how it is being advertised, delivered, deployed, accessed, how intuitive its use is, price and how well it solves problems.

Conceptual integrity means that the system’s separate components work well together as a whole with balance between flexibility, maintainability, efficiency, and responsiveness. This could be achieved by understanding the problem domain and solving it at the same time, not sequentially. The needed information is received in small batch pieces – not in one vast chunk with preferable face-to-face communication and not any written documentation. The information flow should be constant in both directions – from customer to developers and back, thus avoiding the large stressful amount of information after long development in isolation.

Refactoring is about keeping simplicity, clarity, minimum amount of features in the code.

### Optimise The Whole / See the whole

"Think big, act small, fail fast; learn rapidly"
a culture of continous improvement

