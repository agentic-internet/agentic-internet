# Generate One for Your Own Organization

Copy the prompt below into any AI assistant you use, replace the organization name, and see what
comes out. It takes about a minute and needs nothing installed.

Two reasons this is worth doing.

If the idea is right, this is roughly how these descriptions would get made in the first place —
generated from what an organization already publishes, not written by hand. So this is the idea
being tested rather than described.

And if the idea is wrong, this is the fastest way to find out. The output will be thin, or
confidently incorrect, or the shape simply won't fit what your organization does. All three are
useful and all three are things I want to hear about.

The last instruction in the prompt asks the model to say where the structure didn't fit. **That
part is the point.** Don't skip it.

---

## The prompt

```
I want you to describe an organization the way a piece of software acting on someone's
behalf would need it described — not the way a marketing page or an API reference would.

Organization: [NAME AND WEBSITE]

Work only from what this organization publishes publicly: its website, product and
pricing pages, help centre, and API documentation if it has any. Do not invent
endpoints, prices, or capabilities. Where you cannot tell, write "unknown" — that is a
useful answer, not a failure.

Produce two things.

1. An organization profile:

   organization:   name, domain, one-line description of what they do
   status:         mark it as indexed and unverified, since they did not
                   provide this themselves
   capabilities:   a list of ids only (see naming below)
   interaction:    how each capability can actually be reached today — a REST API,
                   a web form, a phone number, an email address. Whatever exists.
                   Most organizations have no API and that is fine.
   identity:       the domains you would use to confirm this is really them
   policies:       whether they appear to permit automated interaction at all

2. For each capability, a short description:

   id:             a structured, lowercase name (see below)
   title:          one line
   semantics:      the domain it belongs to, the outcome it produces, and the
                   words a person would actually use to ask for it
   input:          what someone must supply
   output:         what comes back
   interaction:    how it is reached
   authentication: what is required before it will work
   terms:          does it cost money, can it be undone, is there a time limit on
                   undoing it, does a human have to approve it, is it safe to
                   retry

Naming: use outcomes, not endpoints. hotel.room.reserve, not POST /bookings.
"What can this organization do for me" rather than "what functions does it expose".
Go from general to specific, lowercase, dot-separated.

Two things to keep in mind. Describe what the organization does, not what its software
does — a restaurant that takes bookings by phone has a reservation capability even
though it has no API. And where the organization would clearly need to confirm
something with a person first, say so.

Finally, and separately from the output:

Tell me where this structure did not fit. Which fields were awkward, meaningless, or
impossible to fill for this particular organization? What did this organization
obviously do that there was nowhere to put? Be blunt. This structure is a guess and I
am trying to find out where it breaks.
```

---

## What to do with the result

Open an issue with it, or just the last part — where the structure broke.

I'm most interested in organizations that aren't software companies. A cloud provider fits this
shape suspiciously well, which is a warning sign: it means the shape may have been drawn around
the easy case. A hospital, a shipping company, a university admissions office, a municipal
authority — those are where I expect it to fall apart, and where I'd learn the most.

A few things I already half-expect to be wrong, so don't feel you're telling me something obvious:

- The shape probably assumes an organization is one thing with one set of rules. Many aren't.
- It probably handles "book a room" much better than "apply for something and wait six weeks for
  a decision."
- `terms` is likely both too short for real businesses and too detailed for anyone to publish.

If you run it and the output is just plainly useless, that's the single most valuable thing you
could report. It would suggest that whatever can be worked out from the outside is too thin to
build on, which takes out the part of this idea I'm currently most confident about.
