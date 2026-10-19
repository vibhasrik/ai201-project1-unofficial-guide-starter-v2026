# The Unofficial Guide

<!-- Replace this line with your name and which corpus you picked. -->

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Unit 1

## What This Does

<!-- Three or four sentences. Which corpus you picked, and the kinds of
     questions your system answers. Write it for someone who has never seen
     this repo.

     Milestone 5. -->

     I picked the city_guides corpus. This project is a RAG that can answer grounded questions about the city_guides documents. It can answer direct questions about locations included in the corpus. It will be clear when it doesn't know something. For example, the corpus has a document about Brightwater. The RAG can answer something like "When should I visit Brightwater?"

## Chunking Strategy

**Chunk size:** one `##` section per chunk, with the guide's title on the first line (171 to 759 characters, 319 on average, 94 chunks)
**Overlap:** none

Every city guide is split into short `##` sections (Getting there, Eat and
drink, When to go…), and each section covers one topic in a few sentences. Most
answers are a single sentence inside one section, so a section is the natural
unit: it never cuts a sentence and never mixes two topics. The starter's
800-character windows cut mid-word ("The station is a 15-") and merged
three or four sections into one chunk. Sections like "Eat and drink" read the
same across ten towns, so each chunk starts with the guide's title to say which
town it means. No overlap is needed because sections don't run into each other.

<!-- What about YOUR documents made you pick these numbers? Short posts and
     long sectioned guides don't want the same chunking, and "800 seemed
     reasonable" earns nothing. Point at something you noticed when you read
     the documents in Milestone 1.

     If you changed your mind partway through, say so and say why. That's worth
     more than pretending you got it right first time.

     Milestone 3. -->

## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. -->

**Chunk 1** — source: `guide_accessibility.md#0` — produced by: `chunker.py::split_documents`

```
Getting around the region with limited mobility
An honest assessment rather than a promotional one. Some of these places are
difficult and it is better to know in advance.
```

**Chunk 2** — source: `guide_corry_vale.md#5` — produced by: `chunker.py::split_documents`

```
Corry Vale
## Where to stay

Perhaps thirty beds in the entire valley, spread across two pubs and a handful of farmhouse rooms. In summer these are booked months ahead. Camping is permitted on two marked fields and nowhere else.
```

**Chunk 3** — source: `guide_givens_mill.md#2` — produced by: `chunker.py::split_documents`

```
Givens Mill
## Getting around

Everything is on one street along the river. The mill is at one end and the church at the other, eight minutes apart. The riverside path continues in both directions for as far as you want to walk.
```

**Chunk 4** — source: `guide_kestrelford.md#4` — produced by: `chunker.py::split_documents`

```
Kestrelford
## What to see

The market square on a Saturday morning is the main event and has run continuously since the 1400s. The parish church has a 13th-century tower you can climb for £2. The old trackbed walk runs six miles to the next village along an easy gradient and is the best half-day here.
```

**Chunk 5** — source: `guide_pellew_sands.md#6` — produced by: `chunker.py::split_documents`

```
Pellew Sands
## When to go

June and September for the beach without the crowds. July and August are busy and the town is at its most itself, for better and worse. Winter is bleak, largely closed, and has a following among people who like that sort of thing.
```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

**Question:**"What are the most accessible towns for someone with limited mobility?"

**Answer:**

```
Thornby Wells is the easiest town in the region as it is flat, compact, has a central station, and the pump room and gardens are level throughout. Marchwood features a modern tram network with level boarding, and a step-free city museum and covered market. Brightwater is level along the river and through the centre, with a step-free mill museum.

Sources: guide_accessibility.md

Sources retrieved: guide_accessibility.md, guide_corry_vale.md, guide_walking.md
```

**My relevance cutoff:** 0.7

<!-- The number you set in config.py, and how you got there.

     You ran five questions your corpus covers and the five in OUT_OF_SCOPE
     that it clearly doesn't, and wrote down the best distance for each. What
     did those two groups look like? Where was the gap? Put the actual numbers
     here — the table below wants all ten rows.

     Milestone 4. -->

My five in-corpus questions scored between 0.248 and 0.535,
and the five out-of-corpus questions scored between 0.808 and 0.982, so there
is a clean gap from 0.535 to 0.808 with nothing in it. I started at the default
0.6, but my first version of the mobility question ("If I can't move much, what
are some good places to visit?") scored 0.614 and got refused even though the
answer is in `guide_accessibility.md`. People don't always use the guides'
wording, so I put the cutoff at 0.7, high enough for questions like that to get
through and still about 0.1 below the closest out-of-corpus question.

| Question                                                                                       | In corpus? | Best distance |
| ---------------------------------------------------------------------------------------------- | ---------- | ------------- |
| What are the most accessible towns for someone with limited mobility?                          | Yes        | 0.535         |
| Where do long-distance coaches stop in Brightwater?                                            | Yes        | 0.333         |
| By what time does the Kestrelford bakery usually sell out?                                     | Yes        | 0.351         |
| How often does the road to Elder Ness flood?                                                   | Yes        | 0.248         |
| In Halden Bay, where can I get the same food as on the harbour front for about half the price? | Yes        | 0.279         |
| What is the capital of Mongolia?                                                               | No         | 0.808         |
| How do I change the oil in a diesel engine?                                                    | No         | 0.882         |
| Who won the 1994 World Cup?                                                                    | No         | 0.982         |
| What is the recommended dosage of ibuprofen for a headache?                                    | No         | 0.818         |
| How do I write a for loop in Rust?                                                             | No         | 0.814         |

## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->

**1.** My first question in questions.py was "If I can't move much, what are some good places to visit?" This question though always returned "I don't have enough information about that" regardless of what parameters I tried to change, so I asked AI why. My wording seemed close enough to limited mobility, so I felt like it should have given the same result as a sentence with "limited mobility." After asking Claude, though, I saw that the distance between "can't move" and "can move" was very little compared to the distance between "can't move" and "limited mobility." The wording was causing the issue. I then changed the wording of the question to something that would match better to the content I was looking for.

**2.** I used Claude to try out a couple releveance cut offs and determine the best one. It just helped me minimize the manual coding and testing. I was able to make the decision based on the change in outputs.

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->

| Criterion                              | Target | Run 1 | Run 2 | Run 3 | Verdict |
| -------------------------------------- | ------ | ----- | ----- | ----- | ------- |
| 1. Retrieved chunk contains the answer | 4 of 5 |       |       |       |         |
| 2. Every answer names a source         | 5 of 5 |       |       |       |         |
| 3. Gate stops out-of-corpus questions  | 4 of 5 |       |       |       |         |
| 4.                                     |        |       |       |       |         |
| 5.                                     |        |       |       |       |         |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| #   | Criterion | Verdict | How I decided |
| --- | --------- | ------- | ------------- |
| 1   |           |         |               |
| 2   |           |         |               |
| 3   |           |         |               |
| 4   |           |         |               |
| 5   |           |         |               |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

## The Improvement

**What I changed:**

**Why I picked it:**

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion                              | Target | Run 1 | Run 2 | Run 3 | Verdict |
| -------------------------------------- | ------ | ----- | ----- | ----- | ------- |
| 1. Retrieved chunk contains the answer | 4 of 5 |       |       |       |         |
| 2. Every answer names a source         | 5 of 5 |       |       |       |         |
| 3. Gate stops out-of-corpus questions  | 4 of 5 |       |       |       |         |
| 4.                                     |        |       |       |       |         |
| 5.                                     |        |       |       |       |         |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
