# Team Leads

The [Infrastructure Team] is led by two co-leads who share the role with
equal responsibilities and authority. This document describes what the
co-leads do and how the role rotates between team members.

The process is modeled after the compiler team's [RFC 3262], adapted to the
infrastructure team's size and structure.

## Motivation

Leading the team is rewarding, but it is also a long-term commitment that
can wear people down. We [prioritize wellbeing over everything else][values]
and want to avoid burnout at all costs, and that applies to the leads as
much as to everyone else. Rotating the role regularly:

- gives leads a natural point to step down without feeling like they are
  abandoning the team,
- brings fresh perspectives and ideas into the role,
- spreads knowledge about how the team is run, avoiding a single point of
  failure, and
- gives more team members the opportunity to grow into a leadership role.

Rotation is a shared expectation, not an obligation: no team member is
required to take a turn as co-lead.

## Responsibilities

The co-leads share the following responsibilities and decide between
themselves how to split the work:

- Represent the team as its primary point of contact for other Rust teams,
  the Rust Foundation, and infrastructure sponsors.
- Coordinate with the team's Council Representative on project-wide
  governance topics.
- Run the weekly team meeting and curate its agenda.
- Drive the team's [planning] and roadmap.
- Make decisions that are urgent or have a deadline when the team cannot be
  consulted in time, and inform the team afterwards.
- Make sure that incidents and outages are followed up with a post-mortem,
  and drive the improvements that come out of it.
- Watch over the health of the team, onboard new members, and drive
  improvements to the team's processes.
- Look out for team members who could succeed them as co-lead, and
  encourage them to grow into the role.

## Terms

Each co-lead serves a term of two years. The terms are staggered by one
year, so that only one co-lead rotates at a time and the other provides
continuity.

The two-year term is a target, not a hard limit:

- A co-lead can serve additional terms, for example when no successor is
  available. We would rather extend a term than pressure someone into the
  role or leave it vacant.
- A co-lead can step down before the end of their term. Life happens, and
  wellbeing comes first. In that case, the selection process starts
  immediately, and the length of the successor's term can be adjusted to
  re-establish the staggering.

## Eligibility

Every member of the infrastructure team can become a co-lead. Prior
experience leading a sub-team or larger project is helpful, but not
required. The only constraint is that at least one of the two co-leads
must be an employee of the Rust Foundation, as described in
[Foundation representation](#foundation-representation).

## Foundation Representation

The [Rust Foundation] bears the legal and financial responsibility for
large parts of the Rust project's infrastructure, and employs members of
the team to work on it full-time. Much of the coordination with the
Foundation, vendors, and sponsors happens behind the scenes, and is
difficult to participate in for people who are not employed by the
Foundation.

Therefore, at least one of the two co-leads must always be an employee of
the Rust Foundation.

This has a few implications for rotations:

- One of the two seats is effectively reserved for a Foundation employee.
  With staggered two-year terms, the rotations alternate: one year, the
  Foundation-employed co-lead rotates, and the next year, the open seat
  does.
- When a rotation would otherwise leave the team without a
  Foundation-employed co-lead, only Foundation employees can be candidates
  for the seat.
- The open seat is not restricted: it can be filled by any team member,
  including another Foundation employee. If nobody else steps up, it is
  perfectly fine for both co-leads to be Foundation employees.
- If a co-lead's employment with the Foundation ends during their term,
  and the other co-lead is not a Foundation employee, they hand over the
  role to a Foundation employee before the end of their term. The regular
  selection process applies, and the departing co-lead stays in the role
  until their successor takes over.

## Selection

When a co-lead's term ends, the team selects a successor by consensus:

1. The rotating co-lead announces the upcoming rotation in the team meeting
   and in the [t-infra] Zulip channel, at least one month before the
   planned handover.
2. Over the following two weeks, candidates can volunteer, or team members
   can nominate others with their consent.
3. The team discusses the candidates in the team meeting and on Zulip, and
   selects the new co-lead by consensus. Consensus means that no team
   member sustains an objection, not that everyone's first choice wins.
4. If the team cannot reach consensus, the continuing co-lead makes the
   final decision after consulting the Council Representative.
5. If nobody steps up, the current co-lead can extend their term, and the
   team revisits the rotation at the next opportunity.

## Handover

After the new co-lead has been selected, the outgoing and incoming co-leads
work together for a transition period of a few weeks. During this time, the
outgoing co-lead:

- introduces the incoming co-lead to ongoing work, open decisions, and
  external contacts, and
- hands over recurring duties such as running the team meeting.

The rotation is completed by updating the [team database], the README of
this repository, and announcing the new co-lead in the
[t-infra/announcements] channel.

The outgoing co-lead returns to being a regular member of the team.
Stepping down from the role does not mean stepping away from the team.

[infrastructure team]: https://www.rust-lang.org/governance/teams/infra
[planning]: ./planning.md
[rfc 3262]: https://rust-lang.github.io/rfcs/3262-compiler-team-rolling-leads.html
[rust foundation]: https://rustfoundation.org/
[t-infra/announcements]: https://forge.rust-lang.org/infra/docs/internal-announcements.html
[t-infra]: https://rust-lang.zulipchat.com/#narrow/channel/242791-t-infra
[team database]: https://github.com/rust-lang/team
[values]: ./README.md#values
