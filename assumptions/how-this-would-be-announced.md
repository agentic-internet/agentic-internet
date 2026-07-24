# How an Organization Would Announce This

*A guess. See [README.md](README.md) for what that means.*

The part that requires nothing new to exist. No registry, no signup, no coordination — one line
in a file that's already on almost every domain, and some files at a conventional address.

## One line in robots.txt

`robots.txt` already has a directive that does exactly this job. `Sitemap:` isn't a rule about
crawling — it's a pointer to somewhere else, sitting outside the `User-agent` groups. It was added
years after the original convention and it spread because it cost nothing.

So:

```
User-agent: *
Disallow: /admin/
Disallow: /checkout/

Sitemap: https://example.com/sitemap.xml
Agent:   https://example.com/.well-known/agent
```

That's the whole announcement. Anything that doesn't understand `Agent:` ignores it, which is how
`robots.txt` has always handled unknown directives — no version negotiation, no breakage.

Why this file rather than a new one: it's already there, it's already the place organizations
speak to non-human visitors, and adding a line to it doesn't require anybody's permission or a new
convention to catch on.

## Saying no is part of it

The same file has to be able to refuse, and this should exist from the first version rather than
being added once it becomes a problem:

```
User-agent: *
Agent: none
```

Or narrower — permitted for some things and not others, which is likely to be the common case:

```
Agent: https://example.com/.well-known/agent
Agent-policy: no-automated-purchase
```

I don't know what the vocabulary for that should be. I'm more confident that refusal has to be
expressible than I am about how. It's the one thing `robots.txt` unambiguously got right, and the
reason it was tolerated for thirty years.

## What sits behind the pointer

```
/.well-known/agent                          the organization: who, what, how to reach it
/.well-known/agent/capabilities/            one file per thing it can do
```

`/.well-known/` is already the conventional place for machine-readable facts about a site —
`security.txt`, certificate issuance, OpenID configuration all live there. Using it means no new
convention has to be invented and it'll look familiar to anyone who has set up a certificate.

Details of both files: [what an organization might publish](what-an-organization-might-publish.md)
and [what a capability might look like](what-a-capability-might-look-like.md).

## Two different things get announced here

Worth separating, because I kept conflating them.

**A description.** Static files saying what the organization can do and on what terms. No running
software, nothing to operate. A restaurant whose bookings happen by telephone can publish this and
never change anything about how it works.

**An endpoint.** A live thing that can be talked to. Some organizations will have one; most won't,
at least at first.

So the organization file allows both, and neither is required:

```yaml
interaction:
  - type: rest
    base: https://api.example.com

  - type: agent
    endpoint: https://example.com/agent
    protocol: unspecified

  - type: website
    base: https://example.com/book

  - type: telephone
    number: "+44 20 7946 0000"
    hours: "09:00-18:00 Europe/London"
```

`type: agent` is deliberately vague about what's on the other end. Whatever conventions emerge for
software talking to software, they'll emerge somewhere else and be better than anything I'd invent
here. What matters for this file is only *that there is one and here's where*.

An organization with only a telephone number is a legitimate entry, not a degraded one. If the
design doesn't work for them, it works for a few thousand technology companies and nobody else.

## Public, or behind authentication

Not everything has to be readable by anyone. A reasonable split, though I'm guessing:

- **Public** — who you are, roughly what you can do, whether you accept automated requests at all,
  and the terms that matter before acting. Enough for something to know whether to bother.
- **Behind authentication** — exact parameters, current pricing, availability, anything that
  constitutes a commercial offer.

That matters more than it looks. An organization worried about being reduced to a price in a
comparison can publish what it *does* without publishing what it *charges*, and the second
conversation happens after identification. It also means "publish this" and "give away your
commercial position" aren't the same decision.

## What I'm least sure about

- Whether `Agent:` is the right name, or whether reusing `robots.txt` at all is right. It's a
  thirty-year-old file with an informal grammar and no error handling worth the name.
- Whether the refusal vocabulary is two values or a hundred. Every real organization has
  conditional answers — yes to this, no to that, yes but only for identified callers.
- Whether `/.well-known/` should hold this at all, given that it's usually for things about the
  *site* rather than about the *organization*. A company with twelve domains has a problem here
  and I don't have an answer to it.
- Whether any of this survives contact with a large organization's actual publishing process,
  where changing one line of `robots.txt` can take a quarter and three approvals.
