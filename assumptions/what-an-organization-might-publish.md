# What an Organization Might Publish

*A guess. See [README.md](README.md) for what that means.*

The organization-level description. Read once, cached, changes rarely. It says who this is, what
they can do, and how to get at it — but not the detail of any one thing, which lives
[one level down](what-a-capability-might-look-like.md).

## The shape

```yaml
version: 0.1

organization:
  name: Example Hotel Group
  domain: examplehotels.com
  description: Hotel operator, 40 properties across Europe.

capabilities:
  - id: hotel.room.reserve
    detail: /.well-known/capabilities/hotel.room.reserve.yaml
  - id: hotel.room.cancel
    detail: /.well-known/capabilities/hotel.room.cancel.yaml
  - id: payment.refund
    detail: /.well-known/capabilities/payment.refund.yaml

interaction:
  - type: rest
    base: https://api.examplehotels.com
  - type: website
    base: https://examplehotels.com/book
  - type: telephone
    number: "+44 20 7946 0000"

identity:
  verified_domains:
    - examplehotels.com
    - examplehotels.co.uk

policies:
  contact: agents@examplehotels.com
  accepts_automated_requests: true
```

This file sits at `/.well-known/agent`, pointed at from `robots.txt` — see
[how-this-would-be-announced.md](how-this-would-be-announced.md).

There's no status field and no verification flag, because there's nothing to verify against.
The file is served over HTTPS from the organization's own domain, which is the same evidence
you'd accept from their website. If a third party later copies this into an index of their own,
whether *that* copy is trustworthy is their problem to solve, not something this file should
claim.

## Why each part is there

**`capabilities`** is a list of names and pointers, not definitions. Keeping the detail in
separate files means the organization file stays small and cacheable while individual capabilities
change underneath it — the same reason a sitemap lists URLs rather than containing pages.

**`interaction`** can be anything that already exists, including a phone number. This is the piece
that decides whether the idea is adoptable — if it required organizations to build something new
before they could appear at all, almost none of them ever would.

**`policies.accepts_automated_requests`** is there because the ability to say *no* has to be in
the first version. `robots.txt` got that right and it's the main thing worth copying from it.

## What I'm least sure about

- Whether `identity` here is doing anything that domain control doesn't already do. Possibly it's
  just ceremony.
- Whether an organization is really one entity for this purpose. A hotel group with 40 properties
  and different rules per country may not be describable as one thing, and the whole shape may be
  wrong at the wrong level.
- Whether the split between this file and the capability files is worth the extra fetch. It felt
  right because lifetimes differ, but I haven't tested that against anything real.
- What happens when the description is wrong. Not maliciously — just stale. Nothing here handles
  that and something would have to.
