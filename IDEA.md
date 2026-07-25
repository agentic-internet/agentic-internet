# The Idea, In More Detail

This is the longer version of what's in the README. It's where I've got to by thinking about it,
not a design document. Where I'm unsure, I've said so instead of smoothing it over.

## It's not about agents

I started out thinking about agents — how they identify themselves, how they get listed, how they
find each other. It went nowhere.

The agent isn't the interesting part. From an organization's side, two different systems asking
for the same thing aren't meaningfully different. What matters isn't *who is asking*, it's **what
is being asked for**.

So the thing in the middle isn't a directory of agents. It's a description of what can be done:

```
Need → Capability → Organization → Interaction → Execution
```

You start from what someone needs, not from who might provide it. If that ordering is wrong, most
of what follows is wrong too.

## Naming, and why I stopped worrying about it

For a long time I thought this needed a shared vocabulary. Structured names, agreed across
everyone:

```
restaurant.table.reserve
hotel.room.reserve
cloud.compute.vm.create
```

I spent more time on this than on anything else, and I now think it was the wrong problem — or
at least, that I was solving it at the wrong level.

The trouble with a shared vocabulary is that somebody has to own it. Somebody decides whether a
dentist appointment is `healthcare.dentist.appointment.book` or `healthcare.appointment.book`
with the specialty as a parameter. I've written it both ways on different days, and every
mechanism for settling that argument at world scale has failed —
[see the history](WHY_THIS_ISNT_UDDI.md).

Here's what changed my mind. If the thing reading these files can already tell that "book me a
table" and "reserve a table for four" and "get us in at eight" are the same request, then it can
also tell that my `table.reserve` and your `booking.create` are the same thing. The matching
happens on meaning, not on the string.

So: **names are local.** An organization calls its capabilities whatever it likes. The name is
there so the file is readable and so it can be referred to consistently — not as a claim on a
global vocabulary. There is nothing to register and nobody to agree with.

That does mean the name is carrying much less weight than it looks like it's carrying, and the
real work is done by the description. Which is a strange thing to arrive at, since it means the
most technical-looking part of this is the least important part.

The fair challenge is that loose matching sounds fine for a restaurant and reckless for a bank —
and that the expensive cases are much of the point. I think the answer is that the safety comes
from the terms an organization attaches to a capability rather than from the precision of its
name, and that where precision genuinely is required, the vocabularies already exist and belong
to regulators rather than to me. It's worked through, with what's still unresolved, in
[when-being-wrong-is-expensive.md](assumptions/when-being-wrong-is-expensive.md).

## A name isn't enough

To be usable, a capability needs to carry more than a label — what it needs, what it returns,
what it requires from you first, and what version of it you're looking at. The shape I keep
drawing is in
[assumptions/what-a-capability-might-look-like.md](assumptions/what-a-capability-might-look-like.md).

Read plainly, that's an interface description, and that's fine. Two things are actually being
claimed on top of it.

First, that the description belongs to the *capability* rather than to any one provider — that
`cloud.compute.vm.create` means roughly the same thing whichever provider answers it. That's a
big claim and I haven't earned it.

Second, and I think more interesting: what's being described isn't really a function. It's the
terms under which an organization will accept a request. *If you come to me wanting a room
reserved, here is what I need, what it costs, how fast I'll answer, whether a person has to
approve it, and what happens if you ask twice.*

That last one matters more than it sounds. A thing acting on your behalf needs to know, before it
acts, whether an action is reversible, whether it costs money, and whether a human is supposed to
confirm. None of that is in an API signature, and all of it is the sort of thing a person infers
from a confirmation screen.

## Nobody knows who can do what

Here's a way of putting it that I find clarifying.

The Internet already has an organization knowledge graph. Google has one, Wikipedia has one,
LinkedIn has one. Who exists, what they're called, where they are, who works there, what they
sell.

What doesn't exist anywhere, in any standard form, is a **capability graph**. Not *who is this
company* but *what can this company actually do for me right now*. That information exists — it's
scattered across pricing pages, help centres, API docs and the heads of the people who work
there — but nothing collects it, and nothing agrees on how to say it.

For a person that's fine. You read the page and work it out. It's only a problem for something
that has to decide, quickly, among a thousand organizations it has never dealt with.

## Not every organization has an agent yet — and the design has to survive that

The direction of travel is toward more agents, not fewer.
[HOW_THIS_MIGHT_UNFOLD.md](HOW_THIS_MIGHT_UNFOLD.md) is the argument: companies are building
internal agents right now, and I think that capability turns outward over time. So the endpoint I
actually expect this to reach is *your agent talking to their agent*.

But it can't *require* that on day one, or it dies before it starts. Today a few thousand
companies have a capable agent; a regional haulier has an email address and a restaurant has a
phone. If the idea only works once everyone has built software, it works nowhere.

The way out is that how you *reach* a capability is just an attribute of it, and it's allowed to
be whatever already exists — with room to upgrade as agents spread:

```
National carrier  →  interaction: agent        # today: the leading edge
Large insurer     →  interaction: rest         # today: common
Regional haulier  →  interaction: email        # today: most of the world
Restaurant        →  interaction: telephone    # and this is fine
```

The description is the same in every row. What changes is only the last step. An organization
with nothing but a phone number can be described today and reached today; the same description
points at an agent the day it has one, and nothing else changes. So the static description isn't
a rival to the agent-to-agent future — it's the on-ramp to it, and the thing that lets the world
participate before it's finished building.

That's why this can start small. Describing what you can do doesn't require changing how you do
it — and it's the same description whether the thing on the other end is a person, an API, or an
agent.

## Why an organization would bother

The version of this that needs everyone to move at once never happens. So the only interesting
question is what one organization gets from doing it alone, on a Tuesday, with nobody else
participating.

I think there are two honest answers, and one of them is smaller than it sounds.

**Being used correctly instead of guessed at.** Something is already reading your site on
someone's behalf. Right now it's inferring your cancellation policy from a paragraph of marketing
copy, and it will occasionally be wrong in ways that become your problem — a booking made that
shouldn't have been, a customer who believes something you never said. Publishing the terms
plainly is cheaper than handling the fallout, and it's the same argument that eventually got
people to publish structured data for search.

**Being reachable at all.** If a person's assistant can complete a booking with your competitor
and not with you, that's a lost booking, and you'll never see it in your analytics because it
never arrived.

Both of those depend on the premise being true — that this traffic is real and growing. If it
isn't, neither argument works and nothing here matters, which is the honest position.

### The objection I underrated for a long time

I used to think the barrier was effort. It isn't. For some organizations, being cleanly
machine-readable is *against their interests*, and that's a different kind of problem.

A website is not just information. It's framing — brand, photos, reviews, bundling, the
loyalty programme, the urgency banner, the upsell on the second screen. A capability description
strips all of that and leaves a row in a comparison. Airlines publish their fares publicly and
still spent years fighting the sites that made those fares easy to compare, because friction was
doing commercial work that the fare itself wasn't.

So the ones who'll adopt this eagerly are the ones competing on price and availability. The ones
who won't are the ones with margin to protect — which may be exactly the ones worth reaching.

I think the decentralised shape softens this a lot. Publishing a file on your own domain, that you
control and can withdraw, is much closer to publishing an API than to listing yourself in
somebody's marketplace. Organizations publish APIs happily. But the moment anyone crawls all
these files and builds the comparison engine — and someone will, because it's public — that
dynamic comes back. There's no version of this where an organization gets the reach without the
comparability.

### On generating descriptions for organizations that didn't ask

You can build one of these from the outside — from a website, pricing pages, API docs — and the
prompt in [assumptions/](assumptions/) does exactly that. As a way of testing whether the idea
holds up, that's cheap and I'd encourage it.

Publishing those at scale is a different thing and I want to be careful about it. Describing an
organization that never asked to be described isn't neutral: getting it wrong about a hospital is
not the same as getting it wrong about a pizza place, and "it was marked unverified" is a thin
defence. There's a legal edge too — systematically harvesting a company's prices and republishing
them as a product is territory airlines have litigated for years, whether or not the information
was public to begin with.

The clean version is the one where the organization publishes it themselves. That's an explicit
invitation, and it's the difference between reading `robots.txt` and ignoring it.

## Two different things, not one

The more I look at it, the more I think there are two separate descriptions here, and mixing them
up is a mistake.

**About the organization.** Who this is, how you know it's really them, what they'll vouch for,
and which capabilities they claim to have. One of these per organization, and it changes rarely.

**About a capability.** The actual detail of one thing that can be done — what it needs, what it
gives back, what it requires from you first. One per capability, and these change all the time.

Keeping them apart matters because they have completely different lifetimes and different
audiences. You'd read the first once and cache it; you'd read the second every time you actually
wanted to do something.

## Identity and trust

The bit I've thought about least, and probably the bit that decides whether any of it works.

Two directions, and they're not symmetric:

**Why would you believe the organization?** Something publishes a description claiming to be a
bank. It isn't hard to publish. Domain control gets you some of the way — it's how the Web
already handles this — but "this really is that company's domain" and "this company will actually
do what it says" are different claims.

**Why would the organization believe you?** Something arrives asking to move money on behalf of a
person. Who authorised that, how far does it go, when does it stop, and how does the receiving
side check any of it? Human institutions have handled delegation for centuries with paperwork and
liability rather than technology, which makes me think the interesting part here might not be
technical either.

I don't have answers to either. I mention them because a description of what an organization can
do is useless — worse than useless — if nobody can tell whether to believe it.

## There is no registry in this

Worth being blunt about, because this is where ideas like this usually go wrong and where mine
was heading for a while.

I don't want to build a central directory. Not as a business, not as infrastructure, not as a
thing anyone has to join. A central index of who can do what becomes an authority the moment it
becomes useful, and then everyone has to negotiate with it. That's how you get UDDI.

What I'm actually describing needs no new infrastructure at all:

```
example.com/robots.txt          →  one line pointing at the rest
example.com/.well-known/agent   →  who we are, what we can do
example.com/.well-known/...     →  the detail of each thing
```

DNS already answers "where is example.com". HTTPS already answers "is this really them".
`robots.txt` is already the file everyone uses to talk to non-human visitors. Nothing new has to
exist for this to work — an organization adds a line and publishes some files, exactly the way
sitemaps happened.

If anyone later wants to crawl all of it and build a search index on top, they can. That's their
project, not this one, and it's the same relationship search engines have with websites. The
important part is that nothing depends on that index existing.

The shape of the files is in [assumptions/](assumptions/), including
[how it would be announced](assumptions/how-this-would-be-announced.md).

## What it looks like end to end

A shipping system has a consignment to move. Its usual carrier has no capacity, and there's a
regional operator it has never used.

```
find the domain                  search, a directory, a recommendation — however anyone finds it
        ↓
read /robots.txt                 Agent: https://carrier.example/.well-known/agent
        ↓
read the agent file              who they are, what they can do, whether they take
                                 automated requests at all
        ↓
read the capability              what a quote needs, what it commits you to, whether
                                 booking is reversible, what they require before acting
        ↓
act                              their REST API. Could equally be a form, or a
                                 phone number for a human to ring.
```

Nothing in the middle. No aggregator, no prior contract, no integration project. And notice which
step disappeared: nobody read documentation and wrote code. That step is the entire cost today,
and it's paid per carrier.

Swap the carrier for an insurer being asked whether a procedure needs pre-authorisation, or a
council being asked what a permit application requires, and the steps are the same. Only the last
one changes. That's the property I like most about it: the first four don't care what kind of
organization this is, or how large, or whether it employs a single engineer.

The consumer version is identical and less interesting — an assistant booking a table reads the
same four things and ends at a web form.

The harder version is when you don't know who to ask at all: "find me anyone who can move a pallet
from Rotterdam to Lyon by Thursday." That needs something that has already read a lot of these
files, which is a search problem. Someone else will solve it if any of this turns out to be worth
solving, and it shouldn't be a precondition.

## This isn't DNS

I keep reaching for the DNS comparison and it's wrong every time.

DNS answers `name → address`. You already know the name. Here the hard part is that the asker
*can't name the thing* — they know what they want done, not who does it. That's a much worse
problem, because names are exact and intentions aren't.

It's closer to what a search engine does than to what DNS does. Which is worth sitting with,
because search engines solved "who can do this" without anyone publishing structured
descriptions at all.

## Why organizations, and not everything

I started wider than this. The first version had actors of every kind — organizations, services,
individual agents, and people. A person has capabilities too: I can write software, review
architecture, take consulting work. It's a tidy generalisation and I liked it.

I narrowed it to organizations on purpose, and I think that was right. Almost everything an
autonomous system would want done on someone's behalf is done by an organization — a bank, a
clinic, an airline, a restaurant, a government office, a cloud provider. Starting general would
have meant designing for cases that don't yet exist, at the cost of the ones that do.

It can widen later if it turns out to matter. Generalising early is how ideas like this get
heavy before they get used.

## Nobody will maintain this by hand

One thing I'm fairly confident about: if organizations ever do publish something like this, they
won't be writing it manually for long.

It would be generated from what they already have — their service catalogue, their API
definitions, their internal systems — and kept current the same way. A description maintained by
hand goes stale in a quarter, and a stale description is worse than none, because something will
act on it.

## If only one part of this survives, I think it's this

Everything above is a fairly large idea that needs a lot of things to go right. There's a much
smaller one inside it that I've come to think is the strongest part, and it stands alone.

Before something acts on your behalf, it needs to know what kind of act it is:

```
does this spend money
can it be undone, and for how long
does a person have to approve it
is it safe to retry if I'm not sure it went through
```

None of that is in an API signature. All of it exists today — encoded in a confirmation screen,
in the wording of a button, in the small print under the total. A person reads it without
noticing they've read it. Strip the screen away and the information is simply gone.

This is worth separating from the rest because it survives almost every objection to the bigger
idea:

- **It costs an organization nothing.** Publishing "this action is irreversible" doesn't make you
  comparable, doesn't flatten your brand, doesn't cost you margin. The commoditisation problem
  doesn't apply.
- **It needs no shared vocabulary.** Four or five fields, obvious in any language.
- **It needs no registry and no adoption curve.** It's useful for one organization and one
  assistant, immediately.
- **The demand already exists.** People are nervous about letting software spend their money, and
  right now there's nothing an organization can say to reduce that.
- **It's where regulation is heading anyway.** "What should an autonomous system have known before
  it committed you to this" is going to be asked by someone with more authority than me.

I don't know whether this is the wedge that makes the rest possible, or whether it's the only
part that was ever real. It might be worth doing regardless of whether the rest is right.

## How two agents actually talk

Everything above is a description being *read* — one side publishes, the other reads, then acts.
That's a lookup, and real interaction between two organizations is more than a lookup. It goes
back and forth:

> *No capacity Thursday. Friday?*
> *Friday's fine if it's still the same price.*
> *Not at that weight. It is if you split it across two consignments.*
> *Can you insure it?*

I don't want to pretend I have no view on this, because I do, and it's the actual proposal.

**The hypothesis is that capability is the shared language.** When two agents that have never met
start talking, the thing they have in common isn't a protocol I invented — it's that one of them
has published what it can do and on what terms, and the other can read it. The conversation is
*about* capabilities: which one applies, whether its terms are acceptable, what it needs, what it
commits you to. The description isn't just how you get found; it's the vocabulary the negotiation
is conducted in.

That's a real claim and it's the one I'd most like argued with. It says you don't need a new
agent-to-agent protocol so much as a shared notion of what's being negotiated over — and that
notion is the capability.

What I genuinely don't have is the layer on top: the turn-taking, the counter-offers, the fact
that an offer expires, and the moment something becomes binding. Two honest possibilities:

- That layer is small once capability is the substrate — it's mostly two capable agents talking
  in natural language about a well-described thing, which is exactly what current models are good
  at. In that case there isn't much to specify; the description does the heavy lifting and the
  rest is conversation.
- Or that layer is the hard part — commitment, liability, disputes — and it needs machinery
  (something like contracts) that capability descriptions don't provide.

I lean towards the first for ordinary cases and suspect the second is real wherever money or
obligation is involved. Either way, I don't think the answer is a heavyweight protocol designed
up front. I think it's: describe capabilities well, let capable agents talk, and find out where
that breaks.

There's a useful clue in what people building internal agents already do at the boundary. A good
pattern is that when an agent hits something outside its lane, it doesn't improvise around it — it
emits a *typed request* (needs approval, missing credential, ambiguous decision) that today lands
on a human. That typed request is already the opening move of a negotiation; the only thing this
idea changes is that some of those requests could be answered by the other side's agent rather
than a person, for the subset that side chose to expose and can be held to. Which is why the
[terms block and what it's bound to](assumptions/when-being-wrong-is-expensive.md) matters more
than the conversation layer on top of it.

## What kind of thing this even is

If any of this turned into something, I don't think it would be software.

It wouldn't be an API standard — those describe how two parties talk once they've found each
other, and this is about the part before. It wouldn't be a way of connecting a model to tools;
that problem is being worked on by people closer to it than me. And it wouldn't be a framework for
building agents, which is somebody else's business entirely.

What's left is duller: a shared way of *describing* what organizations can do. A vocabulary rather
than a mechanism.

I'm aware that's a convenient thing to claim, because vocabularies sound harmless. They aren't
always — a vocabulary that catches on decides how everyone afterwards is able to think about the
problem. And a vocabulary nobody implements is just a document.

## The part I actually care about

Underneath all of the above:

> Today's Internet is an **information network** — it's organised around *where things are*.
> What I think is coming is closer to a **capability network** — organised around *who can do
> what*.

The question stops being "where is the website for this" and becomes "who can do this, and how do
I ask them."

## What would change my mind

- AI keeps getting better at using human interfaces, fast enough that the gap closes on its own.
  This is still the strongest objection and I don't have an answer to it.
- Someone shows the naming problem isn't just hard but unworkable at open-web scale.
- Indexing turns out to be useless without cooperation — that whatever can be worked out from the
  outside is too thin or too wrong to act on. This is the part the prompt in
  [assumptions/](assumptions/) is meant to test, and it's cheap to find out.
