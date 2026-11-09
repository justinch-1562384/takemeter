# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

Community: r/Seattle (https://www.reddit.com/r/Seattle/)

We are going to classify these posts based on three intentions:

Answer, Opinion, or Joke


Candidate Distinctions:

Half of the comments provide something concerte, like a place, route, busline, or a fact about the city. These are usually answers. 

Some comments take a specific slant or opinion. THese are usually opnions.  

A noticeable share are humorous. Depending on whether these provide an opinion or not, these are usually jokes. 


Some of the posts are intended for conversation between subreddit users. 
---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `Answer`

**Definition:** A comment that usually provides an answer with information that pertains to the Seattle Community or local info.

**Example 1:**
> https://www.reddit.com/r/Seattle/comments/1xq8snq/comment/19sagfyo58iqr/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button
**Example 2:**
> https://www.reddit.com/r/Seattle/comments/1xrctd5/comment/19scqyrouj29w/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button

### `Opinion`

**Definition:** A comment that agrees, disagrees, or argues regarding a certain opinion.

**Example 1:**
> https://www.reddit.com/r/Seattle/comments/1xrjh8o/comment/19sd0tx62jb1f/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button
**Example 2:**
> https://www.reddit.com/r/Seattle/comments/1xq8snq/comment/19sb6vwp2ybqa/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button

### `Joke`

**Definition:** A post within the forum that shares a humorous reaction to a post. 

**Example 1:**
> https://www.reddit.com/r/Seattle/comments/1xq8snq/comment/19sa6swkfldcz/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button

**Example 2:**
> https://www.reddit.com/r/Seattle/comments/1xrctd5/comment/19scxcelwn8si/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button

### The hardest boundary

**Which two labels:** Opinion vs Joke.

r/Seattle comments are full of sarcasm, and a sarcastic comment is usually
written as a joke while still taking a side.

**The decision rule I used every time:**

Take the humor out and see what's left. If what's left is a stance someone
could agree or disagree with, it's **Opinion**, even when it's said
sarcastically. If nothing arguable is left (a pun, a meme reference, a riff on
the post), it's **Joke**.

- "Ah yes, another 'luxury' apartment with no parking, exactly what Capitol
  Hill needed." → **Opinion** (without the sarcasm: this development is bad
  for the neighborhood).
- "The Seattle Freeze is just the city conserving heat." → **Joke** (no
  position to argue with).

**Tiebreak for Answer vs Opinion:** if the comment replies to a direct question
with something the asker can act on (a place, a route, a fact), it's
**Answer**, even if it adds an evaluation ("Go to Paseo, it's the best
sandwich in the city").

---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**
https://www.reddit.com/r/Seattle/

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Cold first, Pre-labelled with AI and corrected later. 

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `opinion` | 94 | 47.0% |
| `answer` | 65 | 32.5% |
| `joke` | 41 | 20.5% |
| **Total** | 200 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "I certainly am. I make several million dollars every year. I
> can afford to live anywhere I want. But I am going to move to some shithole
> state where my family will be miserable just to stick it to the libs in WA -
> even if I have to pay higher taxes there! Ha ha ha ha - I showed them!
> /sarcasm" ([link](https://www.reddit.com/r/Seattle/comments/1xo3myh/millionaires_tax_opponents_definitely_still/19s5grk9x4ydg/),
> thread: "Millionaires Tax Opponents Definitely Still Moving to Tennessee Any
> Minute Now")
>
> *Could have been:* `joke` or `opinion`. It's written as a comic bit and
> signed /sarcasm, but it mocks people who threaten to leave over the
> millionaires tax, which is a stance.
>
> *I chose, because:*
> Opinion, as sarcasm falls under opinion in this case, and a joke pertains to something more concrete, pertaining to a mem.,
**2.**
> *The post:* "42% voted to repeal the tax. There's no way that any significant
> amount of that 42% make > 1M a year." ([link](https://www.reddit.com/r/Seattle/comments/1xotn0q/wa_millionaires_tax_is_upheld_as_initiative_645/19s71xdjvwsg4/),
> thread: "WA 'millionaires tax' is upheld as Initiative 645 fails")
>
> *Could have been:* `answer` or `opinion`. It opens with a checkable fact
> (the 42% repeal vote), then uses it to argue that most of those voters
> weren't affected by the tax.
>
> *I chose, because:*
> Answer, as this answered a specific question prior to adding to their own opinion. In this case, the answer made up the bulk of the comment. 
**3.**
> *The post:* "She was beautiful ❤️" ([link](https://www.reddit.com/r/Seattle/comments/1xr4kgp/kai_loved_going_on_urban_hikes_she_was_over_14/19sc69vqz67b5/),
> thread: a memorial post for a 14-year-old dog)
>
> *Could have been:* `opinion` or `answer`, though it fits neither well. It
> offers sympathy, not an arguable stance or information, and it isn't a
> joke. It's one of about 18 sympathy or praise comments with no clear label.
>
> *I chose, because:*
> Opinion, as there was not a concrete fact involved. 
---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:**

**Settings:** <!-- epochs, learning rate, batch size, seed -->

**Anything I changed from the defaults, and why:**

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Moment 2**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Pre-labelling disclosure:**

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
