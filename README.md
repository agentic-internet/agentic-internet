# Agentic Internet

An open notebook about one prediction, written down so people can argue with it.

**I think the Internet is shifting from something people use to something their software uses for
them — and that the interesting part is what happens between two organizations' systems that have
never dealt with each other.**

Roughly where I think this is going:

```
yesterday   a person visits a website
today       companies build agents for their own systems (MCP, tools) — pointed inward
tomorrow    those agents turn outward: your agent dealing with their agent
```

The full version of that is in [HOW_THIS_MIGHT_UNFOLD.md](HOW_THIS_MIGHT_UNFOLD.md). The short of
it: I build the inward-facing kind for a living — wiring MCP and internal agents into enterprise
systems is what I do right now — and this is me thinking one step past that. We've got good at the
inside-one-company case, because you control both ends: you chose the tools, hold the credentials,
already trust it. There's nothing for the other case. Two organizations, no prior relationship, no
integration, no contract, no one to email. And I think a company's agent, once it exists, is a
more capable counterpart than an API — it can hold a conversation, interpret a request nobody
anticipated, handle the thing that takes six weeks.

## The pattern, and it's in every industry

This isn't about any one sector. It happens whenever two organizations that never planned to work
together suddenly need to — which is most organizations, most of the time. The same shape, over
and over:

- **A clinic and an insurer it rarely deals with.** Does this procedure need pre-authorisation,
  what evidence, how long for a decision? The clinic finds out by ringing them or logging into a
  portal, and a clinic dealing with thirty insurers has thirty versions of that.
- **A manufacturer whose usual supplier is out of stock.** Another supplier might have it — lead
  time, minimum order, will they ship here, do they meet the standard? An email and a two-day
  wait, per supplier.
- **A company and a tax authority in a country it just started selling into.** What must be
  filed, in what form, by when? Published for people to read, unreachable by anything else.
- **A bank and a business customer it's onboarding.** What documents, what checks, what happens
  next? Different at every bank.

In every case the obstacle is the same, and it isn't that the other side can't be reached. It's
that *finding out what they can do, and on what terms,* takes a person reading things and then
writing code — or paying a middleman who did that once and rents it back.

That "a person" is the part I think is about to change. The whole prediction is that the one doing
the finding-out stops being a developer wiring up an integration and becomes an agent, acting for
you, dealing with the other side's agent — and none of these examples have anywhere for that agent
to even begin. The point was never the integration pain itself; it's what the pain is standing in
the way of.

### The version that's easiest to picture

Shipping, because the numbers are stark. An e-commerce company could use forty carriers, and
today that means forty separate integrations — different interface, auth, rate structure,
tracking format — each built by hand and maintained, or bought from an aggregator.

Now imagine its system needs a quote from a carrier it has never used, at two in the morning,
because the usual one has no capacity. There's no path for that at all. That gap — the unfamiliar
counterpart, the one-off, the thing no aggregator bothered to integrate — is the same gap in
every industry above. Shipping just makes it obvious.

There's a consumer version too — an assistant booking a table or changing a flight — but that's
the easier case, and not where I think this matters most.

## Why APIs don't settle it

The first thing anyone says, including me, is APIs.

An API describes a conversation between two parties who have already found each other and agreed
to talk. It's very good at that. It says almost nothing about how they got there, because
historically that part was done by people — reading docs, signing things, deciding.

That's the pattern in most of what already exists: it solves the interaction, not the
introduction. [WHATS_ALREADY_SOLVED.md](WHATS_ALREADY_SOLVED.md) goes through the main ones,
including the older systems that have quietly done cross-company machine-to-machine work for
decades — and why they don't close this particular gap.

## What I think organizations end up doing

An organization already has more than one public face, and nobody finds this strange:

```
for people           →  the website
for integrations     →  the API
for search engines   →  robots.txt, sitemaps, structured data
```

Each appeared because a new kind of visitor turned up and stayed long enough to be worth
addressing on purpose. Nobody planned the third one — it accumulated.

So I keep wondering whether there's a fourth coming:

```
for autonomous systems  →  ?
```

Websites explain. APIs execute. The thing I'm wondering about sits before both — what an
organization says about itself to something that has never dealt with it before. Roughly: *this
is who we are, these are the things we can actually do, this is how you reach each one, and this
is what we need from you first.*

And I don't think it needs anything new to exist. A line in `robots.txt` pointing at some files
under `/.well-known/` — the same way sitemaps happened. **No registry, nothing to join, nobody in
the middle.** DNS already says where you are; HTTPS already says it's really you.

This is where the shift at the top comes back in. That description is the same whether the thing
reading it is a plain script today or a company's agent tomorrow — only the last step changes,
from "call this API" to "talk to this agent." So it isn't a rival to the agent-to-agent future;
it's the on-ramp to it. It lets an organization with only a phone number take part now, and it's
what two agents that have never met end up negotiating in terms of: not a new protocol, just a
shared idea of what's on offer and on what terms. That last part is a hypothesis, and the one I'd
most like argued with — it's worked through in [IDEA.md](IDEA.md).

The whole thing in one picture. Someone in finance tells their own internal agent to switch an
employee's phone line to a different package — which is really a request to the operator, another
company:

```
  Company A                          Company B  (the operator)
  ─────────                          ───────────────────────

  finance's internal agent
    "switch this line to XX"
          │
          ▼
  A's outward agent   ──────────►    B's public agent
  speaks for A,                      what B chose to expose,
  proves who it is                   described in /.well-known
                                           │
                                           ▼
                                     B's internal agent
                                       → line switched
```

The two internal agents — top-left and bottom-right — are each company's own business, and both
already work today. The single arrow across the middle, between two agents that have never met
with nothing integrated in advance, is the only new thing on the picture — and all this is really
about. ([HOW_THIS_MIGHT_UNFOLD.md](HOW_THIS_MIGHT_UNFOLD.md) walks through it.)

## What I'm not saying

- Websites don't go away. People still read them.
- APIs don't go away. An agent that can hold a conversation is a more capable counterpart than a
  fixed set of endpoints, but plenty of interactions want exactly a fixed set of endpoints, and
  an agent will often be talking to an API underneath anyway.
- I'm not proposing a standard. I don't think anyone should be defining standards for this yet,
  including me.
- I'm not sure I'm right. Two things could sink it: AI getting good enough at using interfaces
  built for humans that none of this is needed, and the fact that aggregators already absorb this
  problem commercially and have no reason to want it solved in the open.

## What's here

- [HOW_THIS_MIGHT_UNFOLD.md](HOW_THIS_MIGHT_UNFOLD.md) — the prediction itself, in three stages:
  yesterday's web, today's inward-facing agents, and the outward turn I think is coming.
- [IDEA.md](IDEA.md) — the longer version: capabilities, why naming matters less than it looks,
  how agents that have never met might communicate, and how an organization with only a phone
  number still takes part.
- [WHATS_ALREADY_SOLVED.md](WHATS_ALREADY_SOLVED.md) — the honest part: most of this is already
  solved. What existing systems cover, why they don't close this particular gap, and the times
  something like it was tried before and failed.
- [assumptions/](assumptions/) — the shapes I've guessed at, kept separate because they're
  guesses. Includes a prompt you can paste into any AI to generate one for your own organization.

## Try it in a minute

There's one thing here you can actually run:
[assumptions/generate-your-own.md](assumptions/generate-your-own.md) is a prompt you paste into
any AI, naming an organization you know well. It produces the description this whole idea depends
on, built only from what that organization already publishes.

Its last instruction asks the model to say where the structure didn't fit. **That part is the
point.** If descriptions built from the outside turn out to be too thin or too wrong to act on,
the idea has a serious problem, and this is the cheapest way to find that out.

I'd rather have one person's output from a hospital or a freight company than a hundred people
agreeing with the premise.

## Talk to me

This is a prediction, not a project plan. What I'd find most useful:

- Tell me the prediction is wrong, and why.
- If you're building agents inside a company too, tell me whether turning them outward looks
  plausible or absurd from where you're standing.
- Tell me this was already tried and I missed it.

Open an issue and say what you think. There's no process, no roadmap, and nothing to sign up for.

## License

MIT — see [LICENSE](LICENSE).
