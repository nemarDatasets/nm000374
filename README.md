# Electrophysiological signatures underlying variability in human memory consolidation — raw intracranial EEG

Intracranial EEG (stereo-EEG depth electrodes) from **11 patients** with medically refractory temporal lobe epilepsy
(Sanbo Brain Hospital, Beijing) recorded during (1) learning of object–location associations and (2) targeted memory
reactivation (TMR) with sounds during NREM sleep. Re-packaged in iEEG-BIDS from the authors' public release
(Zenodo record [14885382](https://doi.org/10.5281/zenodo.14885382), "Raw data of article: Electrophysiological signatures
underlying variability in human memory consolidation", CC BY 4.0).

Reference article: Duan W, Xu Z, Chen D, Wang J, Liu J, Tan Z, Xiao X, Lv P, Wang M, Paller KA, Axmacher N, Wang L (2025).
*Electrophysiological signatures underlying variability in human memory consolidation.* Nature Communications 16:2472.
https://doi.org/10.1038/s41467-025-57766-x. Processed data and analysis code: https://doi.org/10.5281/zenodo.14583770.

## Participants
Eleven patients (2 females and 9 males; mean age 23.9 ± 5.4 years, SD) with medically refractory temporal lobe epilepsy,
stereotactically implanted with depth contacts to identify epileptogenic zones (Duan et al. 2025, Methods). The release
and the paper give sex and age only at group level; `participants.tsv` therefore lists `n/a` for per-subject age/sex and
carries the per-subject recording-site information of Supplementary Table S1 (total recording sites, hippocampal contacts,
electrode laterality, seizure onset zone and laterality). Table S1's "total recording sites" equals the channel count of
each subject's release file (checked by the converter).

## Task
- `task-learning`: object–location association learning. 50 objects (small squares) were associated with locations on a
  grid; each object was paired with a characteristic sound. Participants placed each item, then saw the correct location
  as feedback. The authors extracted the segment from 10 s before the first stimulus to 10 s after the last stimulus.
- `task-sleeptmr`: during NREM sleep (stage 2 or 3, detected with scalp EEG), 25 cued sounds (paired during learning) and
  25 new control sounds (guitar strum) were presented ~5.5 s apart over low-level white noise (~55 dB SPL); each sound was
  presented 6 times (3 and 4 times for the first two participants of the paper's Table S3). Segment from 10 s before
  the first to 10 s after the last stimulus.
Pre-sleep and post-sleep tests are not part of the release's iEEG arrays.

## Recording
Nicolet system (128 channels, 512 Hz; Thermo Nicolet Corporation). Depth electrodes with 8–16 contacts (2 mm long,
0.8 mm diameter, 1.5 mm spacing; Huake-Hengsheng Medical Technology, Beijing), robot-assisted implantation by clinical
need. No seizure occurred during the task period and sleep (paper).

## What was converted, and how
Each `subNN_raw_data.mat` (MATLAB 7.3) holds `Raw_ieeg_data_learning`, `Raw_ieeg_data_tmr` (channels × samples, float64),
`label`, `fs` (512), `mark_learning`, `mark_tmr`, `Behavioral_data` (2 × N). Each array became one BrainVision run
(IEEE float32, resolution 1: the file value is the stored value rounded to float32). The round-trip check found
the BrainVision values identical to the stored float64 values for all 22 runs (the stored values are float32-representable). The original
.mat files are copied byte-identically under `sourcedata/zenodo-14885382/` (with the Zenodo record JSON), so the exact
float64 values and `Behavioral_data` remain available. No filtering, resampling, re-referencing or channel removal.

- **Units**: the release does not state a physical unit. Values have the amplitude of microvolt-scale iEEG (median |x|
  ≈ 16–32, typical SD 27–68) and are labelled µV. Treat the unit as the authors' export unit, inferred, not documented.
- **Reference**: not documented in the release ("raw" export). The paper's analysis re-referencing (common average; nearby
  white-matter contact for hippocampal contacts) and notch filtering (50/100/150 Hz) were NOT applied to these data.
- **Channel names** are kept exactly as in the release (e.g. `A' 01`; a prime marks the left hemisphere in the usual
  SEEG naming). No electrode coordinates are included in the raw release, so no `electrodes.tsv` is provided.
- **Events**: `mark_learning` / `mark_tmr` are stored sample indices (MATLAB 1-based). They are written to `events.tsv`
  (`sample` = value − 1, `onset` = (value − 1)/512 s, plus `source_value`) and as BrainVision markers. The release does
  not label marker types (which item, cued vs. uncued, trial phase), so all markers have `trial_type = stimulus_marker`.
  The number of `mark_learning` entries differs from the number of `Behavioral_data` columns (e.g. sub-01: 404 vs 402;
  per-subject counts in `sourcedata/b2zen_provenance_IEEG040.json`); their correspondence is not documented and is not
  guessed here.

## Licence
CC BY 4.0, as stated on the Zenodo record (metadata license id `cc-by-4.0`).
