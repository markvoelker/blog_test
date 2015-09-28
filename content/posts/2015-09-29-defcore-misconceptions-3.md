+++
title = "DefCore Misconceptions Part 3: Deprecation (or: 'Who's on First?')"
author = "markvoelker"
date = "2015-09-29"
url = "/defcore-misconceptions-3"
socialsharing = true
description = "Think DefCore requiring a capability means the community can't deprecate it?  Guess again!"
categories = ["DefCore","OpenStack"]
draft = true
+++

*[DefCore](https://wiki.openstack.org/wiki/DefCore) isn't a new 
creature in the OpenStack community: it's been in
discussion since at least 2013.  However it was only earlier in 2015
that adherance to DefCore Guidelines became a requirement for products
that want to use the OpenStack name and OpenStack Powered logo.  As a
result, a lot more people are now interested in DefCore than in the
past.  As we've started receiving a lot more feedback about DefCore,
it's become apparent that there are a few misconceptions out there from
folks who are new to DefCore.  Personally I held some of those same
misconceptions when I started participating in DefCore about 9 months
ago.  This series of posts is intended to lay some of those
misconceptions to rest and help folks learn more about DefCore is, how
it works, and what it's trying to accomplish.*

One of the concerns I've been hearing about lately concerning DefCore has to
do with what the implications of a
[Capability](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/Lexicon.rst#n16)
being required actually are.  So, let's start off by talking a little about
what Capabilties are and what their lifecycle is like.

In the [DefCore
Lexicon](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/Lexicon.rst),
a Capability basically boils down to a thing that you can ask the OpenStack
software to do for you.  For example, you might ask OpenStack to give you a
list of all images available for you to boot instances from, or you might ask
OpenStack to create an L2 network for you.  Very often, Capabilities more or
less boil down to API calls (there's a Neutron API call to create networks, for
example).  Capabilities that make it into a DefCore Guideline are also
required to have tests that can be used by vendors to prove that they have
exposed those Capabilities to end users.  Generally, those tests are the same
ones that the project in question already uses in it's gate via Tempest (that
way we know that everyone is using the same tests and that the developers of
the project feel those tests accurately define the feature in question).
Capabilities must also be exposed to end users of the cloud (e.g.
tenants) since those are the people DefCore is currently trying to provide
with interoperable ways of interacting with clouds.

Like any other piece of software, OpenStack changes over time...pretty
dramatically sometimes.  In the [Kilo
release](http://www.openstack.org/software/kilo/) for example, 1,492 people
from more than 169 organizations all over the world created 394 new features,
and fixed 7,257 bugs at it's intial release.  Sometimes features also go away
and code is removed or replaced.

One of the interesting bits about the fact that Capabilities are exposed to
users via API calls is that [API's are
*versioned*](http://developer.openstack.org/api-ref.html).  That means that they
change over time, and older API's may eventually be deprecated and removed
from the project (Cinder's v1 API is now deprecated, for example).  I'll use
major changes in the API to talk about the lifecycle of DefCore Capabilities
here since it provides a convenient way to illustrate the story.

Keystone currently has two supported major versions of it's API: v3 is
considered "CURRENT" and v2 is also "SUPPORTED" (v1 was deprecated and removed
a long time ago).  [The
tests](http://git.openstack.org/cgit/openstack/tempest/tree/tempest/api/identity)
that are used at the gate to ensure that those API's are working as
expected when developers make changes to the code are also written against
either the [v2
API](http://git.openstack.org/cgit/openstack/tempest/tree/tempest/api/identity/v2)
or the
[v3
API](http://git.openstack.org/cgit/openstack/tempest/tree/tempest/api/identity/v3).
So if the DefCore Committee wants to add Capabilities provided by Keystone to
it's Guidelines, it first needs to decide which ones it wants: the v2 ones,
the v3 ones, or both.  Remember, the capability of (for example) generating an
authentication token is exposed to end users differently by those two API's:
if I'm a developer writing an application that I want to interact with
OpenStack clouds, I need to decide whether I make my application call the v2
API or the v3 API and code it up accordingly.  Vendors shipping OpenStack
distributions, running public clouds, etc also have to actually expose those
API's to you before you can use them (and likewise, they also have to choose
whether to expose v2, v3, or both).

So let's imagine for a moment that DefCore ends up requiring some capabilties
of both v2 and v3 in one of it's Guidelines (which might be a reasonable
approach to take for a while given that you can run both API's concurrently
and doing so allows application developers some time to move their
applications from using v2 to using v3).  Eventually though, Keystone might
decide to deprecate the v2 API.

But wait a minute....vendors who want to run an OpenStack Powered(TM) public
cloud, ship a distribution or appliance, or provide a managed offering are
required to expose the v2 API, right?  So the Keystone project can't deprecate
it, right?  Doesn't DefCore have to stop requiring the capability before it
can be deprecated from the code as well in a case like this?

It turns out, the answer is *no*.

First, it's important to understand when DefCore creates Guidelines.  The
schedule for doing so is actually laid out in
[a procedural
document](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/2015A.rst#n10)
that's approved by the OpenStack Board of Directors.  That schedule looks like
this:

| Time Frame | Milestone | Activities                           | Lead By   |
|------------|-----------|--------------------------------------|-----------|
| -3 months  | S-3       | "Preliminary" draft (from current)   | DefCore   |
| -2 months  | S-2       | ID new Capabilities                  | Community |
| -1 month   | S-1       | Score Capabilities                   | DefCore   |
| Summit     | S         | "Solid" draft                        | Community |
| Summit     | S         | Advisory/Deprecated items selected   | DefCore   |
| +1 month   | S+1       | Self-testing                         | Vendors   |
| +2 months  | S+2       | Test Flagging                        | DefCore   |
| +3 months  | S+3       | Approve Guidance                     | Board     |

Notice that the DefCore cycle is actually offset from the OpenStack release
cycle by a couple of months: generally when we gather for an OpenStack Summit
we've just ended one development cycle with a new stable release of OpenStack
and are starting another.  By contrast, the DefCore Committee is smack in the
middle of it's work cycle, and uses the Summit as a chance to publicize a
reasonably solid draft of it's proposed next Guideline to the community.  The
actual approval of that Guideline doesn't happen until three months later.

Because Guidelines don't solidify until near the end of the development cycle,
the DefCore Committee has an opportunity to evaluate decisions the community
has made regarding the direction of the code: for example, if Keystone were
going to deprecate the v2 API, they'd almost certainly have decided on
doing so well in advance of the summit and could communicate that intent
to the Committee.  DefCore would then have the opportunity to deprecate the
corresponding Capabilties from it's draft Guideline.  You might recall that in
order to make a new Capability required in a Guideline, it has to spend
[one six-month cycle in advisory status
first](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/2015A.rst#n113).
The same waiting period *doesn't apply to deprecation*.  That's because when
we add a new Capability, it's reasonable to give vendors some time to either
update their products to support that Capability or tell us why they think
that Capability shouldn't actually be required.  When we deprecate a
Capability though, vendors don't actually have to do anything.  All it
means is that they're no longer *required* to offer the Capability.  If they
want to provide it anyway, there's no prohibition against doing so and
therefore no waiting period is necessary.

It's important to remember that DefCore's Guidelines are generally
trailing indicators of market acceptance (which I wrote about in 
[this earlier post](../defcore-misconceptions-1/)).  So when a project wants
to deprecate a Capability that's currently required, it's generally a good
idea to tell the end-user community about it.  That's why we actually have a
"deprecated" state in Guidelines rather than simply ommitting them from the
document entirely: it helps call out that a thing that was required in the
past no longer is.  In some respects, a Capability being required can also
help foster an informed decision for a project about when to deprecate
something: since the DefCore Committee is a trailing indicator, the Committee
can generally offer the project some information along the lines of "you know,
an awful lot of client toolkits are still using that version of the API, so if
we remove it they're going to be up a creek".  That doesn't necessarily mean
the project won't or shouldn't deprecate something (maybe deprecation is how a
project wants to signal externally developed toolkits that it's time to start
using the newer API, for example), but it does help them make an informed
decision.

Before we wrap up, we should probably say a word or two about standards and
deprecation.  DefCore's Guidelines effectively are standards for
interoperability among OpenStack Powered(TM) products in that they require
that all products bearing the name/logo expose certain capabilities in a way
that people can rely on them and code against.  One way to measure the success
of an interoperability standard is by how many products adhere to it.  If the
DefCore Committee rolls out a new Guideline and only one or two vendors ever
test against it, the standard isn't all that useful in that end users only
have one or two products to choose from that they know expose the capabilities
in the Guideline.  Another way to measure the success of an interoperability
standard is by how many end users use it.  If Capabilities are dropped from
Guidelines frequently, end users may get frustrated that they keep having to
change their code because Capabilties they depended on have gone away.  In
that sense, we generally want to be careful about churn in DefCore Guidelines.
That's not new information for the OpenStack developer community either.  The
Technical Committee actually recently
[created a
tag](http://git.openstack.org/cgit/openstack/governance/tree/reference/tags/assert_follows-standard-deprecation.rst)
to help people understand which projects follow a standard deprecation policy.
If you read the criteria for projects that get the tag, one of them actually
says that the project will
["take into account defcore capabilities deprecation
schedule"](http://git.openstack.org/cgit/openstack/governance/tree/reference/tags/assert_follows-standard-deprecation.rst#n65).
That's good news for everyone and helps the Guidelines accurately reflect the
direction the project is taking.

*Questions?  Comments?  Drop by #openstack-defcore on IRC or drop the
DefCore Committee a line at
[defcore-committee@lists.openstack.org](mailto:defcore-committee@lists.openstack.org).*
