# Infrastructure team planning

We (the infrastructure team) plan our work every quarter.

> [!NOTE]
> In one year we have 4 quarters:
>
> - Q1: January, February, March.
> - Q2: April, May, June.
> - Q3: July, August, September.
> - Q4: October, November, December.

Here's how the planning process works:

- At the begin of the quarter, we publish a blog post in the
  [Inside Rust](https://blog.rust-lang.org/inside-rust/) blog to share
  what we've accomplished in the last quarter and what we planned for this
  quarter.
- During the quarter, we execute the work planned.
- Before the end of the quarter, we plan the work for the next quarter.

Each entry in the plan has the following fields:

- _Owner_: the person responsible for the work.
- _Tracking issue_: link to GitHub issue.
- _Description_: a brief description of the work
  (full details should be in the GitHub issue).

## Are volunteers expected to own items in the plan?

Volunteers don't have any obligation in:

- Owning items in the plan.
- Working on items in the plan.
- Participating in the planning process.

However, volunteers are welcome to participate in the planning process
in any way they want, including owning items in the plan.

See [Team structure](README.md#team-structure) for more information.

We want volunteers to spend their time on things they enjoy,
and we acknowledge that planning might not be one of those things.

## How do you decide what to work on?

We have a wide range of responsibilities, and different
[stakeholders](README.md#stakeholders), so deciding what to work on is hard.
Items are selected taking into account the following criteria:

- _Value_: how much the work maximizes our [goals](README.md#goals).
- _Effort_: how much effort is required to complete the work.
- _Urgency_: how urgent is the work.

## Does the team only work on items in the plan?

No, there are:

- Unexpected things that come up that the team needs to work on urgently, such
  as incidents, security issues or high-priority requests from stakeholders.
- Periodic maintenance tasks, such as updating dependencies, packages and
  operating systems.

Plus, volunteers are free to work on anything they want, although they are
encouraged to work on the items that align with the team's priorities.

## Why do we plan?

Planning has the following benefits:

- _Transparency_: The community knows what we are working on and what to expect
  from us in the near future.
- _Coordination_: Other teams know if a certain infra feature they are waiting
  for is coming soon or not. This helps them plan their work.
- _Team growth_: People reading our plan might be interested in certain tasks
  and they might decide to help us. See our "growth" [goal](README.md#goals).
  The blog post will contain an explicit call to action about this.
- _Prioritization_: We have limited capacity and there are many things to do.
  Planning helps us deciding what to work on in a more structured way, so that
  we don't end up working on things that are not important.
- _Execution_: Planning helps us distinguish between "time to take decisions"
  and "time to execute". If a new big idea comes in the middle of the quarter
  we can easily say "cool, we'll evaluate it at the end of the quarter" and
  keep executing our initial plan instead of
  discussing if it's worth interrupting what we are doing and switch to it.
  Changing plans too often is not productive. If we have a clear planning, we
  can focus on what we need to do and avoid distractions.

Planning has the following drawbacks:

- _Pressure_: We might feel pressured to finish what we planned by the end of
  the quarter.
  However, we shouldn't. See our [Values](README.md#values).
  The main purpose of the plan is to give us a direction and be mindful of
  how we spend our time.
  Also, estimating is hard: if we finish 100% of what we planned, it might mean
  that we underestimated the work, not that we are super productive. The
  opposite is also true: if we finish 50% of what we planned, it might mean
  that we overestimated the work, not that we are not productive.
  Of course we should do our best to finish what we planned, but if don't, it's
  totally fine.
  The blog post should make this point clear.
- _Time_: Deciding what to work on and writing it in the form of blog post
  takes time. However, we think this time is well spent.
  Also, we want to keep the planning process as lightweight as possible to avoid
  spending too much time on it.

Overall, we think that the benefits of planning outweigh the drawbacks.

## Why every quarter?

Planning too often is not productive, because it takes time.
Planning too rarely doesn't allow us to adapt to changes quickly.

We think 3 months is the right balance.

## Why having a deadline?

We think that having a deadline (3 months in this case) helps us to:

- Cut scope during planning: thinking about a limited period of time, helps
  us stay with the feets on the ground.
- Cut scope during execution: avoid spending too much time on a single item.
  E.g. we are at month 2 of the quarter and we still have remaining items to
  work on for the quarter. We can decide to cut the scope of one item
  (because it's in a good enough state) and move to the next one.
  In the next quarter we can decide if it's worth to pick up the item again and
  finish it.

We think that not having deadlines can easily lead to scope creep,
i.e. we would end up working on a single item for too long.
