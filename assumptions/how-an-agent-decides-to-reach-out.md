# How An Agent Decides To Reach Out

Another shape I've guessed at. Everything else in here describes what gets read once an agent has
gone looking. This is the step before that: how it works out it needs to go looking at all, and —
the part I care about more — what stops it going looking *everywhere*.

I ended up here from the other direction. I built a small version of this, and when I read back what
I'd done I found I'd cheated. I'd told the agent, in the prompt, "this isn't yours to do." That's not
the agent deciding anything. That's me writing the answer down and pretending it reasoned its way to
it. So this file is the attempt to take that sentence out and put something honest in its place.

## Recognising the boundary

The question "should I handle this myself or is it someone else's to do" has a cheaper answer than I
first thought, and it's the same trick used everywhere else in this idea, only pointed inward.

An agent already has a set of things it can do in-house — its own tools, its own systems, whatever
it's been connected to. So the test for "is this external" is just: does any of that actually fit
what's being asked?

```
request → match against my own capabilities → something fits?  → do it here
                                             → nothing fits?    → this is someone else's to do
```

"Change this line's tariff" finds nothing in the company's own tools, because changing a line is the
operator's to do, not the company's. The recognition falls out of the agent *not finding itself
capable* — not out of a rule someone wrote. I like that better, because a hardcoded boundary is a
list somebody has to keep current, and this one maintains itself: add an internal tool and the
boundary moves on its own.

It isn't free of failure. The match can misfire in both directions — reach outward for something it
could have done at home, or fail to reach out when it should have. There's no ground truth for
"should this have stayed inside," so this is a place that will need watching rather than a place
that's solved.

## The permission to reach out

This is the part that actually matters, and the part I'd most like taken apart.

An agent that can call any address on the Internet the moment it decides to is not a thing a company
deploys. Reaching outward can't be a raw ability the agent simply has — it has to be a *permitted*
one, granted narrowly, each time.

The thing that worried me is that this sounds like it needs a central authority, which is the one
thing this whole idea is trying not to need. It doesn't. The thing that grants the permission is not
new and it is not shared. **Every company already keeps the list**: who it does business with, and
what it lets happen without a person signing off. Approved suppliers. Spending limits. The vendors
procurement has already cleared. What I'm describing is that list made readable by the agent, and
almost nothing more.

So the agent, having decided it needs help, asks its *own* side a narrow question — not the open
Internet:

```
for this need, am I allowed to act outside, and with whom?

  → yes, and it's this organization   → hand back its domain (+ what I'm cleared to do, and any limit)
  → no                                → not an error: a typed request to a human
```

On "yes," it has a domain, and everything else in this folder takes over — the ordinary read of
`/robots.txt`, the agent file, the capability, the terms. The permission step decided *whether* and
*who*; the published description still decides *what* and *on what terms*. Those stay separate, and
keeping them separate is what stops this from becoming the discovery-by-registry that
[didn't work before](../WHY_THIS_ISNT_UDDI.md). This never answers "who can do X." It only answers
"for this partner I already deal with, am I cleared."

On "no," the important thing is that "no" isn't a failure state. It's the
[typed request to a human](when-being-wrong-is-expensive.md) arriving one step earlier: *I'd have to
reach outside for this, and I'm not cleared to.* The same boundary, in a slightly earlier place.

## Keeping the agent inside the lane

For any of the above to be worth anything, the permission has to actually bind — otherwise it's
advice the agent is free to ignore. The shape I'd draw:

- The agent has **no general ability to call out** — no open "fetch any URL" in its hands.
- Its only outward move is to ask for permission, and what comes back is a **short-lived, narrow
  grant**: this organization, these capabilities, this limit, for now. The agent can't mint one
  itself.
- The actual outbound call carries that grant. No grant, no call.

The tidy part is where the enforcement lives. It doesn't need to be a separate service everyone has
to stand up and route through — a central checkpoint like that is its own bottleneck and its own
thing to run. It can live in the code the agent is already using to make the call: a client that
simply refuses to go out without a valid grant. An organization that wants harder guarantees can put
a real checkpoint on its network too, but that's a choice, not a precondition. The default should be
that reaching out safely is the path of least resistance, or nobody takes it.

## What this is not

- **It isn't the other side's trust problem.** This governs the *caller* — how an agent decides to
  leave, and stays in its lane while doing it. Whether the *receiving* organization should believe
  the agent that shows up is the harder, unsolved half, and it's untouched here.
- **It isn't shared.** One of these per company, private, describing that company's own
  relationships. The moment it's a common directory of who-may-talk-to-whom, it's the central
  authority the rest of this avoids.
- **It isn't the public "is this stranger really who they say" question.** That one might be worth
  answering some day, and if it is, it's a trust anchor closer to a certificate authority than to
  anything here — heavy, centralising, and deliberately kept out of this. It isn't in scope, and I
  think putting it in scope too early would sink the rest.

## Where I'm least sure

- Whether "nothing internal matched" is a reliable enough signal to hang the whole outward decision
  on, or whether it needs a firmer notion of intent than that.
- Whether the permission step, however lightweight, quietly becomes the bottleneck it was trying not
  to be — every outward action waiting on one small service.
- Where the grant's authority actually bottoms out. It says the agent is cleared *by its own company*
  to act. It does not, by itself, prove anything to the other side. That gap is real and it's the
  same gap the trust section keeps running into.
