# Why This Isn't UDDI

If you know the history, "this is UDDI" is the first thing you'll think, and you'll be right about
the shape. It's worth me saying up front that I know, and saying where I think the difference is —
so we can argue about the difference instead of about whether I've heard of it.

I should also be straight about my sources. I haven't worked on any of this. What follows is the
account I've been able to piece together from what's written about it. **If you were actually
involved in UDDI, I'd very much like to hear from you** — you'd be correcting me from primary
experience, which is worth more than anything in this repository.

## What UDDI was

Universal Description, Discovery and Integration. Announced in 2000 by Ariba, IBM and Microsoft;
a public registry — the UDDI Business Registry — ran as replicated nodes operated by IBM,
Microsoft and SAP.

You'd look up what a business could do, get back a description of the service and how to bind to
it, and go use it. Entries were organised against standard classification systems: industry codes,
product and service codes, geography.

The public registry was shut down in January 2006. The specification went to OASIS. Private
registries inside enterprises carried on for years.

## What's the same

Honestly: quite a lot of the shape.

A machine-readable description of what an organization can do. A central index of who claims what.
A naming or classification scheme so two parties can mean the same thing. And the same core
promise — that two parties with no prior relationship could find each other and start working
together.

`businessEntity → businessService → bindingTemplate` and
`organization → capability → interaction` are not far apart. If you sketched both on a whiteboard
you'd draw something similar.

So the shape isn't the difference. Something else has to be.

## The fact I keep coming back to

The detail that changed how I read this: **the public registry died and the private ones
survived.**

Inside a company, or between partners who already had contracts, a UDDI registry was useful
enough to keep running. The open, anyone-can-find-anyone version is the part that emptied out.

Which means the piece that failed was specifically *discovery between strangers*. Not the data
model, not the XML, not the vendors. The premise.

And I think the reason is that in 2001 nobody needed it.

## Why the consumer matters more than the format

UDDI's actual user was a developer, at design time. A person sat down, looked up a service, read
its description, wrote code against it, and shipped.

But that person had better options. They could read the documentation. They could email someone.
They could ring the vendor. Programmatic discovery of an unknown business partner solved a problem
developers didn't have — and the parts of a partnership that are genuinely hard (contracts,
liability, pricing, trust) were never going to be settled by a registry lookup anyway.

The thing I'm betting on is that this consumer is now different, in a way that didn't exist then:

- It arrives at runtime, not design time.
- It has never seen this organization before and can't be pre-programmed for it.
- It can't ring anyone.
- And there may eventually be a great many of them.

If that consumer doesn't materialise, then this idea fails the same way UDDI did and for the same
reason. I'd rather state that plainly than pretend the precedent doesn't apply.

## Three other things that are different

**Ambiguity is now cheap.** What killed UDDI's classification — and Yahoo's directory — was rigid
matching. If a service was filed under one term and you searched for another, you got nothing. The
whole taxonomy problem was that meaning had to be exact.

Language models are good at exactly this. The single hardest part of the old approach is the part
that got much easier, and that's the most substantial change since 2001.

**It doesn't need everyone at once.** This is the difference I most often see missed, because
people apply UDDI's failure mode without checking whether it transfers.

UDDI was a *directory*. A directory has no value until it has many entries — one company in a
registry helps nobody, so everyone has to arrive at roughly the same time, so nobody does. That's
the chicken-and-egg problem that emptied it out, and it's a real one.

What's described here isn't a directory. One organization publishes a description; one system
reads it; those two get value immediately. Nobody else has to participate, and there's nothing to
join. It's closer to a fax machine than to a phone book — useless in isolation, useful the moment
there are two.

The two sides aren't even symmetric. Publishing takes some work. **Reading takes none** — a
description at a known address is something a model can already consume today, with no adoption,
no library, and no coordination. So only the publishing side has to accumulate, and each new
publisher gets whatever value exists on day one rather than waiting for a network to form.

That doesn't make it free. The first organization to publish still has to believe someone will
read it, and "some work now, uncertain benefit later" is how most conventions die. But it is a
much weaker problem than a registry's, and treating them as the same thing overstates the case
against this by quite a lot.

**The bar to appear is lower.** UDDI assumed you had a web service. No SOAP endpoint, no entry —
which excluded almost every organization on earth by construction. A restaurant that takes
bookings by telephone can appear in the shape I've drawn. That's not a detail; it's most of the
world.

## Where the analogy still bites

I don't think I get to walk away clean.

**The demand may still not be there.** My entire case rests on a consumer that mostly doesn't
exist yet. UDDI shows that a reasonable design for an absent need goes nowhere regardless of who
backs it — and IBM, Microsoft and SAP could push harder than anyone reading this.

**A registry in the middle has a poor record generally.** Not just UDDI. The pattern of "central
place that knows who can do what" has lost to "read the messy thing directly" more than once, most
notably to search. There's no registry here, but anything that ends up crawling these
descriptions starts to look like one, and it would inherit the same history.

**And there's a tension inside my own idea.** If a model can resolve `table booking` to
`hotel.room.reserve` through aliases and context, why does the structured name exist at all?
Either the model is good enough and the naming scheme is decoration, or it isn't and the naming
scheme fails the way UDDI's classification did. There may be a sensible middle — names as a coarse
filter, meaning for the fine matching — but I haven't made that argument yet, and until I do this
is the weakest joint in the whole thing.

## What would settle it

**Why the public UDDI registry really emptied out.** If it was the taxonomy or the XML or the
vendor politics, that's survivable. If it was that organizations fundamentally don't want to be
programmatically discoverable by strangers — that's not a technical problem and it hasn't changed.

**Whether descriptions built from the outside are worth anything.** Testable cheaply, with the
prompt in [assumptions/](assumptions/).

**Whether the naming layer is load-bearing or ornamental.** Also testable: try matching requests
to capabilities with the structured names removed and see if anything is lost.

## The others worth knowing about

**WSDL, SOAP, the WS-\* stack.** Rich descriptions that survived in enterprises and lost the open
web to REST, which described far less and was easier to start. Descriptive richness tends to lose
to ease of adoption.

**The Semantic Web.** Decades of serious work on machine-readable meaning by people who thought
about it far harder than I have. The narrow, incentive-backed parts persist; the general vision
mostly didn't arrive.

**Yahoo's directory.** A curated taxonomy of the web, beaten by a search engine that ignored
taxonomy and read the pages. The sharpest objection to any naming scheme.

**schema.org.** The one that worked, and the useful counter-example. Publishers got something
immediate back — better representation in search — and there was a party with both the reach to
define the vocabulary and the leverage to reward using it. I can't name that party here, which is
why the bootstrap has to come from indexing rather than from anyone's goodwill.

**`robots.txt`.** Not prior art for the idea, but for the method. Nobody designed a system for
crawlers; a tiny informal convention appeared in 1994 and spread because it was trivial and solved
an immediate problem. It's also the only thing here that got *refusal* right, and that's the part
worth keeping.
