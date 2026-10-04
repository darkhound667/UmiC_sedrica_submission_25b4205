# Perception – run instructions

## Setup
Python 3 with `numpy`, `pandas`, `matplotlib`, `opencv-python` and `jupyter`:

```bash
pip install numpy pandas matplotlib opencv-python jupyter
```

## Run
Open each notebook from **inside this `Perception/` folder** (the data folder `SeDriCa_perception_starter_data/` sits next to the notebooks and is read by relative path), then **Kernel → Restart & Run All**. No GPU, ROS or model training is needed.

```bash
cd Perception
jupyter notebook
```

| Notebook | Question parts |
|---|---|
| `Q1_a-e_lane_centre.ipynb` | Q1 (a) annotated frames, (b) baseline, (c) evidence panel, (d) revision with confidence, (e) error and unknown-rate table, failure frames |
| `Q2_a_signal_flow.ipynb` | Q2 (a) signal-flow diagram and annotated frames |
| `Q2_b_baseline_rule.ipynb` | Q2 (b) baseline GO / SLOW / STOP rule and timeline |
| `Q2_c-e_stale_message_stop_timing_trials.ipynb` | Q2 (c) message age and revised rule, (d) stopping distance, (e) 20-trial test, false alarm and dangerous delay |
| `Q4_a_map_fingerprints.ipynb` | Q4 (a) fingerprint plots and look-alike pairs |
| `Q4_b-e_belief_filter.ipynb` | Q4 (b) single-scan baseline, (c) belief filter, (d) heatmap and uncertainty rule, (e) corrupted-beam test |

Each notebook runs independently and produces its own figures and tables.

## Settings to know
- **Q1:** baseline white threshold 500, revised threshold 650, near row v = 260, far row v = 170.
- **Q2 (e):** random seed 42, 20 trials.
- **Q4:** likelihood exp(−MAE / 0.15), move 0.8 / 0.2, stay 0.9 / 0.1, uncertain if the largest belief is below 0.5; 4.5 m beams interpolated for the baseline; (e) uses seed 42.

## Evaluation files
`*_reference.csv` files are read only after predictions exist, for scoring (Q1 (e), Q2 (d)/(e), Q4 (d)/(e)). No detector or filter reads them.
