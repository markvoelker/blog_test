+++
title = "DefCore Misconceptions Part 1: Leading vs Trailing Indicators"
author = "markvoelker"
date = "2015-09-25"
url = "/defcore-misconceptions-1"
socialsharing = true
description = "Did you know DefCore is more of an indication of what the market has determined to be interoperable than a visionary standard that tells the market what to think?"
categories = ["DefCore","OpenStack","Misconceptions"]
+++

DefCore Misconceptions Part 1: Leading vs Trailing Indicators
=============================================================

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

One item of feedback we've received a couple of times now is that some
folks expect the DefCore Committee to be a visionary body that sets
forward-looking standards that others will be expected to adhere to.  In
essence: DefCore defines what makes a cloud interoperable, and then
everyone else gets to work implementing those capabilities in their
products. 

**This actually isn't the case (today).**

Well, more accurately: it's only partly true.  DefCore does set standards
that vendors have to adhere to if they want to use the OpenStack
trademark or logo...but most of how those standards are defined is by
looking at what the market has already accepted.

The criteria that the Board of Directors has approved for DefCore to use
in establishing interoperability standards today are strongly weighted
toward requiring capabilities that have *already* gained wide
adoption--not what we’d like the world start adopting.  In essence,
DefCore's Guidelines are *trailing indicators* of the acceptance of a
feature.  The evidence of this lies in the 12 Criteria that DefCore uses
to decide which Capabilities become required in it's Guidelines.
Currently, all 12 Criteria are weighted roughly equally.  If we examine
them to see whether they're more of a leading indicator or a trailing
indicator, we find:

* [Widely Deployed](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n40): **trailing**.  No points are awarded for this
  Criteria unless products are already supporting the Capability in
  question.
* [Used by Tools](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n44): **trailing**.  Tools are mostly unlikely adopt a new
  feature/API the minute it’s released.  Rather, they tend to lag and
  add support for it when the tool's users demand for it is sufficient.
* [Used by Clients](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n48): **trailing**.  See Used by Tools.
* [Future Direction](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n54): **leading**.  If the project (for example, Nova) or
  the TC says a Capability is going away in the future, no points are
  awarded even if the Capability isn't gone yet.
* [Complete](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n61): **trailing** (sort of).  If the capability is plugin dependent,
  all plugins have to support it.  That doesn't always happen immediatly
  when a feature is released (unless the project requires it) so we'll
  call that a trailing indicator.  
* [Stable](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n58): **trailing**.  The capability has to have been present in 3 or
  more OpenStack releases, which means the capability has to have been
  released for 18 months before it gets points here.
* [Foundational](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n84): **trailing**.  Other capabilities are unlikely to
  be dependent on a very new thing.  In fact there are numerous examples
  of the opposite: for instance, until around the Juno timeframe
  neutronclient (on which any Neutron capabilties that require
  authentication would depend) supported only the Keystone v2 API.  The
  Keystone v3 API was introduced as far back as Grizzly.
* [Atomic](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n87): **neither**.  
* [Proximity](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n90): **neither**.
* [Discoverable](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n70): **neither**.
* [Documented](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n73): **neither** (or maybe in some instances trailing).  In
  most cases major new capabilties get documented when released (the
  community has generally gotten a lot better about this over the years
  IMHO).  There are still some cases where a capability isn't well
  documented up front though, and does get documented at some later
  point.  In that sense you might call this a trailing indicator.  
* [DefCore in Last Release](http://git.openstack.org/cgit/openstack/defcore/tree/doc/source/process/CoreCriteria.rst#n77): **trailing**.  A capability only scores
  points here if it was required six months ago in the last DefCore
  Guideline as well (which also means the Capability was likely present in at
  least three OpenStack releases per the "Stable" criteria, so in
  essence the Capability is at least two years old if it gets points
  here).

So of the 12 criteria, 7 are looking at trailing indicators of adoption,
4 aren’t really either, and 1 is a leading indicator.  It’s
also maybe worth pointing out that using current levels of real-world
interoperability as the starting point has been [discussed by longtime
Board members since at least 2013](https://blogs.gnome.org/markmc/2013/10/30/openstack-core-and-interoperability/),
so the idea that DefCore is weighted toward trailing indicators of 
adoption isn't new.  That said, adherence to DefCore’s Guidelines only
became a requirement to get a trademark/logo license agreement from the
OpenStack Foundation in the past 6 months or so, so we probably now have
a lot more people now paying attention and interested in providing feedback.  

Some of those discussions have indicated that at least part of the
community feels that the current criteria are perhaps *too* heavily
weighted toward trailing indicators.  Personally, I tend to agree.  To
that end, [some](https://review.openstack.org/226978)
[discussions](https://review.openstack.org/226980) have already begun
about proposing changes to the Board of Directors concerning weighting of
criteria.

*Questions?  Comments?  Drop by #openstack-defcore on IRC or drop the
DefCore Committee a line at
[defcore-committee@lists.openstack.org](mailto:defcore-committee@lists.openstack.org).*
