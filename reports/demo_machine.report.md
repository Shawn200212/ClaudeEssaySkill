# EssayExam — demo_machine.md

1,651 prose words · 107 sentences · 1 sections


## Verdict: **FAIL**

- FAIL · `machine.median_within_prellm` — median window 1.0000 vs pre-LLM median 0.0785, p75 0.3385  
  <sub>pre-LLM stratum, n=780 windows / 30 papers with arXiv v1 before 2022-11-30</sub>
- FAIL · `machine.share_over_half_within_prellm` — 100.0% of windows above 0.5 vs pre-LLM 17.05% (the detector's false-positive floor on definitively human prose)  
  <sub>same stratum</sub>
- FAIL · `machine.not_above_published` — median window 1.0000 vs currently-published median 0.2772, p75 0.7959  
  <sub>published 2023-26 stratum, n=1430 windows / 55 papers — a lenient reference, see bands.json</sub>
- FAIL · `style.fingerprint_under_p90` — index 16.0% vs corpus p90 8.3%  
  <sub>chi-paper fingerprint, n=597</sub>
- FAIL · `style.sentence_rhythm_not_flat` — CV 0.419 vs human p10 0.492  
  <sub>n=116 papers</sub>
- FAIL · `prose.defensive_under_p90` — 6.06/1k vs human p90 0.774/1k (above p95)  
  <sub>lexical_bands.json</sub>
- PASS · `prose.redundant_under_p90` — 1.21/1k vs human p90 1.716/1k (p75–p90)  
  <sub>lexical_bands.json</sub>
- FAIL · `prose.precision_under_p90` — 19.38/1k vs human p90 8.496/1k (above p95)  
  <sub>lexical_bands.json</sub>
- PASS · `prose.no_high_severity` — none  
  <sub>analyzers.py</sub>

_Passing means this instrument found no defect. It is not a judgement on the contribution, which is what reviewers actually decide on._


## MACHINE — sliding-window detector

- windows: 39 of 160 words, stride 40
- median window **1.0000** · p90 1.0000 · peak 1.0000
- share of windows above 0.5: **100.0%** · above 0.9: 100.0%

| reference | median window | share > 0.5 | share > 0.9 |
|---|---:|---:|---:|
| **this manuscript** | **1.0000** | **100.0%** | **100.0%** |
| human, pre-ChatGPT (strict) · n=780 | 0.0785 | 17.05% | 4.49% |
| published 2023–26 (lenient) · n=1430 | 0.2772 | 37.9% | 18.95% |

_The pre-ChatGPT row is the detector's false-positive floor: those papers cannot contain generated text, and 17% of their windows still score above 0.5. No single window supports a conclusion._

| peak windows | word range | p(machine) | vs. human |
|---|---|---:|---:|
| #2 | 80–240 | 1.0000 | p100.0 |
| #3 | 120–280 | 1.0000 | p100.0 |
| #4 | 160–320 | 1.0000 | p100.0 |
| #5 | 200–360 | 1.0000 | p100.0 |
| #19 | 760–920 | 1.0000 | p100.0 |

## STYLE

- fingerprint index **16.0%** (corpus median 0.6%, p90 8.3%, n=597)
- fingerprint load sits in: `sentence_len_CV` 62.6, `hollow_per_1k` 21.8
- sentence length 15.4 words, CV **0.419** (real-paper median 0.58; below ~0.45 reads flat)

## Prose dimensions

| dimension | findings | per 1k | human median | human p90 | position | gated |
|---|---:|---:|---:|---:|---|---|
| DEFENSIVE | 10 | 6.06 | 0.243 | 0.774 | above p95 | yes |
| REDUNDANT | 2 | 1.21 | 0.679 | 1.716 | p75–p90 | yes |
| PRECISION | 32 | 19.38 | 5.432 | 8.496 | above p95 | yes |
| FLOW | 4 | 2.42 | 0.0 | 0.301 | above p95 | diagnostic |

### DEFENSIVE — 10

- **disclaimer** (medium) · w278 · _(front matter)_  
  "It is important to note"  
  > It is important to note that the concept of adaptivity is not new.  
  → Announces significance instead of demonstrating it. State the point directly.
- **uncited_attribution** (medium) · w291 · _(front matter)_  
  "Prior work" with no citation  
  > Prior work has explored
various dimensions of this design space over several decades.  
  → Name the works, or drop the appeal to a consensus you are not citing.
- **uncited_attribution** (medium) · w348 · _(front matter)_  
  "Some researchers" with no citation  
  > Some researchers argue that adaptivity can significantly improve task performance and user
satisfaction.  
  → Name the works, or drop the appeal to a consensus you are not citing.
- **disclaimer** (medium) · w395 · _(front matter)_  
  "It should be noted that"  
  > It should be noted that resolving this tension is likely to require careful
consideration of multiple competing factors.  
  → Delete the frame; keep the claim.
- **uncited_attribution** (medium) · w586 · _(front matter)_  
  "Several studies" with no citation  
  > Several studies have investigated the relationship between adaptivity and perceived control.  
  → Name the works, or drop the appeal to a consensus you are not citing.
- **uncited_attribution** (medium) · w633 · _(front matter)_  
  "Some scholars" with no citation  
  > Some scholars have argued that the
benefits of personalization outweigh these concerns, while others maintain that transparency
mechanisms are essential.  
  → Name the works, or drop the appeal to a consensus you are not citing.
- **uncited_attribution** (medium) · w671 · _(front matter)_  
  "Prior studies" with no citation  
  > Prior studies have identified a number of factors that
may moderate user reactions, including technical expertise, prior experience with similar
systems, and various personality ch  
  → Name the works, or drop the appeal to a consensus you are not citing.
- **disclaimer** (medium) · w696 · _(front matter)_  
  "further research is needed"  
  > It is worth noting that this literature is
still relatively nascent, and further research is needed to establish robust findings.  
  → Non-committal closer. Name the specific next study or cut the sentence.
- **disclaimer** (medium) · w1404 · _(front matter)_  
  "It is important to note"  
  > It is important to note that our study has a number of limitations.  
  → Announces significance instead of demonstrating it. State the point directly.
- **disclaimer** (medium) · w1588 · _(front matter)_  
  "further investigation is needed"  
  > In
particular, further investigation is needed to elucidate the mechanisms underlying individual
variation in responses to adaptivity.  
  → Non-committal closer. Name the specific next study or cut the sentence.

### REDUNDANT — 2

- **filler** (low) · w146 · _(front matter)_  
  "in order to"  
  > Our
approach leverages a mixed-methods methodology, combining quantitative telemetry with rich
qualitative insights, in order to delve into the nuances of this complex phenomenon.  
  → -> to
- **filler** (low) · w810 · _(front matter)_  
  "in order to"  
  > We employed a mixed-methods approach in order to develop a comprehensive understanding of our
research questions.  
  → -> to

### PRECISION — 32

- **orphan_reference** (medium) · w47 · _(front matter)_  
  sentence opens with an unanchored pronoun  
  > It is deliberately written in the
register that generated academic prose tends toward — it is not a real paper, and its
claims, citations and findings are fabricated.*
Abstract  
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w278 · _(front matter)_  
  sentence opens with an unanchored pronoun  
  > It is important to note that the concept of adaptivity is not new.  
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w696 · _(front matter)_  
  sentence opens with an unanchored pronoun  
  > It is worth noting that this literature is
still relatively nascent, and further research is needed to establish robust findings.  
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w1020 · _(front matter)_  
  sentence opens with an unanchored pronoun  
  > It is important to acknowledge that our approach has certain limitations, which we discuss in
greater detail below.  
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w1404 · _(front matter)_  
  sentence opens with an unanchored pronoun  
  > It is important to note that our study has a number of limitations.  
  → Attach a noun: "This constraint...", "This result...".
- **vague_term** (low) · w0 · _(front matter)_  
  "Holistic"  
  > Toward a Holistic Framework for Understanding User Engagement with Adaptive Interfaces  
  → name what it includes
- **vague_term** (low) · w98 · _(front matter)_  
  "significant"  
  > However, despite the growing body of
literature in this space, there remains a significant gap in our understanding of how users
actually experience adaptivity in situ.  
  → statistically significant, or large? give the number
- **vague_term** (low) · w124 · _(front matter)_  
  "shed light on"  
  > In this paper, we present a comprehensive framework
that seeks to shed light on the multifaceted nature of engagement with adaptive systems.  
  → shows
- **vague_term** (low) · w146 · _(front matter)_  
  "complex"  
  > Our
approach leverages a mixed-methods methodology, combining quantitative telemetry with rich
qualitative insights, in order to delve into the nuances of this complex phenomenon.  
  → complex in what respect
- **vague_term** (low) · w170 · _(front matter)_  
  "underscore"  
  > Our
findings underscore the importance of considering both individual differences and contextual
factors when designing adaptive experiences.  
  → shows
- **vague_term** (low) · w187 · _(front matter)_  
  "meaningful"  
  > We believe these results pave the way for future
research and offer meaningful implications for practitioners seeking to build more effective
and intuitive systems.  
  → meaningful on which measure
- **vague_term** (low) · w291 · _(front matter)_  
  "various"  
  > Prior work has explored
various dimensions of this design space over several decades.  
  → name them or give the count
- **vague_term** (low) · w348 · _(front matter)_  
  "significantly"  
  > Some researchers argue that adaptivity can significantly improve task performance and user
satisfaction.  
  → statistically significant, or large? give the number
- **vague_term** (low) · w381 · _(front matter)_  
  "crucial"  
  > This tension
between personalization and predictability represents a crucial challenge for the design
community.  
  → crucial to what outcome

_(18 more in the JSON)_

### FLOW — 4

- **no_connectives** (medium) · w11 · _(front matter)_  
  65-word argumentative paragraph, 4 sentences, no connective  
  > *Machine-generated control document.  
  → Sentences are adjacent but not related. Name the relation (contrast, cause, example) at least once.
- **no_connectives** (medium) · w212 · _(front matter)_  
  66-word argumentative paragraph, 3 sentences, no connective  
  > The proliferation of digital technologies has fundamentally transformed the way people
interact with information systems.  
  → Sentences are adjacent but not related. Name the relation (contrast, cause, example) at least once.
- **no_connectives** (medium) · w348 · _(front matter)_  
  65-word argumentative paragraph, 4 sentences, no connective  
  > Some researchers argue that adaptivity can significantly improve task performance and user
satisfaction.  
  → Sentences are adjacent but not related. Name the relation (contrast, cause, example) at least once.
- **no_connectives** (medium) · w849 · _(front matter)_  
  62-word argumentative paragraph, 4 sentences, no connective  
  > Participants were recruited through a combination of institutional mailing lists and social
media channels.  
  → Sentences are adjacent but not related. Name the relation (contrast, cause, example) at least once.
