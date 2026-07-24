# When Being Wrong Is Expensive

*An open question, and the one I'd most like help with.*

The rest of these notes say that capability names are local — an organization calls things
whatever it likes, and whatever is reading the file works out what's what from meaning rather
than from an agreed vocabulary. See
[what-a-capability-might-look-like.md](what-a-capability-might-look-like.md).

That's fine for booking a table. Match the wrong thing and you've got a reservation you don't
want.

It's a different proposition for moving money, proving who someone is, authorising a medical
procedure, or signing something binding. And those aren't edge cases I can defer — they're a
good part of why this seemed worth thinking about at all. An assistant that can book restaurants
but has to hand back to a person for anything consequential hasn't changed much.

So: how do you build on loose matching when a mistake is expensive?

## Two questions that got tangled together

I kept treating this as one problem. It's two, and separating them helped.

**Did it pick the right thing?** The request was "send 1000 to my landlord" and something had to
decide which capability that is. This is a matching problem, and matching is probabilistic.

**Does it understand what will happen?** Whatever it picked, does it know this spends real money,
can't be undone after ten seconds, and needs a person to say yes first.

Almost everything I'd been worrying about was the first question. But the damage is done by the
second. A wrong match that gets caught costs nothing. A right match executed without anyone
understanding the consequence is how people lose money.

## Which means the terms block is doing more work than I realised

The fields that say `costs_money`, `reversible`, `human_approval_required`, `idempotent` aren't
just useful metadata. They're what makes local naming survivable, and they work for a specific
reason:

**The organization sets them, not the caller.** If a bank marks a transfer as requiring human
approval, nothing reading the file can decide to skip that. The safety property doesn't depend on
the calling system being careful, well-built, or honest — which is good, because you have no
control over what's calling you.

That inverts the usual worry. The question stops being "is the matching good enough to trust
with money" and becomes "does the design still hold when the matching is wrong." For anything
consequential, it has to.

## Restating before acting

The other half is that a system should say what it's about to do before doing it — but using the
capability's own description, not its own paraphrase of the request.

The difference matters. If the assistant says "sending 1000 to your landlord", that's just
repeating the instruction back and proves nothing. If it says "this will start an international
transfer, arriving in 2-3 working days, with a 15 EUR fee, which cannot be cancelled once
submitted" — and that text came from the file — then a wrong match shows up as a wrong
description.

Which suggests the description isn't only for machines to match on. It's what a person reads at
the moment they decide.

## Where that isn't enough

Here's the failure I can't design away, and I'd rather write it down than leave it out.

Someone says "transfer 1000 to this account". There are two capabilities and both are plausible:
a domestic transfer and an international one. They differ in fee, in settlement time, and in
whether they can be recalled. The restatement sounds entirely reasonable either way, because the
person never specified which one they meant — they don't think in those terms.

Echoing back doesn't catch this. The person can't catch it either, because nothing looks wrong.
The distinction that matters is one they don't know exists.

I think this generalises: **meaning-matching fails badly where the important distinctions are
ones the requester doesn't know to make.** Assurance levels in identity. Which insurance product
a claim falls under. Whether a medical appointment needs a referral. These aren't ambiguities in
the request — they're ambiguities the domain contains and the requester has no way to resolve.

## The part I think I got wrong

My first instinct was that this means agreed names are needed after all, at least in the
expensive domains. I now think that's the wrong conclusion, for a reason I like:

**In most of these domains, agreed vocabularies already exist, and they aren't ours to invent.**

Payments have ISO 20022 and the scheme rules that go with it. Identity has assurance levels
defined by regulators — eIDAS in Europe, NIST's levels in the US. Healthcare has coding systems
that predate all of us. These are already agreed, already enforced, and already the thing
practitioners argue in.

So where precision is required, the answer probably isn't a new namespace. It's referencing the
vocabulary that already governs that field:

```yaml
id: payments.transfer.international     # local name, means nothing outside this file
kind: immediate

conforms_to:
  scheme: SEPA
  message_standard: ISO 20022
```

That keeps the "no authority, nothing to register" property intact — we never define a
vocabulary, we point at ones that exist and are maintained by people with actual jurisdiction.
And it fails honestly: if a capability claims conformance to a scheme it doesn't follow, that's a
lie a regulator can act on, not an ambiguity.

## What I don't know, and would like to discuss

Genuinely open, in roughly the order they bother me:

**Is "the organization sets the safety terms" enough?** It assumes organizations mark things
accurately. Some will understate — approval requirements are friction, and friction loses
conversions. What stops a merchant marking an irreversible purchase as reversible? Nothing in
this design does.

**Should the caller be able to demand more caution than the file offers?** A person might want
confirmation on everything above 100 EUR regardless of what the organization thinks. That's a
setting on their side, not the organization's — but then the two sides have policies that can
conflict, and I don't know who wins.

**Where's the line for referencing an existing vocabulary?** Payments and identity, clearly. What
about shipping, ticketing, energy contracts? Say "conforms to nothing in particular" too often
and precision quietly leaves the design.

**Does the plausible-but-wrong problem have a solution at all, or is it just a cost?** Human
intermediaries have this problem too — a travel agent can book you the wrong fare class. We
handle it with liability and recourse rather than with better matching. Maybe that's the honest
answer here, and this is a legal question wearing technical clothes.

**Is there a category of thing that simply shouldn't be reachable this way?** I don't have a
principled test for what belongs on that list, and "everything, eventually" and "nothing
important, ever" both seem wrong.

If you work in payments, identity, or anywhere the cost of a wrong match is measured in money or
harm, I'd rather hear from you than from almost anyone. This is the part of the idea I'm least
qualified to have opinions about and most likely to be confidently wrong on.
