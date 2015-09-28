+++
title = "DefCore Misconceptions Part 2: Debunking Advisory Status"
author = "markvoelker"
date = "2015-09-28"
url = "/defcore-misconceptions-2"
socialsharing = true
description = "Advisory capabilities don't necessarily become required.  Let's debunk some myths about the DefCore state machine!"
categories = ["DefCore","OpenStack"]
+++

*[DefCore](https://wiki.openstack.org/wiki/DefCore) isn't a new 
creature in the OpenStack community: it's been in
discussion since at least 2013.  However it was only earlier in 2015
that adherence to DefCore Guidelines became a requirement for products
that want to use the OpenStack name and OpenStack Powered logo.  As a
result, a lot more people are now interested in DefCore than in the
past.  As we've started receiving a lot more feedback about DefCore,
it's become apparent that there are a few misconceptions out there from
folks who are new to DefCore.  Personally I held some of those same
misconceptions when I started participating in DefCore about 9 months
ago.  This series of posts is intended to lay some of those
misconceptions to rest and help folks learn more about DefCore is, how
it works, and what it's trying to accomplish.*

In DefCore parlance, a
[Guideline](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/Lexicon.rst#n72)
is essentially a document that lists what capabilities an [OpenStack
Powered(TM)](http://www.openstack.org/interop) product must expose to end
users.  For each one of those
[Capabilities](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/Lexicon.rst#n16),
there is also a list of tests that the product (which could be a public cloud,
a distribution, an appliance, or a managed offering) must pass in order to
prove that it supports those Capabilities.  Individual Capabilities can be
[marked as being in one of four
states](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/Lexicon.rst#n16):
*required*, *advisory*, *deprecated*, or *removed*.
As soon as you read those four words, a few ideas about the lifecycle of
Capabilities probably starts to materialize in your head.  Today I'll talk
about one of those states (we'll save the others for a later post):
*advisory*.

A Capability starts off life when the DefCore Committee considers adding and
removing Capabilities to it's Guidelines, which happens [once every six
months](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/2015A.rst#n10) 
and is offset by a couple of months from OpenStack software releases.
This part of the lifecycle is pretty subjective:
generally speaking, in order to be proposed someone has to think that there's
a strong enough argument for including a capability to bring it up for
discussion.  Typically the list of candidates has been created by having
members of the DefCore Committee chat with PTL's or other members of the
technical contributor community while also injecting their own opinions, though
anyone can submit ideas.  You don't have to be sure the candidate is a
slam-dunk for inclusion, you just need to think it's at least worth talking
about with a wider audience.

The next step is that the candidate Capabilities are proposed for scoring.  In
order to decide what Capabilities make it into Guidelines, the DefCore
Committee has a set of [12
Criteria](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst)
that it measures Capabilities against (mostly are [trailing indicators of
market acceptance](../defcore-misconceptions-1/)).  In the early
days of the Committee, scoring was done via live audio meetings with a Google
spreadsheet and was, IMHO, a somewhat inexact process (which is fine:
bootstrapping a new thing is always challenging and your processes should
naturally improve and mature over time as long as you incorporate feedback and
lessons learned).  In the current cycle, the DefCore Committee [modified it's
process for scoring](https://review.openstack.org/#/c/210076/) so that scoring
is now done in Gerrit.  This basically involves filling out a [simple
worksheet](http://git.openstack.org/cgit/openstack/defcore/tree/working_materials/scoring.txt)
for the Capabilities you want to propose, and using a [simple
script](http://git.openstack.org/cgit/openstack/defcore/tree/working_materials/tabulate_scores.py)
to tally up a total score (it also emits a CSV version if you're more
accustomed to using a spreadsheets for this sort of thing).  If a Capability
scores high enough, you can include modifications to the
still-under-construction [next
Guideline](http://git.openstack.org/cgit/openstack/defcore/tree/2016.next.json)
in the same patch.  This allows everyone to see what candidates are being
proposed, some initial thinking behind how they score to get the conversation
started, and exactly what tests are being proposed for those candidates.
You can have a look at [several](https://review.openstack.org/213353)
[proposals](https://review.openstack.org/221631)
[for](https://review.openstack.org/213330)
[new](https://review.openstack.org/223915)
[capabilties](https://review.openstack.org/210080) that are currently going
through this process.  

You may notice that for each Capability added to a Guideline, there's a field
for
[status](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/schema/1.3.rst#n66).
Very importantly, when a new Capability is added to a Guideline for the first
time, it [must be listed in *advisory* state](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/2015A.rst#n113)
rather than *required* state according to DefCore's Board-approved operating
rules. So, what exactly does *advisory* mean?  Does it mean that the
Capability in question is going to be required in the future and that vendors
should get busy adding it to their product offerings if they don't include it
today?  *Not quite.*

There is actually a
[definition](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/Lexicon.rst#n13)
of advisory status included in the DefCore lexicon, and it's fairly simple: it
means that a Capability has *been **suggested** for the next Guideline*.  Note
that word: *suggested*.  Essentially, the DefCore Committee is saying to
the world: "hey ladies and gents, we're thinking about requiring this
Capability in the future--what do you think about that?"  In other words:

**"Advisory" status means that the DefCore Committee thinks there's good
reason for requiring a Capability and would like to solicit feedback from the
community about whether that's actually the case.**

So, just because a Capability is marked as advisory in one Guideline **does
not** mean it will automatically graduate to required status in the next
Guideline.  In many cases that might actually happen if the scoring was done
properly and there's not much argument about a given Capability (and in
such cases the advisory state also serves as a handy heads-up to vendors to
check whether they're supporting that Capability).  But the
Committee might also get feedback such that the Capability actually isn't a
good choice.  That might be for any number of reasons: perhaps the
Capability is actually [specific to a particular backend
driver](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n61), maybe it's not
actually as [widely
deployed](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n40)
as was assumed, or perhaps it's not as widely [used by
clients](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n48)
or
[tools](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n79)
as was thought, etc etc.  

Getting the scoring right for a given Capability is actually a fairly hard
and labor-intensive process that involves doing a lot of research, and
sometimes good information isn't readily available.  The advisory status
actually serves as means of stepping up the conversation so that more
information can be gathered and more opinions can be heard, and it provides a
minimum of six full months for that conversation to happen.  Notice the word
*minimum* there.  If the community is divided, it's entirely possible the
Capability might stay in advisory status
while more discussion is had or while other adjustments (to code, to products,
to tests, to clients, etc) are made.  Alternately, the Capability might simply
be dropped in the next Guideline after discussion indicates it's really not a
good candidate.  Or, it may graduate to required status if there aren't good
grounds for dropping it or prolonging the advisory period.

It's worth mentioning that one clue that advisory Capabilities don't always
become required is that the two status have different bars to meet when
scoring a Capability.  The DefCore Committee [picks a cut-off
score](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/2015A.rst#n109)
that Capabilities must meet in order to become required.  That score can
change from cycle to cycle as weights to Criteria are also adjusted, but
in the past the cut-off score has hovered around 74.  Though not strictly
required to do so, the DefCore Committee has also generally applied a minimum
score for advisory Capabilities: if a candidate can't muster at least 50
points, there's probably not much point in having a larger discussion about it.

I should also note that there are all kinds of "pressure relief valves" built
into the DefCore process, of which the six-month advisory period is just one.
For example, if you read the [timeline
section](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/2015A.rst#n10)
of DefCore's operating procedures that I mentioned earlier, you'll notice that
Capabilities are generally selected for advisory status by the time a Summit
rolls around (with an implicit assumption that Summits are a good opportunity
to talk about what's being proposed so everyone is aware of it), but the
proposal doesn't go to the Board of Directors for a full three months
afterward.  That means there's actually several months even before a
Capability becomes required during which it's merits can be discussed (not to
mention the discussion that happens when the initial scoring patch is
submitted).  And once a Capability does become required, it can be
[flagged](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/Lexicon.rst#n68)
if there's [sufficient
reason](http://git.openstack.org/cgit/openstack/defcore/tree/HACKING.rst#n85)
to think it shouldn't be required.

So there you have it: advisory status doesn't mean that a Capability will
necessarily be required in the future, just that there is a *chance* it will
be subject to discussion by all segments of the community.

*Questions?  Comments?  Drop by #openstack-defcore on IRC or drop the
DefCore Committee a line at
[defcore-committee@lists.openstack.org](mailto:defcore-committee@lists.openstack.org).*
