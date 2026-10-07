# Electrophysiological signatures underlying variability in human memory consolidation — raw intracranial EEG

## Overview
Intracranial EEG (stereo-EEG depth electrodes) from **11 patients** with medically refractory temporal lobe epilepsy
(Sanbo Brain Hospital, Beijing) recorded during (1) learning of object–location associations and (2) targeted memory
reactivation (TMR) with sounds during NREM sleep. Re-packaged in iEEG-BIDS from the authors' public release
(Zenodo record [14885382](https://doi.org/10.5281/zenodo.14885382), concept DOI 10.5281/zenodo.14885381, "Raw data of
article: Electrophysiological signatures underlying variability in human memory consolidation", CC BY 4.0). The Zenodo
record describes itself as "Raw intracranial electrophysiological signals and behavioral data from 11 participants. All
data processed using MATLAB 2019b."

Reference article: Duan W, Xu Z, Chen D, Wang J, Liu J, Tan Z, Xiao X, Lv P, Wang M, Paller KA, Axmacher N, Wang L (2025).
*Electrophysiological signatures underlying variability in human memory consolidation.* Nature Communications 16:2472.
https://doi.org/10.1038/s41467-025-57766-x (PMC11903871). Processed data and analysis code:
https://doi.org/10.5281/zenodo.14583770. Ripple-detection code used by the authors: https://doi.org/10.5281/zenodo.3259369.

## Participants / cohort
Eleven patients (2 females and 9 males; mean age 23.9 ± 5.4 years, SD) with medically refractory temporal lobe epilepsy,
stereotactically implanted with depth contacts to identify epileptogenic zones, recruited at Beijing Sanbo Brain Hospital;
they participated voluntarily without compensation, and all reported normal or corrected-to-normal visual acuity and
normal colour vision (Duan et al. 2025, Methods). Recording years are not stated in the paper or the release.
The release and the paper give sex and age only at group level; `participants.tsv` therefore lists `n/a` for per-subject
age/sex and carries the per-subject recording-site information of Supplementary Table S1 (total recording sites,
hippocampal contacts, electrode laterality, seizure onset zone and laterality) and the per-subject TMR cue counts of
Supplementary Table S3 (total cues and cues per sleep stage N3/N2/N1/wake; number of presentations per cued sound).
`n_electrode_groups` (number of electrode labels in the channel names) is derived from the release itself.

Subject mapping: release file `subNN_raw_data.mat` = `sub-NN` = paper Subject ID NN. Evidence: Table S1's "total
recording sites" equals the channel count of each subject's release file for all 11 subjects (checked by the converter);
the number of `mark_tmr` markers is lowest for sub-01 (150) and sub-02 (230) and 300 for all others, consistent with the
Methods statement that the first two participants of Table S3 heard each cued sound 3 and 4 times instead of 6; and
right-only implants in Table S1 (subjects 1, 5, 8, 9, 10) have only primed electrode labels while the left-only implant
(subject 7) has only unprimed labels.

## Task
- `task-learning`: object–location association learning. 50 objects (small squares with 2.3 cm sides) were associated with
  locations on a grid shown on a monitor (13.6 cm square); each object was paired with a characteristic sound (e.g. goblet
  with breaking sound). After a preview (items shown at their locations; participants instructed to remember the location
  of each item), participants placed each item (self-paced; button press to confirm), then saw the correct location for
  3000 ms as feedback. Rounds (random order) continued until all objects were placed within 3.4 cm of the correct location
  on two consecutive rounds; correctly placed objects dropped out. Learning took 29.9 ± 9.4 min on average. The authors
  extracted the segment from 10 s before the first stimulus to 10 s after the last stimulus.
- `task-sleeptmr`: about 40 min after learning, a pre-sleep test (all 50 objects, no feedback) was given; participants then
  slept with lights off and low-intensity white noise (~55 dB SPL) from a Bluetooth speaker ~0.5 m from the head. When the
  participant entered NREM stage 2 or 3 (detected with scalp EEG), 25 cued sounds (paired during learning; chosen so that
  pre-sleep accuracy was matched for cued and uncued objects) and 25 new control sounds (guitar strum) were presented
  ~5.5 s apart; each sound was presented 6 times (total TMR stimulation about 30 min) (3 and 4 times for the first two
  participants of the paper's Table S3). Segment from 10 s before the first to 10 s after the last stimulus. Offline AASM
  sleep staging (30-s windows) of the cue periods is summarised per subject in Table S3 (`participants.tsv`).
Pre-sleep and post-sleep tests (5.9 ± 0.6 and 5.8 ± 1.1 min) are not part of the release's iEEG arrays.

## Acquisition
Nicolet system (128 channels, 512 Hz; Thermo Nicolet Corporation). Depth electrodes with 8–16 contacts (2 mm long,
0.8 mm diameter, 1.5 mm spacing; Huake-Hengsheng Medical Technology, Beijing), robot-assisted implantation by clinical
need. No seizure occurred during the task period and sleep (paper). For sleep staging the authors also placed scalp EEG
electrodes (F3, F4, C3, C4, O1, O2, A1, A2; a nearby electrode was substituted for clinical reasons in 6 patients) and two
EOG electrodes at the outer canthi; these scalp/EOG channels are not in the release. Contact localisation in the paper
(post-implantation CT co-registered to pre-operative T1 MRI with FreeSurfer v6.0.0, mapped to MNI space) was not released.
Ground and acquisition filters are not documented.

## Preprocessing already applied by the source
None documented beyond the authors' segment extraction (learning and TMR periods). The paper's analysis re-referencing
(common average; nearby white-matter contact for hippocampal contacts) and notch filtering (50/100/150 Hz) were NOT
applied to these data.

## What was converted, and how
Each `subNN_raw_data.mat` (MATLAB 7.3) holds `Raw_ieeg_data_learning`, `Raw_ieeg_data_tmr` (channels × samples, float64),
`label`, `fs` (512), `mark_learning`, `mark_tmr`, `Behavioral_data` (2 × N). Each array became one BrainVision run
(IEEE float32, resolution 1: the file value is the stored value rounded to float32). The round-trip check found
the BrainVision values identical to the stored float64 values for all 22 runs (the stored values are float32-representable). The original
.mat files are copied byte-identically under `sourcedata/zenodo-14885382/` (with the Zenodo record JSON), so the exact
float64 values and `Behavioral_data` remain available. No filtering, resampling, re-referencing or channel removal.

## Files
- `participants.tsv` / `participants.json`: per-subject Table S1 and Table S3 fields (see above).
- `sub-NN/ieeg/sub-NN_task-{learning,sleeptmr}_ieeg.{vhdr,vmrk,eeg,json}`, `_channels.tsv`, `_events.tsv`.
- `sub-NN/ieeg/sub-NN_space-Other_electrodes.tsv` / `_coordsystem.json`: contact names and electrode labels only
  (x, y, z = n/a, because no coordinates are released).
- `sourcedata/zenodo-14885382/`: original .mat files and Zenodo record JSON; `sourcedata/b2zen_provenance_IEEG040.json`.

## Known caveats
- **Units**: the release does not state a physical unit. Values have the amplitude of microvolt-scale iEEG (median |x|
  ≈ 16–32, typical SD 27–68) and are labelled µV. Treat the unit as the authors' export unit, inferred, not documented.
- **Reference**: not documented in the release ("raw" export). The paper's analysis re-referencing (common average; nearby
  white-matter contact for hippocampal contacts) and notch filtering (50/100/150 Hz) were NOT applied to these data.
- **Channel names** are kept exactly as in the release (e.g. `A' 01`). An earlier version of this README stated that a
  prime marks the left hemisphere "in the usual SEEG naming"; in this cohort the release labels and Supplementary Table S1
  point the other way (right-only implants carry only primed labels, the left-only implant only unprimed labels), so do
  not infer hemisphere from the prime without checking. No electrode coordinates are included in the raw release;
  `electrodes.tsv` lists contact names with x/y/z = n/a.
- **Events**: `mark_learning` / `mark_tmr` are stored sample indices (MATLAB 1-based). They are written to `events.tsv`
  (`sample` = value − 1, `onset` = (value − 1)/512 s, plus `source_value`) and as BrainVision markers. The release does
  not label marker types (which item, cued vs. uncued, trial phase), so all markers have `trial_type = stimulus_marker`.
  The number of `mark_learning` entries differs from the number of `Behavioral_data` columns (e.g. sub-01: 404 vs 402;
  per-subject counts in `sourcedata/b2zen_provenance_IEEG040.json`); their correspondence is not documented and is not
  guessed here. The `mark_tmr` counts (150, 230, then 300 for sub-03 to sub-11) are not explained in the release
  (sub-02: 230 markers, whereas 50 sounds × 4 presentations would give 200).
- Per-subject age, sex, handedness and recording dates are not available from the paper or the release.

## How to load
```python
from mne_bids import BIDSPath, read_raw_bids
bp = BIDSPath(root=".", subject="01", task="sleeptmr", datatype="ieeg")
raw = read_raw_bids(bp)          # BrainVision, 512 Hz, SEEG channels
events = raw.annotations         # stimulus_marker annotations from events.tsv
```

## Citation
Duan W, Xu Z, et al. (2025) Electrophysiological signatures underlying variability in human memory consolidation.
Nat Commun 16:2472. doi:10.1038/s41467-025-57766-x; data: doi:10.5281/zenodo.14885382.

## Provenance / sources
Zenodo record 14885382 (REST metadata JSON under `sourcedata/`); Duan et al. 2025 full text (Europe PMC PMC11903871:
Methods, Acknowledgements, Data and Code availability) and Supplementary Information (Tables S1 and S3). Metadata
enrichment 2026-10-07 (see CHANGES).

## Licence
CC BY 4.0, as stated on the Zenodo record (metadata license id `cc-by-4.0`).

## Ethics approval

Verbatim from Duan W, Xu Z, Chen D, Wang J, et al. (2025). Electrophysiological signatures underlying variability in human memory consolidation. Nature Communications 16:2472. https://doi.org/10.1038/s41467-025-57766-x, Methods, "Participants":

> Informed consent was obtained from all participants and study procedures were approved by the ethical committee of Beijing Sanbo brain hospital.
