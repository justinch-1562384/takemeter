# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1. 

Test-set accuracy is at least 0.70 on all three seeds.

**Why this target:**
When choosing this target, we want to keep the bounds tight but not exacting, as the type of post can vary based on the content. As such, 0.7 is a good baseline, with testing revealing whether the target is realistic or not. 

---

## 2.

Opinion has an F1 of at least 0.75 on all three seeds.


**Why this target:**
Opinion is the most numerous label (94 of 200, about 14 per test split), so its F1 should be the highest and the most stable across seeds.

---

## 3.

Answer has an F1 of at least 0.60 and Joke has an F1 of at least 0.50 on all three seeds.


**Why this target:**

Both labels are smaller than Opinion, so their targets are lower. Answer (65 of 200, about 10 per test split) has clear signals such as place names, links and phone numbers. Joke is the smallest label (41 of 200, only about 6 per test split), so one wrong prediction moves its F1 by 0.1–0.15. It also sits on the hardest boundary: sarcastic comments that take a side go to Opinion, which leaves Joke with the subtler cases. That is why its target is the loosest.

---

## 4.

No label is more than 2.5× the size of the smallest label in labels.csv.


**Why this target:**
We are looking to make sure that this is a balanced set that we can find targets for all three categories. If a set is lopsided, the model learns to favor the biggest label and under-predicts the smallest one, which would sink the Joke target in criterion 3.

---

## 5.

The most-confident third of test predictions has an accuracy of at least 0.85, and at least 0.20 higher than the least-confident third, on all three seeds.


**Why this target:**
A confident prediction should be right more often than an unsure one. If the confident third only matched the overall 0.70 from criterion 1, the model's confidence would tell us nothing, so the target sits above it and also requires a clear gap to the least-confident third. The notebook reports both thirds in results.json. Each third is only about 10 comments, so the gap is set at 0.20 (about two comments) rather than anything finer.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
