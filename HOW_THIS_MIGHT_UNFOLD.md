# How This Might Unfold

The short version of the whole idea is a change in who, or what, an organization is talking to.
It's easiest to see across three stages.

## Yesterday — the web was built for people to look at

For thirty years the primary interaction was a human reading a screen. A person visits a site,
reads it, clicks, fills in a form, decides. Everything on top — layout, buttons, menus, the
checkout flow — exists so that a person can work out what to do.

APIs existed alongside this, but they were for integrations that had already been arranged. Two
parties negotiated, someone read the docs, someone got a key, and then the software talked. The
API is the conversation *after* the introduction. The introduction was done by people.

## Today — agents, but pointed inward

Right now, "agentic AI" mostly means companies building agents for themselves.

This is the part I think is under-noticed, because it's happening quietly and everywhere — and
not just in engineering. Finance, HR, procurement, operations, support: departments are building
internal agents to shorten their own processes, fast. MCP servers, tool definitions,
function-calling, orchestration. An agent that reads your own systems, calls your own services,
automates your own back office.

And it works, for a specific reason: **you control both ends.** You chose the tools. You hold the
credentials. You know what everything does because you built it. You already trust it. Inside
that boundary, connecting a capable agent to your systems is a solved problem.

I should be clear about what is and isn't evidence here. That the internal build-out is happening,
at scale, across departments — that's observable; it's what a lot of enterprise engineering is
spending its time on right now. That it *turns outward* is my inference, not a fact. But the
inference is short, because of what these internal agents keep running into.

This is not a small thing that's about to be replaced. It's the groundwork. Once a company has an
agent that genuinely understands what the company can do, something becomes possible that wasn't
before.

## Tomorrow — the agents turn outward

Here's the prediction, and it's only a prediction.

The moment you have capable internal agents everywhere, a lot of the tasks they're given don't
actually stop at the company wall. A concrete one I keep coming back to: someone in finance types
"switch this employee's line to the XX package" to their own internal agent. That's a request to
the *mobile operator*, not to anything inside the company. Today that finance person — or their
agent — ends up in the operator's web panel, or against the operator's API if someone integrated
it. The internal agent has run straight into another organization and has nowhere to go.

It's everywhere once you look: asking a supplier for last month's invoice, checking a shipment
with a carrier, filing something with an authority. The internal agent was built to shorten a
process, and half of these processes cross a company boundary. So the outward pull isn't a new
ambition someone has to decide on — it's the internal agent doing exactly what it was built for
and hitting a wall.

I think that changes, and I think the thing your agent ends up talking to is *their* agent. In the
line-change example: the finance agent talks to the operator's agent, which already knows what it's
allowed to do and on what terms — because the operator decided what to expose, the same way it
decides what's in its API today. Nobody integrated the two in advance.

Drawn out, that example has four hops and only one of them is new:

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
already work today; connecting an agent to your own systems is the solved part. The single arrow
across the middle is the only new thing: two agents that have never met, with nothing integrated
in advance. That one hop is all this repository is about. Everything else on the diagram, each
company already builds for its own reasons.

And a company's agent can do things an API can't. An API is a fixed set of endpoints — it answers
exactly what it was built to answer. An agent can hold context across a conversation, interpret a
request that wasn't anticipated, weigh options, ask a clarifying question, handle the case that
takes six weeks and three rounds of missing documents. It is a more capable counterpart than a
list of endpoints, because it can reason about the request rather than just match it.

So the shape of a business interaction shifts from:

```
your integration code  →  their API
```

to:

```
your agent  →  their agent
```

with no integration project in between, because neither side was built against the other.

## Why the order is inward-then-outward

Companies build for themselves first because that's where the return is obvious and the ground is
safe — both ends under one roof. The outward version is harder: you don't control the other side,
you haven't met it, and trust has to come from somewhere. So it comes second.

But once the internal agent exists, turning it outward is a smaller step than building it was. The
hard part — an agent that understands what the organization can actually do — is already done for
internal reasons. Exposing some of that to the outside is a decision, not a rebuild.

That's why I don't think this needs a grand coordinated effort. It rides on something companies
are already doing for their own reasons.

## Which raises two questions

If agents start dealing with agents they've never met, two things have to work, and neither is
solved by MCP or by APIs — because both of those assume the relationship already exists.

**How does one agent find another, and know it's really them?** This is the detection question,
and I think it has a fairly boring answer that needs nothing new — a pointer in `robots.txt`,
files under `/.well-known/`, HTTPS for "it's really them." Covered in [IDEA.md](IDEA.md) and
[assumptions/](assumptions/).

**How do two agents that have never met actually communicate?** This is the interesting one, and
it's where I have a hypothesis rather than a hole — see below.

## Where I might be wrong

Maybe internal stays internal — companies automate their own back offices and never expose any of
it, because the risk of an outward-facing agent isn't worth it. Maybe APIs are simply enough for
the cross-company case and always will be. Maybe the trust problem is hard enough that the
outward step never happens at scale.

Each of those would mean this prediction is wrong. I think it's worth writing down anyway, because
if it's right, the interesting decisions are being made now — inside all those internal-agent
projects that don't yet think of themselves as building the next layer of anything.
