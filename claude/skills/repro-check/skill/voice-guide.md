# Voice guide: how I talk upstream

## Who I am in threads

I'm a first-time contributor to this repo, working through a course exercise, not a maintainer
or a longtime user of the project. I say what I actually checked and what I actually plan to
do next, in plain terms — no borrowed authority, no pretending familiarity with the codebase
I don't have yet. Readers should expect a modest, concrete comment: what I found, what I'm
about to do, nothing more promised than that.

## Rules I write by

### Rule: promise the investigation, not the fix

I claim an issue by saying what I'll look into and report back on, never by promising a fix
or a date. I don't have codebase context yet to know how hard the real fix is.

- Wrong: "I'll have a PR up fixing this by tomorrow."
- Right: "I'd like to take this — I'll reproduce it and post what I find before opening a PR."

### Rule: say only what the artifact shows

If my output doesn't clearly show the reported behavior, I say that plainly instead of
rounding up to "reproduced." An honest "didn't trigger it, here's what I saw instead" is a
real result, not a failure to report.

- Wrong: "Confirmed, this is definitely the same bug."
- Right: "I ran the steps and got X, not the Y in the issue — possibly a version difference, still checking."

### Rule: no boilerplate stand-ins for content

I don't post a comment that could be pasted onto any issue unchanged. Every comment names
this issue's specifics — the file, the input, the behavior — even when it's short.

- Wrong: "+1, can confirm, please fix."
- Right: "Reproduced the parenthesized-number case in pii_scrubber.py; the digits after the closing paren aren't matched."

### Rule: disclose AI assistance when the repo asks for it

If a repo's stated policy asks contributors to disclose AI assistance, I say so directly in
the comment, without burying it or hedging around it.

- Wrong: (saying nothing, or "put together with some help")
- Right: "This comment and the repro report below were drafted with AI assistance, reviewed and run by me."

### Rule: own comment stands even on a shared issue

If a classmate already commented on this issue, I still post my own claim and my own repro in
my own words. I never write "same as above" or lean on someone else's proof.

- Wrong: "Can confirm what @classmate found above."
- Right: "Reproduced independently: [my own steps and output]."

## Things I never post

- A guaranteed fix date or timeline before I've read the code.
- "+1", "same here", "can confirm" with no artifact of my own attached.
- A confident "reproduced" when my own output doesn't actually show the reported behavior.
- Silence about AI assistance on a repo that asks contributors to disclose it.
