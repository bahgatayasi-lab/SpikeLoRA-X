# SpikeLoRA-X

**SpikeLoRA-X** is an explainable and responsible extension of the SpikeLoRA spiking-neural-network forecaster for multi-horizon renewable-energy forecasting. It keeps the SpikeLoRA-based forecasting backbone unchanged and adds post-hoc analysis for horizon-wise attribution, attribution fidelity, physical plausibility, and spike-activity reporting.

This repository accompanies the CAEPIA/LNAI paper:

> *SpikeLoRA-X: Explainable, Responsible, and Energy-Efficient Spiking Neural Networks for Multi-Horizon Renewable-Energy Forecasting at the Edge*

## Repository contents

- `spikelora_x_Expemental_source_code.py` — Colab-exported experimental source code for training and evaluating SpikeLoRA-X.
- `spikelora_x_outputs/` — generated automatically when the script is executed. It contains CSV tables and attribution figures.

The filename currently follows the uploaded repository name. For clarity, a future commit may rename it to `spikelora_x_experimental_source_code.py`.

## What the code does

The script implements the main SpikeLoRA-X workflow:

1. loads renewable-energy time-series datasets;
2. builds chronological train/validation/test splits;
3. trains an SNN-TCN backbone;
4. freezes the backbone and adapts a SpikeLoRA head;
5. evaluates forecasting metrics;
6. produces feature-attribution heatmaps;
7. tests attribution fidelity by masking top-attributed features;
8. checks physical plausibility violations;
9. summarizes spike activity as a hardware-agnostic edge-efficiency proxy;
10. exports CSV and PNG files for manuscript tables and figures.

## Datasets

The main CAEPIA experiments use:

- **Solar Radiation (SR)**: `Palestine-Solar.csv`, target variable `GHI`.
- **Wind Power (WP)**: `Turky-Wind-power-Turbine.csv`, target variable `LV ActivePower (kW)`.

The code also contains optional dataset entries for:

- **Wind Speed (WS)**: `Palestine-wind.csv`.
- **Electricity Consumption (EC)**: `Moroco-power-consumption.csv`.

Clean benchmark datasets and the original SpikeLoRA implementation from the previous paper are available in the public SpikeLoRA-SNN repository:

<https://github.com/bahgatayasi-lab/SpikeLoRA-SNN>

## Environment

The released script was exported from Google Colab and installs the main dependencies directly in the notebook/script. For a cleaner local setup, install dependencies from `requirements.txt`:

```bash
pip install -r requirements.txt
```

Core dependencies:

- Python
- PyTorch
- torchvision
- torchaudio
- SpikingJelly
- NumPy
- pandas
- scikit-learn
- tqdm
- matplotlib

The script automatically selects a CUDA device when available and otherwise falls back to CPU:

```python
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
```

The experiments are software simulations of spiking-neural-network dynamics. No neuromorphic chip is required to reproduce the reported numerical results.

## Reproducibility settings

The main experiment uses the following default configuration:

| Setting | Value |
|---|---:|
| Random seed | 0 |
| Lookback window | 96 time steps |
| Forecast horizons | [1, 2, 4, 8, 24] |
| Batch size | 128 |
| SNN simulation steps | 8 |
| SpikeLoRA rank | 8 |
| SpikeLoRA scaling alpha | 16 |
| SpikeLoRA LIF gate threshold | 0.05 |
| Backbone pretraining epochs | 10 |
| SpikeLoRA adaptation epochs | 10 |
| Backbone learning rate | 1e-3 |
| SpikeLoRA adaptation learning rate | 5e-3 |
| Train/validation/test split | 70% / 10% / 20%, chronological |

## Quick start in Google Colab

1. Open the script or paste it into a Colab notebook.
2. Upload the dataset CSV files to the Colab working directory.
3. Run all cells from top to bottom.
4. Download the generated files from `spikelora_x_outputs/`.

## Quick start locally

The current `.py` file was exported from Colab and contains notebook shell commands such as `!pip install`. For command-line execution, first remove those notebook-only lines or convert the notebook to a clean Python script. Then run:

```bash
pip install -r requirements.txt
python spikelora_x_Expemental_source_code.py
```

Expected output files include:

- `spikelora_x_outputs/Table_backbone_sanity_caepea.csv`
- `spikelora_x_outputs/Table_attribution_fidelity_caepea.csv`
- `spikelora_x_outputs/Table_physics_plausibility_caepea.csv`
- `spikelora_x_outputs/Table_spike_activity_caepea.csv`
- `spikelora_x_outputs/Attribution_heatmap_SR.csv`
- `spikelora_x_outputs/Attribution_heatmap_SR.png`
- `spikelora_x_outputs/Attribution_heatmap_WP.csv`
- `spikelora_x_outputs/Attribution_heatmap_WP.png`

## Capturing the exact environment

For camera-ready reproducibility, run `environment_capture.py` in the same environment used for the final experiments and copy the printed values into the manuscript.

```bash
python environment_capture.py
```

## Relation to the original SpikeLoRA repository

SpikeLoRA-X is not a replacement for SpikeLoRA. The original SpikeLoRA repository provides the parameter-efficient spiking forecasting framework and cleaned benchmark datasets. SpikeLoRA-X adds a Responsible-AI post-hoc layer around that backbone, including attribution, attribution-fidelity checks, physical-plausibility checks, and spike-activity reporting.

Original SpikeLoRA-SNN repository:

<https://github.com/bahgatayasi-lab/SpikeLoRA-SNN>

## Citation

If you use this code, please cite the SpikeLoRA-X CAEPIA/LNAI paper and the original SpikeLoRA paper/repository.

```bibtex
@inproceedings{ayasi2026spikelorax,
  title     = {SpikeLoRA-X: Explainable, Responsible, and Energy-Efficient Spiking Neural Networks for Multi-Horizon Renewable-Energy Forecasting at the Edge},
  author    = {Ayasi, Bahgat Waleed Deeb and coauthors},
  booktitle = {Proceedings of CAEPIA 2026},
  series    = {Lecture Notes in Artificial Intelligence},
  year      = {2026},
  note      = {Code: https://github.com/bahgatayasi-lab/SpikeLoRA-X}
}
```

## License

Add a license file before final public release. If no license is added, external users do not have clear permission to reuse, modify, or redistribute the code.
