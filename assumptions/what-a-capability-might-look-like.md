# What a Capability Might Look Like

*A guess. See [README.md](README.md) for what that means.*

One thing an organization can do, described in enough detail that something could decide whether
to use it — and whether it's allowed to.

## The shape

```yaml
id: hotel.room.reserve       # this organization's own name for it, not a global one
kind: immediate
version: 1.0

title: Reserve a hotel room
description: Hold a room at a named property for given dates.

# what this actually is, in words other systems might use
semantics:
  domain: hospitality
  outcome: reservation
  verbs: [reserve, book]
  aliases: [room booking, hotel booking]

input:
  property_id:   { type: string,  required: true }
  check_in:      { type: date,    required: true }
  check_out:     { type: date,    required: true }
  guests:        { type: integer, required: true }

output:
  reservation_id: { type: string }
  status:         { type: string }
  total_price:    { type: money }

interaction:
  type: rest
  endpoint: https://api.examplehotels.com/reservations

authentication:
  type: oauth2
  scope: reservations.write

# the part that isn't an API description
terms:
  reversible: true
  cancellation_window: 24h
  costs_money: true
  human_approval_required: false
  idempotent: true
  rate_limit: 10/minute
  typical_response: 2s
```

## The part I actually think is interesting

Everything above `terms` is an interface description, and OpenAPI already does that better.

`terms` is the bit I keep coming back to. Before something acts on your behalf, it needs to know
things that don't appear in any function signature: does this spend money, can it be undone, how
long do I have to undo it, is a person supposed to confirm this, and what happens if I
accidentally ask twice.

A human gets all of that from a confirmation screen — the wording, the big red button, the
"you'll be charged" line underneath. That information exists, it's just been encoded in
presentation for the last thirty years because a person was always there to read it.

Which suggests this file isn't really describing a function. It's stating the conditions under
which an organization will accept a request: *if you come to me wanting a room, here's what I
need, what it'll cost, and whether a person has to say yes.*

## The name is local, and that's deliberate

`id` is whatever this organization calls this thing. It is not a claim on a shared vocabulary,
there is nothing to register, and nobody has to agree with anyone.

I spent a long time trying to design a global naming scheme before concluding it was the wrong
problem. If the thing reading this file can tell that "book me a table", "reserve a table for
four" and "get us in at eight" are the same request, it can also tell that my `table.reserve` and
your `booking.create` are the same capability. Matching happens on `semantics` and the
description — on meaning, not on the string.

That's why `semantics` exists and why it matters more than the id does: the domain, the outcome,
and the words people actually use to ask for it.

The honest consequence is that the most technical-looking field here is doing the least work.
Every attempt to agree a global vocabulary for what the world can do has failed — see
[../WHY_THIS_ISNT_UDDI.md](../WHY_THIS_ISNT_UDDI.md) — so this design just doesn't try.

The obvious objection is that loose matching is fine for a dinner reservation and alarming for a
bank transfer — and that the expensive cases are much of the reason any of this seemed worth
thinking about. That deserved more than a shrug, so it has its own document:
[when-being-wrong-is-expensive.md](when-being-wrong-is-expensive.md). Short version: the `terms`
block does more work there than the name does, and where real precision is needed the vocabulary
usually already exists and isn't ours to invent.

## What this shape can't express

A capability description says *what I can do and on what terms*. It doesn't say anything about
going back and forth.

> *No capacity Thursday.* — *Friday, same price?* — *Not at that weight.*

That's most of what real interaction between two companies consists of, and none of it fits here.
Whether the description is the necessary first step before a conversation, or a small part of a
problem that's mostly conversation, is discussed in
[../WHATS_ALREADY_SOLVED.md](../WHATS_ALREADY_SOLVED.md). I don't know which.

## Things that take six weeks

Everything above assumes the act completes while you wait. Book a table, create a VM, transfer
money. Request in, result out.

Plenty of what organizations actually do doesn't work like that at all. Apply for a residence
permit. Apply for a mortgage. File an insurance claim. Get a referral to a specialist. These
are the cases where a person most wants help, and the shape above describes none of them.

They differ in ways that matter:

- The answer arrives weeks later, not seconds later.
- More information gets requested partway through, and you can't know in advance what.
- There's a state you need to be able to check.
- The outcome is discretionary. Submitting is not obtaining.
- A human decides, and they can say no for reasons that aren't in any rulebook.

So `kind` distinguishes them, and a `process` carries different fields:

```yaml
id: immigration.residence-permit.apply
kind: process

stages: [submitted, documents-requested, under-review, decided]

check_status:
  interaction: website
  url: https://example.gov/status

typical_duration: 6-10 weeks
may_request_more: true          # and you can't know what in advance

outcome:
  guaranteed: false
  discretionary: true           # a person decides, and may refuse

terms:
  costs_money: true
  refundable: false             # the fee is not returned if refused
  human_approval_required: true
  reversible: false
  appeal_possible: true
```

The most important field there is `outcome.guaranteed: false`. Something acting on your behalf
needs to understand the difference between "I did the thing" and "I asked, and we'll find out."
Conflating those is how an assistant tells you your permit is sorted when it has done nothing of
the kind.

I'm not confident this is right. It might be that long processes need a different design
entirely rather than a variant of this one, and that trying to cover both in one shape produces
something that serves neither. But leaving them out felt worse, because it would mean quietly
designing only for the cases that already work well.

## What I'm least sure about

- The whole naming scheme. `hotel.room.reserve` vs `hospitality.accommodation.book` — I've
  written both. Nobody owns the left-hand side of that name and I don't think anybody should,
  which leaves the question of how it ever stabilises.
- Whether `terms` is a short list or an endless one. Every real business has conditions that
  don't fit here, and the moment you start adding fields for them this stops being small.
- Versioning. `1.0` is written there out of habit. I haven't thought about what a breaking change
  to a capability even means when the thing consuming it is a language model.
- Whether pricing belongs here at all. `costs_money: true` is nearly useless, and anything more
  precise is a promise most organizations won't make in public.
