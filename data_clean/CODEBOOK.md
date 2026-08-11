# Codebook: Cleaned Data Files

Accompanies "Social Value Orientation Predicts Directional Search in Allocation Decisions"
(Jekel, Lisovoj, & Fiedler). These files are **derived** from the raw survey exports (which
remain unchanged in `data/`) — they contain only the variables used in the reported analyses,
with consistent, readable column names across all three studies. Every number in these files
was spot-checked against the corresponding published correlation/statistic in the manuscript
(see verification notes at the end of this file).

**Provenance:** These cleaned files, this codebook, and the verification checks below were
created by Claude (Claude Sonnet 5, Anthropic), working from the existing analysis script
(`SVO_ASE_SCRIPT.Rmd`) and raw data files, at the authors' request. All analytic decisions
about what to include and how to verify it were reviewed by the authors, but the extraction,
column naming, and spot-checks themselves were carried out by Claude, not manually by the
authors.

Seven files:

- `{study}_participants.csv` — one row per participant (Pilot Study, Study 1, Study 2)
- `{study}_search_trials.csv` — one row per participant per trial, first search only (Pilot Study, Study 1, Study 2)
- `study2_search_trials_full_sequence.csv` — Study 2 only: one row per participant per trial **per search step** (up to 8 rows per trial), covering the full reveal-by-reveal sequence, not just the first search

`{study}` is one of `pilot`, `study1`, `study2`, matching the manuscript's terminology
(NOT the internal script labels "Study 1"/"Study 2"/"Study 3", which numbered these
differently during data collection).

## A note on missing values (NA)

Most `NA`s in these files are **structural**, not missing data: the column is entirely `NA`
(100% of rows) for a given study because that variable does not apply to that study's design
at all, not because anything went unrecorded. Only one column has genuine, partial missing
data. Full breakdown:

| File | Column | NA count | Why |
|---|---|---|---|
| `pilot_participants.csv` | `svo_angle_slider` | 306/306 (all) | Pilot Study only used the Ring Measure, not the Slider Measure. |
| `pilot_participants.csv` | `svo_angle_mean` | 306/306 (all) | Only computed for Study 2, where both measures were collected in the same session. |
| `pilot_participants.csv` | `condition_preference_rating` | 306/306 (all) | This manipulation only existed in Study 1. |
| `pilot_search_trials.csv` | `search_target` | 6,120/6,120 (all) | Pilot Study had no target dimension (other's payoff was always fully visible). |
| `pilot_search_trials.csv` | `search_cost_eur` | 6,120/6,120 (all) | Search was free in the Pilot Study. |
| `study1_search_trials.csv` | `search_target`, `search_cost_eur` | 3,760/3,760 (all) | Same reasons as the Pilot Study above. |
| `study2_participants.csv` | `condition_preference_rating` | 217/217 (all) | This manipulation only existed in Study 1, not Study 2. |
| **`study1_participants.csv`** | **`age`** | **3/188 (partial)** | **The only genuine missing data in these files: 3 participants did not report their age.** |

`study2_search_trials.csv` and `study2_search_trials_full_sequence.csv` have zero `NA`s.

## `{study}_participants.csv`

| Column | Type | Description |
|---|---|---|
| `participant_id` | string | Unique ID, prefixed by study (e.g. `study2_143`). Not linkable across studies — participants were recruited independently for each. |
| `study` | string | `pilot`, `study1`, or `study2`. |
| `age` | numeric | Age in years. |
| `gender` | string | `female`, `male`, `diverse`, `prefer_not_to_say`, or `other`, depending on what the original survey offered (response options differed slightly by study). |
| `svo_angle_ring` | numeric | SVO angle (degrees) from the Ring Measure (Liebrand & McClintock, 1988). |
| `svo_angle_slider` | numeric | SVO angle (degrees) from the Slider Measure (Murphy et al., 2011). `NA` for the Pilot Study, which only used the Ring Measure. |
| `svo_angle_mean` | numeric | Average of `svo_angle_ring` and `svo_angle_slider`. Only computed for Study 2, where both measures were collected in the same session; `NA` for the Pilot Study and Study 1, where the two measures (where available) were collected up to 14 months apart and analyzed separately instead. |
| `condition_preference_rating` | string | `yes`/`no`, Study 1 only: whether this participant rated their preference for each option before searching (the awareness-manipulation between-subjects condition). `NA` for the Pilot Study and Study 2, which did not have this manipulation. |

## `{study}_search_trials.csv`

| Column | Type | Description |
|---|---|---|
| `participant_id` | string | Links to the participants file. |
| `study` | string | `pilot`, `study1`, or `study2`. |
| `trial` | integer | Trial number within the search task (1-20 for the Pilot Study and Study 1; 1-24 for Study 2). |
| `search_step` | integer | Which search this row describes within the trial. Always `1` (first/only search) for the Pilot Study and Study 1, which allowed only one reveal per trial. For Study 2, this file reports only the **first** search per trial; the full reveal-by-reveal sequence (up to 8 searches per trial) is in the separate `study2_search_trials_full_sequence.csv` file below. |
| `search_target` | string | `self` or `other`: whose payoff the revealed information concerned. `NA` for the Pilot Study and Study 1, where the other's payoff was always fully visible and only the own payoff could ever be searched (so this dimension does not apply). |
| `search_option` | string | `higher_own_payoff` or `lower_own_payoff`: which of the two options was searched (identified by which one had the visibly higher own payoff before any reveal). |
| `chose_option` | string | `higher_own_payoff` or `lower_own_payoff`: which option the participant ultimately chose in this trial. |
| `total_reveals_in_trial` | integer | Study 2 only: how many of the up to 8 concealed amounts were revealed in this trial before the participant chose. `NA` for the Pilot Study and Study 1 (always exactly 1 by design). |
| `search_cost_eur` | numeric | Study 2 only: monetary cost (in Euro) actually deducted for information revealed in this trial (\euro{}0.10 per additional reveal beyond the first free one, from the participant's own payoff; a matching \euro{}0.10 was also deducted from the other participant's payoff per reveal, not tracked separately here). `NA` for the Pilot Study and Study 1, where search was free. |

## `study2_search_trials_full_sequence.csv`

Same idea as `study2_search_trials.csv`, but with one row per search step actually taken
(not just the first). A trial with 3 reveals before the participant chose contributes 3 rows
here (`search_step` = 1, 2, 3); a trial where the participant stopped after the first free
reveal contributes only 1 row.

| Column | Type | Description |
|---|---|---|
| `participant_id` | string | Links to `study2_participants.csv`. |
| `study` | string | Always `study2`. |
| `trial` | integer | Trial number (1-24). |
| `search_step` | integer | 1st, 2nd, 3rd, ... reveal within this trial (up to 8). |
| `search_target` | string | `self` or `other`: whose payoff this specific reveal concerned. |
| `search_option` | string | `higher_own_payoff` or `lower_own_payoff`: which option this specific reveal belonged to. |
| `chose_option` | string | The option ultimately chosen in this trial (repeated on every row of the same trial, for convenience). |
| `total_reveals_in_trial` | integer | Total number of reveals in this trial (repeated on every row of the same trial). |
| `search_cost_eur` | numeric | Total cost deducted for this trial (repeated on every row of the same trial; not broken down per individual reveal). |

## Verification

Every correlation below was recomputed directly from these exported CSV files (not from the
original analysis script) and matched the corresponding published value in the manuscript:

| Study | Test | Recomputed from export | Published |
|---|---|---|---|
| Pilot | SVO (ring) vs. % search higher-own-payoff | rho = -.571 | rho = -.57 |
| Study 1 | SVO (ring) vs. % search higher-own-payoff | rho = -.246 | rho = -.25 |
| Study 1 | SVO (slider) vs. % search higher-own-payoff | rho = -.219 | rho = -.22 |
| Study 1 | % search vs. % choice (higher-own-payoff) | rho = .391 | rho = .39 |
| Study 2 | SVO (mean) vs. % first-search higher-own-payoff | rho = -.339 | rho = -.34 |
| Study 2 | SVO (mean) vs. % first-search own-outcome | rho = -.398 | rho = -.40 |
| Study 2 | % own-outcome targeted, step 1 (n=5,208) | 92.5% | 92.5% |
| Study 2 | % own-outcome targeted, step 2 (n=2,138) | 91.6% | 91.6% |
| Study 2 | % own-outcome targeted, step 3 (n=854) | 90.4% | 90.4% |
| Study 2 | % own-outcome targeted, step 4 (n=636) | 85.5% | 85.5% |

The last four rows (from `study2_search_trials_full_sequence.csv`) match the manuscript's
"Search behavior by search step" table exactly, including the trial counts at each step.
