# /chi-paper claims vs. the CHI 2024–2026 census

> Instrument: `chi_official/oracle.json`, built from a **census** (not a sample) of ACM CHI
> main-conference papers 2024–2026, n=4,011, enumerated from the DBLP tables of contents which
> mirror the ACM proceedings. Metadata from OpenAlex (99.8% DOI match) and Crossref.
> Audited 2026-07-29. Direct audit — findings from the adversarial rounds are logged separately.

The prior corpus (`CC_chi_paper`, n=597 full texts) is arXiv-derived and its own calibration file
says so: *"arXiv open-access (cs.HC and adjacent); NOT a census of ACM CHI proceedings."* Every
number the skill inherited from it is therefore a claim about **HCI preprints on arXiv**, which is
what this audit tests against the actual proceedings.

---

## D1 · Abstract length — the skill's number is wrong and the error is one-directional

**Skill says.** `structure-and-argument.md:31` heads the section *"Abstract (~150–250 words, one
paragraph)"*; `format-and-compliance.md:108` repeats *"typically ~150–250 words"*;
`prose-and-citations.md:46` gives the empirical anchor as 161 words [140, 196] corpus-wide and
147 [141, 169] for the CHI-family subset.

**Census says.** Median **149 words**, and the distribution is far tighter than any of those
ranges: p25 143, p75 151, and **72.5% of all CHI abstracts fall in 140–160 words**. Only 1.3%
exceed 200 words.

**Why it is tight.** This is not extraction truncation — that was checked. Of the 502 abstracts
landing at exactly 150 words, 100% end on a complete sentence, a *lower* mid-sentence rate than the
rest of the corpus (3%). The tightness is an authoring constraint, and the rule is stated on the
venue's own page: the **CHI 2026 Papers page requires "abstract (150 words max)"**
(chi2026.acm.org/authors/papers/, verified 2026-07-29). The CHI 2027 Papers page does not restate
it — which is exactly why the skill's standing instruction to re-verify per cycle matters here, and
why the census is the safer anchor: the 140–160 band holds across all three years regardless of
which year's page happens to spell the rule out.

**Severity: high, and asymmetric.** The skill's stated range tops out at 250 — 67% above the limit.
An author following it writes an abstract that the submission form will reject, and discovers this
at the deadline. The corpus figure (147) was accidentally close to right but was presented as a
soft median rather than as a hard cap, so it carried no warning.

**Fix.** Lead with the rule, not the distribution: 150 words, ±10 tolerance. Keep the census figure
as corroboration. Delete "~150–250" from both files.

**Consequence in practice.** The draft this audit was run against carried an abstract well over the
limit, and no prior pass had caught it — because no prior pass had the right number to check against.
That is the whole argument for measuring rather than remembering.

---

## D2 · Body length — a units mismatch that pushes papers toward the desk-reject band

**Skill says.** `structure-and-argument.md:132` and `prose-and-citations.md:45`: CHI-family papers
run a median **11,322 body words** (IQR 9,531–12,804, n=210). This sits in the same skill as
`format-and-compliance.md:23`, which correctly reports CHI 2027's **5,000–8,000 encouraged** band
and **12,000 desk-reject** threshold.

**The mismatch.** `body_words` in the corpus is everything the extractor found before the
References heading. It includes figure and table captions, table cell contents, section headings,
page furniture, and any inline appendix. CHI's counted-word rule excludes references, captions, and
appendices. The two numbers are in different units and are printed ~100 lines apart in the same
skill without a conversion.

**Measured, not assumed.** Re-parsing the 335 venue-anchored full texts with captions and
appendices stripped: raw body median 11,675 → **counted-rule median 10,529** (ratio 0.93). So
caption-stripping closes only ~1,100 words of a ~3,000-word gap. The rest is the arXiv sampling
frame: preprint versions are routinely the extended author versions, not the camera-ready. CHI's
own page states the average paper is **~7,000–8,000 words** on the counted rule.

**Severity: high.** An author who reads "median 11,322, IQR floor 9,531" concludes an 8,600-word
draft is *short* and pads it. The official band says that draft is already at the top of the
encouraged range. The skill's own compliance file has the right answer; the structural files talk
over it.

**Note the split verdict.** `scripts/audit_docx.py` is *correct* — it implements the counting rule
properly and reports "~counted words (CHI rule): 8633 … above the 5000-8000 encouraged band". So the
tooling and the compliance file agree with the venue, and only the two prose reference files
disagree. This is the cleanest possible case for the fix being a text edit, not a re-measurement.

**Year drift to record while here.** CHI 2026's Papers page defines standard-length papers as
**5,000–12,000 words, "about 7,000–8,000 on average"**; CHI 2027's page states an encouraged
**5,000–8,000** band with the same 12,000 desk-reject threshold. The skill's format file already
tracks this correctly. Both years agree on the counted-word *rule*; neither is anywhere near 11,322.

**Consequence in practice.** A draft sitting comfortably inside the official encouraged band reads,
against the skill's structural anchor, as roughly 900 words *short* of the IQR floor. The skill would
have advised padding — in the wrong direction, toward the desk-reject threshold.

**Fix.** In both structural files, state the official counted band first, label the corpus figure
explicitly as *raw pre-references extraction, not the CHI counting rule*, and give the 0.93 caption
ratio so the two can be reconciled rather than confused.

---

## D3 · Reference count — right number, wrong year: CHI bibliographies are growing

**Skill said.** `chi-reviewer-preferences.md:122` and `prose-and-citations.md:48`: median **82
references** (IQR 62–108, n=209), stated flat, with no year resolution.

**Two instrument errors had to be cleared before the real finding appeared.**

*First*, OpenAlex `referenced_works_count` gave a census median of 69, which would have made the
skill look 19% inflated. Validating against Crossref's publisher-deposited `reference-count`:
OpenAlex resolves a **median 80.4%** of each ACM bibliography (~17 references missing per paper),
because it counts only references it can match to an indexed work. It is a lower bound and must
never be quoted raw. The oracle now reads Crossref and records the fallback rate.

*Second*, the corrected check on 400 papers gave a median of exactly 82 — an apparently perfect
confirmation of the skill. It was an artifact: those 400 were the first records in file order, and
the file is in year order, so the sample was **entirely CHI 2024**.

**The finding.** Across the full census (n=4,006 publisher-deposited counts):

| | median | IQR | p90 |
|---|---|---|---|
| CHI 2024 | 82 | 63–105 | 131 |
| CHI 2025 | 87 | 70–112 | 139 |
| CHI 2026 | **91** | 71–116 | 140 |

CHI bibliographies are growing by roughly 4–5 references per cycle. The skill's 82 was not wrong —
it matches CHI 2024 exactly, which is the era its arXiv corpus sampled — but it was pooled and
stale, and a flat number cannot show a trend. For a CHI 2027 submission the working target is **~90**.

**Severity: medium, and it compounds.** A stale reference target does not fail a submission on its
own, but it is the number authors use to decide when related work is "done". The more useful
statistic is the **p25 of 71**: below it a paper sits in the bottom quarter of its cohort for
engagement, which is what an AC is seeing when a reviewer writes "related work is thin".

**Live consequence.** the audited draft has a reference count well below the census median, and the standing plan was to raise it to
55–65. Against CHI 2026 that target is still below the p25 of 71 — the plan needs to move to
**~75–90**, and that is 40+ references to find and integrate before 9/10, not 20.

**Method note worth keeping.** Both errors here were sampling or instrument artifacts that pointed
the wrong way — one made the skill look wrong when it was right, the other made it look right when
it was stale. Neither would have surfaced without checking a second source and checking the
denominator.

---

## D4 · Title craft — a total coverage gap in the highest-traffic sentence of the paper

**Skill says.** Nothing. No reference file discusses the title. `figures.md` covers captions,
`structure-and-argument.md` starts at the abstract.

**Census says.** CHI titles are a genre with measurable conventions: median **12 words**
(IQR 10–15), and **74.1% contain a colon** — the "Hook: Descriptive subtitle" form is the CHI
default, not a stylistic option. The share is stable across all three years (71.9 / 72.6 / 76.5%)
and rises to **80.2%** in the generative-AI agents-and-games pool.

**Severity: medium.** Not a desk-reject risk, but the title is the one string every reviewer, AC,
and skimmer reads, and it is the field where an author with no CHI publication history has no
intuition to fall back on. The skill has 12 reference files and none of them mention it.

---

## D5 · Subfield share — the author's positioning claim is now measurable

Not a skill defect; a fact the skill cannot currently supply and that changes how this author should
position.

Over three years of CHI, papers combining a generative-AI/LLM term with a game/player/NPC term
number **26** (0.6% of the conference). Adding character/persona/embodied-agent papers without a
game term brings it to **106** (2.6%). The trend is real but recent: 25 papers in 2024, 18 in 2025,
**63 in 2026** — the subfield roughly tripled in the most recent cycle.

Read the 2.6% carefully before quoting it. Only **45%** of the 80 `core_agent` papers involve an
avatar, a virtual character, or any embodied representation; the rest are text-only chatbots
(mental-health support, persuasion, tutoring). The population that actually shares this paper's
problem — generative AI driving an *embodied* character — is roughly **62 papers, 1.5% of CHI**.
Use 0.6% for games specifically, 1.5% for embodied generative characters, and 2.6% only if the
claim genuinely includes chatbots.

Two consequences. First, a reviewer drawn from this area is unlikely; the paper will be read by
adjacent generalists, which is precisely the "write for the gate-holder" case
`chi-reviewer-preferences.md` already argues, now with a number behind it. Second, "CHI has not
addressed this" is defensible at 0.6% — but 2026's tripling means the related-work sweep must be
re-run close to submission or the paper will miss the wave it is riding.

---

## D6 · Corpus provenance caveat is understated in three files

`domain-craft.md:101`, `research-framework.md:155`, and `figures.md:41` quote sub-field base rates
("this pool writes more tables", "survey/questionnaire 49%") from the arXiv pool without repeating
the arXiv-bias caveat that `prose-and-citations.md` carries. Read alone — which is how SKILL.md
routes them — they read as statements about CHI. They are statements about HCI preprints.

Low severity individually; systematic across the skill, and it is the mechanism behind D1 and D2.

---

## Summary

| # | Claim | Verdict | Severity |
|---|---|---|---|
| D1 | Abstract ~150–250 words | **Wrong** — 150 max is a submission-form rule; census median 149 | High |
| D2 | CHI body median 11,322 words | **Wrong units** — official counted band is 7,000–8,000 | High |
| D3 | 82 references, flat | **Stale** — 82/87/**91** by year; target ~90, p25 71 | Medium |
| D4 | Title conventions | **Missing** — 74% colon rate, median 12 words, undocumented | Medium |
| D5 | Subfield share | **Missing** — 0.6% core-games, 1.5% embodied, 2.6% incl. chatbots | Medium |
| D6 | arXiv provenance caveat | **Inconsistently applied** across three files | Low, systematic |

All six are patched as of 2026-07-29; the adversarial round-2 holdout tests whether the patches
generalise or merely memorised these cases. Three of them changed a live revision plan: an abstract
had to be cut to the 150-word limit, a body length that had looked short turned out to be correctly
sized and was not padded, and a reference target rose to roughly **75–90** to sit near the census
median of 82.
