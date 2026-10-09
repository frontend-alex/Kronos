# Kronos checkout guide

This checkout contains the Kronos financial time-series foundation-model project and supporting prediction, fine-tuning, and visualization workflows.

## Provenance

The original README identifies the upstream Kronos research project, authors, paper, and pretrained NeoQuasar models. Preserve that attribution and the [license](../LICENSE). This guide documents how to inspect this checkout; it does not attribute development or pretraining of the foundation model to the owner of this repository.

When presenting the project in a portfolio, identify your own experiments or contributions through their commits and results. The existence of an example in this checkout alone does not establish who authored it.

## Repository map

| Path | Purpose |
| --- | --- |
| [../README.md](<../README.md>) | Original research overview and usage reference |
| [../model](<../model>) | Model, tokenizer, and prediction implementation |
| [../examples](<../examples>) | Prediction and market-data examples |
| [../finetune](<../finetune>) | Fine-tuning and dataset utilities |
| [../finetune_csv/README.md](<../finetune_csv/README.md>) | CSV-oriented fine-tuning guide |
| [../webui/README.md](<../webui/README.md>) | Web interface setup |
| [../tests/test_kronos_regression.py](<../tests/test_kronos_regression.py>) | CPU prediction regression tests |
| [../requirements.txt](<../requirements.txt>) | Core Python dependencies |

## Core setup

Use a Python environment compatible with the checked-in dependency versions and a PyTorch installation appropriate for your machine:

```bash
git clone https://github.com/frontend-alex/Kronos.git
cd Kronos
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

On Windows, use the environment's Scripts activation command. Model and tokenizer loading through from_pretrained requires access to the Hugging Face model files, or a correctly populated local cache. Additional market-data and web-interface examples may need dependencies beyond the core requirements; follow their local guides.

## Prediction example

[examples/prediction_example.py](../examples/prediction_example.py) loads NeoQuasar/Kronos-Tokenizer-base and NeoQuasar/Kronos-small, then predicts from a candlestick CSV.

Run it from examples because its import and data paths are relative to the working directory:

```bash
cd examples
python prediction_example.py
```

Before running, provide data/XSHG_5min_600977.csv relative to that directory or deliberately update the example's path to your own dataset. The expected columns are timestamps, open, high, low, close, volume, and amount. The example uses a 400-row lookback and a 120-row prediction horizon, so its input must cover that range. It displays a Matplotlib comparison plot.

## Verification

Pytest is used by the test source but is not declared in the core requirements file:

```bash
python -m pip install pytest
python -m pytest tests/test_kronos_regression.py
```

Run those commands from the repository root. The regression tests use CPU execution, fixed seeds, pinned model/tokenizer revisions, and committed CSV fixtures. They still require downloaded model weights or a populated cache and can take substantial time. No weight downloads, inference, fine-tuning, or tests were executed for this documentation update.

Regression agreement measures implementation stability; it does not establish future-market forecasting performance.

## Limits and review priorities

- Example scripts differ in their data sources, configuration, and working-directory assumptions.
- Fine-tuning needs a suitable dataset, preprocessing, compute, and evaluation plan.
- Historical plots or sample prediction JSON files are not an independent benchmark of predictive usefulness.
- Document any local modifications separately from upstream capabilities and retain upstream acknowledgments.
