# Acceptance criteria — The Unofficial Guide

Five criteria that say what "working" means for this system, written in unit 1
**before** any results existed.

An acceptance criterion names a target: a number, a count, a rate, or something
a person could plainly observe. _"Retrieval works"_ is an opinion. _"For at
least 4 of my 5 test questions, the top results include a chunk containing the
answer"_ is a criterion.

Under each one, write a sentence or two on **why that target** and not a
stricter or looser one. A reason that says something about your corpus or your
pipeline earns credit; _"80% seemed reasonable"_ does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## 1. Retrieved chunks contain the answer

For at least 4 of my 5 test questions, the retrieved chunks include one that
contains the answer.

**Why this target:**

<!-- e.g. "One of my questions is about a topic only two documents mention, so
     I expect that one to be hard." -->

We cannot expect every question to have an answer, however, most questions will be relevant and answerable by the docs, therefore, 4/5.

## 2. Every answer names a source

Every answer the system produces names at least one source document.

**Why this target:**

<!-- Why all five and not four? What about your setup makes that achievable —
     or what would have to go wrong for it not to be? -->

This target is needed because every answer must be backed by evidence in the documents, so there is no halucination.

## 3. The relevance gate stops out-of-corpus questions

When I ask a question my documents clearly don't cover, the relevance gate
stops it and the system returns "I don't have enough information about that" —
in at least 4 of 5 tries.

<!-- The five questions are the ones in `OUT_OF_SCOPE` at the bottom of
     `questions.py`, and `run_eval.py` puts them through the gate and writes
     what happened into your run log. Swap them for your own if you'd rather —
     just keep five of them, or the "4 of 5" above has nothing to be 4 of. -->

**Why this target:**

<!-- What did your distances look like when you set the cutoff in Milestone 4?
     Was there a clean gap, or did the two groups overlap? -->

This is to also make sure the model doesn't halucinate to just come up with an unsupported answer.c

## 4. Full sentence chunks

<!-- YOU WRITE THIS ONE.

     How would you know if your chunks were the right size? Name something
     countable or observable.

     Examples of the right shape — don't copy these, they should come from
     what you actually saw in Milestone 3:
       - "At least 4 of 5 sampled chunks read as a complete thought, with no
          sentence cut in half at either end."
       - "No chunk is shorter than 200 characters, since anything below that
          in my corpus turned out to be a heading with no content under it." -->

All 5 sampled chunks start and end on a full sentence (nothing cut mid-word or mid-sentence), and each one names the town or topic it's about.

**Why this target:**

Most answers in the city guides are a single sentence ("Kestrelford's bakery
sells out by 11am"), so a cut through that sentence means no chunk contains the
answer.

---

## 5. Answer includes source

<!-- YOU WRITE THIS ONE TOO.

     Pick something you actually care about getting right. It could be about
     speed, about refusals, about a particular kind of question your corpus
     handles badly, about source attribution being correct rather than merely
     present — anything, as long as it names a number or an observable
     outcome. -->

Every answer ends with one line listing every document it used, like
`Sources: guide_kestrelford.md, guide_eating.md`.

**Why this target:**

A fixed `Sources:` line makes that quick to check for more information and to verify the information.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 2 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath it, like this:

         ## 1. Retrieved chunks contain the answer

         For at least 4 of my 5 test questions, the retrieved chunks include
         one that contains the answer.

         **Why this target:** ...

         > **Revised in unit 2:** For at least 4 of 5 questions, the top three
         > results contain the answer.
         >
         > **Why revised:** I couldn't judge "the chunks include one that
         > contains the answer" the same way twice — I scored two questions
         > differently on Monday than on Wednesday. The new version is
         > something I can actually check.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "I said 4 of 5 but got 2 of 5, so 2 of 5 is more realistic."

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.

     The whole reason the originals stay visible is so someone can see what you
     said before you knew the answer.
     ───────────────────────────────────────────────────────────────────────── -->
