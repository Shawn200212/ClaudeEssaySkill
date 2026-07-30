# Adversarial iteration protocol for `/chi-paper`

A GAN-shaped optimisation loop for a *prompt-and-reference* artifact rather than a
weight matrix. The analogy is load-bearing in three places and breaks in one; both
are written down here so later rounds do not quietly drift into self-congratulation.

## Role mapping

| GAN | Here | Sees | Blind to |
|---|---|---|---|
| Generator **G** | The skill under test — a subagent given *only* `chi-paper/` and one scenario | Skill files, scenario text | The attack hypothesis, the rubric, the oracle |
| Adversary **A** (对抗器) | Scenario generator — a white-box attacker that reads the skill first | Skill files, oracle, previous rounds' surviving failures | G's answers, the verifier's rubric |
| Discriminator **D** (验证器) | Blind scorer against corpus ground truth | Scenario, G's answer, oracle, fixed rubric | Whether the scenario was adversarial, and what A intended to break |
| Gradient step | A patch to the skill files, authored by the main agent from D's findings | — | — |

**D is blind to A on purpose.** A verifier told "this case was designed to expose a
missing SLR lens" will find a missing SLR lens whether or not one is missing. Keeping
D blind means an attack that fails to land shows up as a *high* score, which is the
only way the loop can report that an attack missed.

**A is white-box on purpose.** A black-box adversary rediscovers the same surface
failures every round. Letting A read the skill pushes it toward the boundaries of
what is written down, which is where the real gaps are.

## Where the analogy breaks

A GAN converges to a Nash equilibrium because G and D update against each other
continuously. Here the update is discrete, hand-authored, and *monotone*: text only
gets added or corrected, never gradient-descended. Two consequences:

1. **Overfitting is the failure mode, not mode collapse.** The skill can memorise
   the adversary's cases. Countermeasure: a held-out scenario split that never
   informs a patch, and a reported train/holdout gap. Prior rounds established the
   calibration — a 7–12 point gap is the *expected* advantage of a specialised
   lens; a gap above ~20 means the patches memorised the test.
2. **There is no automatic loss.** D's score is an LLM judgement, so it is anchored
   to `chi_official/oracle.json` — a census of CHI 2024–2026, where "the median CHI
   paper cites N works" is a counted fact, not an opinion. Every quantitative
   deduction D issues must name the oracle field it contradicts.

## Scoring rubric (fixed across rounds — changing it invalidates comparison)

Five dimensions, 0–100 each, reported separately and as an unweighted mean.

| Dimension | Fails when |
|---|---|
| **Coverage** | The answer does not address what was actually asked, or silently narrows it |
| **Grounding** | A quantitative or conventional claim is asserted with no traceable source, or contradicts the oracle, or quotes an `n` the corpus does not have. **Fabricating a number is capped at 20 on this dimension regardless of other quality.** |
| **Genre fit** | Wrong lens for the paper's genre — judging a research-through-design paper by controlled-experiment standards, or vice versa; or forcing a default-route paper into a specialised reference |
| **Actionability** | Advice a competent author could not act on: platitudes, restatement of the problem, "consider carefully" |
| **Calibration** | Overclaims what the evidence supports, undersells a real strength, presents a stylometric index as an AI-detection verdict, or omits a caveat the source requires (open-access bias, small `n`, lower-bound phrasing) |

## Round loop

```
for round r:
  1. A reads skill + oracle + surviving_failures[r-1]  ->  K scenarios
  2. split scenarios by md5(salt + scenario_id) -> 60% train / 40% holdout
  3. G answers ALL scenarios (blind)
  4. D scores ALL answers (blind to adversarial intent)
  5. main agent patches skill using TRAIN failures only
  6. re-run G+D on HOLDOUT -> generalisation gap
  7. surviving_failures[r] = holdout failures not fixed by the patch
```

## Stopping criterion

Stop when **any two** of:

- Holdout mean gains < 3 points over the previous round.
- No *new* failure family appears for two consecutive rounds (loop-until-dry).
- Train−holdout gap sits in the 7–12 band with holdout ≥ 80.

Do **not** stop merely because scores are high: a high score with a novel failure
family still open means the adversary found something the patch has not addressed.

## Anti-degeneracy rules

- A patch that merely names the adversary's scenario ("for SLR papers, do X") is
  rejected. Patches must generalise to the *family*, not the instance.
- A round that produces no failures is evidence the adversary was too weak, not
  that the skill is finished. Escalate the adversary before declaring convergence.
- Numbers introduced by a patch must come from `oracle.json` or `calibration.json`
  with their `n` and caveat, never from the model's prior.
