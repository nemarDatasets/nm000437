# NeuroListen ("Listening to the Brain"): sEEG auditory-reconstruction features (derivative)

Stereo-EEG from **five patients with epilepsy** (native Mandarin Chinese speakers with basic English; 25-40 years,
mean 31) listening to spoken Mandarin Chinese words (44 categories), Mandarin digits (0-9) and English words (20
categories), 2 s per word. The paper reports more than 10 hours of neural-speech paired recordings. Re-packaged from
the authors' public release (Zenodo record [17426506](https://doi.org/10.5281/zenodo.17426506), CC-BY-4.0).

Reference article: Zhang X, Wang R, Sun J, Cai S, Li H. *Listening to the Brain: Multi-Band sEEG Auditory
Reconstruction via Dynamic Spatio-Temporal Hypergraphs.* NeurIPS 2025 Datasets and Benchmarks Track.
https://doi.org/10.52202/085713-4302. Code: https://github.com/NeuroListen/NeuroListen

Recording (paper): SEEG shafts (Sinovation SDE-10/12/16, 0.8 mm, 3.5 mm spacing, 8-13 shafts and 118-186 contacts per
participant), Nihon Kohden EEG-1200 at 1000 Hz. A marker sent to the DC channel aligned stimuli and sEEG.

## This is a derivative dataset
The release provides the authors' **per-trial feature arrays**, not the continuous recordings. The authors removed
contacts with epileptiform activity (61, 27, 46, 22 and 37 in participants 1-5) and applied bipolar re-referencing.
They then computed three feature sets at 200 Hz:
- `desc-hga`: high-gamma activity (70-150 Hz band-pass, analytic amplitude).
- `desc-lfs`: low-frequency signal (low-pass 100 Hz).
- `desc-bbs`: broadband, which combines LFS and HGA. Its channel count equals HGA plus LFS, and the provenance JSON
  records whether its values equal the stacked HGA and LFS arrays.

All three are normalised within each 2 s window. Each trial is one 2 s array (400 samples × channels).

## Layout
- `sub-<NN>/ieeg/sub-<NN>_task-listen_desc-<band>_ieeg.vhdr`: one epoched BrainVision file per participant and feature
  set, with the trials back to back in release order (`RecordingType: epoched`, `EpochLength: 2`). Values are the stored
  float64 values rounded to float32. The round-trip check compares every epoch with its release array.
- `..._events.tsv`: one row per trial (epoch onset, word, language CN / EN / DIGIT, release index and file name).
- Channel names are `hga001`, `lfs001`, ... (column order of the release arrays). The release does not give the bipolar
  contact pairs, so `electrodes.tsv` lists names only, with `n/a` coordinates. Units are `n/a` (normalised features).
- `stimuli/audio/`: the stimulus recordings (`<word>_<CN|EN>.wav`, `<digit>_DIGIT.wav`, 24 kHz mono) per participant,
  verbatim. `stimuli/mel/`: the authors' 80-bin mel-spectrograms of the stimuli (100 frames per word), verbatim.
- Age and sex are not given per participant (`n/a`). Trial counts per language are in `participants.tsv`.

## Not included: the five "Original EEG" files
The release also contains `Original_EEG_Files.zip`: five Nihon Kohden `.EEG` excerpts (`SubN_Sample_Original.EEG`,
0.2-0.44 GB each). They are **not packaged here**, for three reasons:
1. They come without the Nihon Kohden channel table (`.21E`), patient (`.PNT`) and log (`.LOG`) files, so channels
   cannot be named or typed.
2. MNE's Nihon Kohden reader does not parse their EEG-1200A extended layout (it returns one 1-second channel).
3. Their headers carry full recording date-times (year, month, day and time) in several places, including binary
   fields that cannot be masked reliably without the format specification.

They remain available from the Zenodo record.

## Additional details from the paper

Compiled by the campaign enrichment lane (batch3/enrich/c-listening, 2026-10-08) from the reference article and the deposit; section numbers refer to the article. No electrode coordinates or anatomical labels are available in the deposit or the article.

Paper: Zhang X, Wang R, Sun J, Cai S, Li H. "Listening to the Brain: Multi-Band sEEG Auditory Reconstruction via Dynamic Spatio-Temporal Hypergraphs", NeurIPS 2025 Datasets & Benchmarks Track (https://openreview.net/forum?id=koxXKW4FbR; proceedings PDF https://proceedings.neurips.cc/paper_files/paper/2025/file/bbc6396ae4b4dbf78ce328aa8beafd27-Paper-Datasets_and_Benchmarks_Track.pdf). Section numbers below refer to it.

**Task/paradigm.** Passive listening: per round 44 Mandarin Chinese words, 10 Mandarin digits (0-9) and 20 English words, 2 s per word, presented from a speaker; 5-s ready interval with the prompt "Please listen to the speech attentively" then a "ding"; 5-10 min breaks between rounds; > 10 h of neural-speech pairs in total (Sec 3.4). Stimulus markers were sent to a DC channel of the amplifier while the speech was played (Sec 3.5).

**Participants.** Five patients with epilepsy, native Mandarin speakers with basic English, aged 25-40 (mean 31) (Sec 3.1). Per-subject age/sex: n/a.

**Recording system.** Nihon Kohden EEG 1200, 1000 Hz (Sec 3.3). Raw deposit files `Original_EEG_Files/SubN_Sample_Original.EEG` have the header "EEG-1200A V01.00" (Nihon Kohden), recording dates June 2023 in the header; the .21E/.PNT/.LOG companion files are not deposited, so electrode names are not recoverable from the deposit (Voyager Job probe).

**Reference scheme.** Bipolar re-referencing of the remaining contacts after excluding contacts with epileptiform activity (61, 27, 46, 22, 37 per subject) (Sec 4.2). Derivatives: high-gamma (70-150 Hz analytic amplitude), low-frequency (< 100 Hz) and broadband, downsampled to 200 Hz, normalised per 2-s window (Sec 4.2); deposit zips `subN_bipolar_listen_seeg_2000_{hga,lfs,bbs}.zip` hold one 400 x C .npy per trial (C = 101, 145, 148, 109, 73 bipolar channels).

**Electrode types.** Platinum-iridium sEEG shafts, Sinovation (Beijing) Medical Technology SDE-10/12/16, diameter 0.8 mm, inter-contact distance 3.5 mm, 10-16 contacts per shaft; 8-13 shafts and 118-186 contacts per subject (Sec 3.2-3.3).

**Localisation.** Contact positions were determined with LeGUI (SPM12-based) after co-registering pre-implantation MRI with post-implantation CT (Sec 3.2, Fig. 3 caption). Figure 3 shows shaft trajectories for subjects 1-3 (shaft letter: contact count) without region names. No coordinates/labels deposited.

## License

CC-BY-4.0
