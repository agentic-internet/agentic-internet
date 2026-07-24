# What's Already Solved

Most of this problem is solved. It's worth being precise about which parts, because the leftover
piece is small and specific, and if I can't say clearly what it is then there probably isn't one.

## Inside one organization: solved

Connecting a model to tools you control is in decent shape. MCP does it, tool-calling does it,
plenty of frameworks do it. You write definitions, hand over credentials, and it works.

What makes that tractable is everything you already have before you start:

- You chose the tool.
- You hold the credentials.
- You know what it does, because you or a colleague built it.
- You've decided to trust it.
- If it breaks, there's someone to ask.

None of that survives contact with an organization you've never dealt with. The interesting
question isn't how a model calls a tool — that's settled. It's how anything gets to the point of
having a tool to call.

## Between organizations, with a prior relationship: also solved

This one is older and less fashionable, and people forget it exists — but it's the strongest
"this is already solved", so it's worth being concrete about.

EDI (Electronic Data Interchange) is how two companies' computers exchange business documents —
orders, shipping notices, invoices — directly, in an agreed format, with no human retyping
anything. A supermarket's system sends an order straight to a supplier's system, which reads it
automatically. It's been running since the 1970s, on standards most people have never heard of
(ANSI X12, EDIFACT), and enormous volumes of world trade still move on it every day in retail,
logistics and healthcare. It is genuinely machine-to-machine, genuinely cross-company, and it
works.

So anyone who has worked in supply chain will read this repository and think "that's just EDI",
and it's worth saying plainly why it isn't.

The whole thing assumes the two companies already know each other. Before any documents flow,
someone negotiates the relationship, someone maps this company's fields onto that company's
fields, someone sets up the connection — usually paying an intermediary for the privilege. That
setup takes weeks and is done once per partner. After it, everything is automatic and runs for
years.

Which means EDI answers "how do these two companies, who have agreed to work together, exchange
documents" — not "how does a company deal with one it has never set up anything with". Same shape
as the API problem, one level up: it solves the relationship, not the first contact.

That EDI succeeded where UDDI failed cuts both ways, and the second edge is the sharper one. It
proves cross-company machine interaction is genuinely valuable — good. But it also proves that
enormous value gets captured without ever solving discovery, which is a fair argument that
discovery was never really the bottleneck. The honest reading is that EDI shows companies *will*
pay for expensive setup when the volume justifies it. So the gap this repository is about isn't
the high-volume, ongoing relationship EDI already serves. It's the low-volume, occasional, or
one-off case — the carrier you use once, the supplier you try in an emergency, the small operator
nobody will build an integration for. That's a narrower space than "the future of commerce", and
it's the honest scope: the long tail EDI was never worth setting up for.

Something much closer to what this repository describes *was* tried, at the height of the web
services era, and shut down — along with several other attempts to make organizations
machine-readable to strangers. That history is worth its own page:
[WHY_THIS_ISNT_UDDI.md](WHY_THIS_ISNT_UDDI.md).

## Between organizations, without one: worked around

This is the part I think is unsolved, and I want to be honest that "unsolved" is too strong.
There's a working arrangement. It's just expensive and someone else is paying for it.

An e-commerce company that wants forty carriers doesn't integrate forty carriers. It pays an
aggregator that did it once and sells it as one interface. Shippo, EasyPost, and their equivalents
in payments, banking and travel. The pattern is the same everywhere: someone absorbs N
integrations, exposes one, charges for it.

That's a real solution and I don't want to be dismissive about it. It works today, it needs
nothing from the carriers, and it's a viable business.

But it has properties worth noticing:

- **The fragmentation is the product.** If carriers all described themselves the same way, the
  aggregator's main asset would evaporate. The party best positioned to fix this is the one with
  the most to lose from it being fixed.
- **Coverage stops where the business case stops.** Aggregators integrate the carriers worth
  integrating. The small regional one, the specialist, the operator in a market too small to
  bother with — those stay invisible, permanently.
- **It's per-vertical.** Shipping has aggregators. Payments has aggregators. Municipal permitting
  does not, and probably never will, because nobody can make the numbers work.

So the gap isn't that cross-organizational interaction is impossible. It's that it currently
requires either a negotiated relationship or someone standing in the middle taking a cut, and both
of those scale with money rather than with need.

### "Whoever crawls all this just becomes the new aggregator"

This is the sharpest objection I've had, and the first thing a reader with any commercial sense
says. It's partly right and I think it misses something.

Partly right: if you want to ask forty carriers the same question and compare, something has to
have read all forty descriptions. That something is an aggregator. Aggregators don't disappear.

What it misses is what the aggregator *owns*. Today the moat is the integrations — forty bespoke
pieces of work, each expensive, each needing maintenance. That's genuinely hard to replicate,
which is why the margin holds. If every carrier published the same kind of description, the moat
becomes the crawl, and crawling is cheap. Anyone can do it. So aggregation survives and the
margin collapses.

That's not a hypothetical: it's Yahoo's directory against a search engine that read the pages
itself. Aggregation didn't go away — it got faster, wider, and nearly free, and the money moved
somewhere else.

The other half is that plenty of interactions don't need comparison at all. A buyer who already
knows which regional haulier they want, a clinic dealing with one specific insurer, a contractor
applying to one specific council — none of those need an index, and today none of them have a
path either. The N-way comparison case is the one that needs a middleman. The
"work with this particular unfamiliar party" case doesn't, and it's the more common one.

## The part I think is left

Stated as narrowly as I can:

**An organization saying, in public and without being asked, what it can do and on what terms —
so that something that has never dealt with it can find out without a person reading a page.**

That's it. Not how to call it, which APIs handle. Not how to exchange documents once you're
set up, which EDI handles. Not how a model uses a tool, which MCP handles. Just the introduction.

If that gap isn't real — if the introduction was always going to be done by humans and by
aggregators, and that's fine — then this idea has nothing in it. That's a legitimate position and
I'd like to hear it argued properly.

## What this doesn't cover, and I should say so

**Negotiation.** Everything I've described is a description being read. Real cross-company
interaction involves going back and forth: no capacity on that date, what about Thursday, what's
the price at that volume, we'll need it insured. A static file is the opening move, not the
conversation. I don't have a design for the conversation, and I'm not sure the file-based approach
extends to one.

**Commitment.** When two systems agree something, what makes it binding, and what happens when one
of them was wrong? People handle this with contracts and liability, not with formats.

**The multi-hop case.** A device acting for a household acting for a person, with a payment method
belonging to someone else. The chain of "on whose behalf" gets long, and nothing here addresses
how authority travels along it.

These are real gaps rather than details, and the first one is large enough that it may turn out to
be the actual problem, with everything I've written being a prerequisite to it.
