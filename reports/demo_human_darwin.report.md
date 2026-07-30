# EssayExam — demo_human_darwin.md

8,254 prose words · 233 sentences · 1 sections


## Verdict: **PASS**

- PASS · `machine.median_within_prellm` — median window 0.0074 vs pre-LLM median 0.0785, p75 0.3385  
  <sub>pre-LLM stratum, n=780 windows / 30 papers with arXiv v1 before 2022-11-30</sub>
- PASS · `machine.share_over_half_within_prellm` — 0.0% of windows above 0.5 vs pre-LLM 17.05% (the detector's false-positive floor on definitively human prose)  
  <sub>same stratum</sub>
- PASS · `machine.not_above_published` — median window 0.0074 vs currently-published median 0.2772, p75 0.7959  
  <sub>published 2023-26 stratum, n=1430 windows / 55 papers — a lenient reference, see bands.json</sub>
- PASS · `style.fingerprint_under_p90` — index 0.7% vs corpus p90 8.3%  
  <sub>chi-paper fingerprint, n=597</sub>
- PASS · `style.sentence_rhythm_not_flat` — CV 0.619 vs human p10 0.492  
  <sub>n=116 papers</sub>
- PASS · `prose.defensive_under_p90` — 0.0/1k vs human p90 0.774/1k (below p10)  
  <sub>lexical_bands.json</sub>
- PASS · `prose.redundant_under_p90` — 0.48/1k vs human p90 1.716/1k (p25–median)  
  <sub>lexical_bands.json</sub>
- PASS · `prose.precision_under_p90` — 7.27/1k vs human p90 8.496/1k (p75–p90)  
  <sub>lexical_bands.json</sub>
- PASS · `prose.no_high_severity` — none  
  <sub>analyzers.py</sub>

_Passing means this instrument found no defect. It is not a judgement on the contribution, which is what reviewers actually decide on._


## MACHINE — sliding-window detector

- windows: 204 of 160 words, stride 40
- median window **0.0074** · p90 0.0341 · peak 0.1268
- share of windows above 0.5: **0.0%** · above 0.9: 0.0%

| reference | median window | share > 0.5 | share > 0.9 |
|---|---:|---:|---:|
| **this manuscript** | **0.0074** | **0.0%** | **0.0%** |
| human, pre-ChatGPT (strict) · n=780 | 0.0785 | 17.05% | 4.49% |
| published 2023–26 (lenient) · n=1430 | 0.2772 | 37.9% | 18.95% |

_The pre-ChatGPT row is the detector's false-positive floor: those papers cannot contain generated text, and 17% of their windows still score above 0.5. No single window supports a conclusion._

| peak windows | word range | p(machine) | vs. human |
|---|---|---:|---:|
| #154 | 6160–6320 | 0.1268 | p57.7 |
| #127 | 5080–5240 | 0.0876 | p51.8 |
| #156 | 6240–6400 | 0.0794 | p50.3 |
| #100 | 4000–4160 | 0.0713 | p48.0 |
| #155 | 6200–6360 | 0.0701 | p47.4 |

## STYLE

- fingerprint index **0.7%** (corpus median 0.6%, p90 8.3%, n=597)
- fingerprint load sits in: `emdash_per_1k` 3.4
- sentence length 35.4 words, CV **0.619** (real-paper median 0.58; below ~0.45 reads flat)

## Prose dimensions

| dimension | findings | per 1k | human median | human p90 | position | gated |
|---|---:|---:|---:|---:|---|---|
| DEFENSIVE | 0 | 0.0 | 0.243 | 0.774 | below p10 | yes |
| REDUNDANT | 4 | 0.48 | 0.679 | 1.716 | p25–median | yes |
| PRECISION | 60 | 7.27 | 5.432 | 8.496 | p75–p90 | yes |
| FLOW | 0 | 0.0 | 0.0 | 0.301 | below p10 | diagnostic |

### REDUNDANT — 4

- **filler** (low) · w529 · _On the Origin of Species — excerpt_  
  "in order to"  
  > If, however, a caterpillar were taken out of a hammock made up, for instance, to the third stage, and were put into one finished up to the sixth stage, so that much of its work was  
  → -> to
- **filler** (low) · w3616 · _On the Origin of Species — excerpt_  
  "a large number of"  
  > This instinct may probably be accounted for by the fact of the hens laying a large number of eggs; but, as in the case of the cuckoo, at intervals of two or three days.  
  → -> many / few / some
- **filler** (low) · w6219 · _On the Origin of Species — excerpt_  
  "It is obvious that"  
  > It is obvious that the Melipona saves wax by this manner of building; for the flat walls between the adjoining cells are not double, but are of the same thickness as the outer sphe  
  → -> (delete: if it is evident, show it)
- **filler** (low) · w7173 · _On the Origin of Species — excerpt_  
  "in order to"  
  > The bees must have worked at very nearly the same rate on the opposite sides of the ridge of vermilion wax, as they circularly gnawed away and deepened the basins on both sides, in  
  → -> to

### PRECISION — 60

- **orphan_reference** (medium) · w182 · _On the Origin of Species — excerpt_  
  sentence opens with an unanchored pronoun  
  > It would be easy to show that several distinct mental actions are commonly embraced by this term; but every one understands what is meant, when it is said that instinct impels the   
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w714 · _On the Origin of Species — excerpt_  
  sentence opens with an unanchored pronoun  
  > It can be clearly shown that the most wonderful instincts with which we are acquainted, namely, those of the hive-bee and of many ants, could not possibly have been thus acquired.  
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w745 · _On the Origin of Species — excerpt_  
  sentence opens with an unanchored pronoun  
  > It will be universally admitted that instincts are as important as corporeal structure for the welfare of each species, under its present conditions of life.  
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w827 · _On the Origin of Species — excerpt_  
  sentence opens with an unanchored pronoun  
  > It is thus, as I believe, that all the most complex and wonderful instincts have originated.  
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w2469 · _On the Origin of Species — excerpt_  
  sentence opens with an unanchored pronoun  
  > It may be doubted whether any one would have thought of training a dog to point, had not some one dog naturally shown a tendency in this line; and this is known occasionally to hap  
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w2694 · _On the Origin of Species — excerpt_  
  sentence opens with an unanchored pronoun  
  > It is scarcely possible to doubt that the love of man has become instinctive in the dog.  
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w2872 · _On the Origin of Species — excerpt_  
  sentence opens with an unanchored pronoun  
  > It is not that chickens have lost all fear, but fear only of dogs and cats, for if the hen gives the danger-chuckle, they will run (more especially young turkeys) from under her, a  
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w3143 · _On the Origin of Species — excerpt_  
  sentence opens with an unanchored pronoun  
  > It is now commonly admitted that the more immediate and final cause of the cuckoo’s instinct is, that she lays her eggs, not daily, but at intervals of two or three days; so that,   
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w3286 · _On the Origin of Species — excerpt_  
  sentence opens with an unanchored pronoun  
  > It has been asserted that the American cuckoo occasionally lays her eggs in other birds’ nests; but I hear on the high authority of Dr.  
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w5652 · _On the Origin of Species — excerpt_  
  sentence opens with an unanchored pronoun  
  > It has been remarked that a skilful workman, with fitting tools and measures, would find it very difficult to make cells of wax of the true form, though this is perfectly effected   
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w6219 · _On the Origin of Species — excerpt_  
  sentence opens with an unanchored pronoun  
  > It is obvious that the Melipona saves wax by this manner of building; for the flat walls between the adjoining cells are not double, but are of the same thickness as the outer sphe  
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w6835 · _On the Origin of Species — excerpt_  
  sentence opens with an unanchored pronoun  
  > It was most interesting to me to observe that wherever several bees had begun to excavate these basins near together, they had begun their work at such a distance from each other,   
  → Attach a noun: "This constraint...", "This result...".
- **orphan_reference** (medium) · w8207 · _On the Origin of Species — excerpt_  
  sentence opens with an unanchored pronoun  
  > It was really curious to note in cases of difficulty, as when two pieces of comb met at an angle, how often the bees would entirely pull down and rebuild in different ways the same  
  → Attach a noun: "This constraint...", "This result...".
- **empty_intensifier** (low) · w226 · _On the Origin of Species — excerpt_  
  "very young"  
  > An action, which we ourselves should require experience to enable us to perform, when performed by an animal, more especially by a very young one, without any experience, and when   
  → Delete the intensifier, or replace the pair with one exact word.

_(46 more in the JSON)_
