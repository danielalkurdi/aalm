# Australian Administrative Law Model (AALM)
## Introduction
The Australian Administrative Law Model (**AALM**) is a model trained on Australian administrative law.

## Workflow
- Data prep: Use `notebooks/AALM_Data_Preparation_Semchunk.ipynb` to build an Administrative Law–focused dataset from `isaacus/open-australian-legal-corpus` using `semchunk`. By default it saves to `data/aalm-adminlaw-semchunk`.
- Fine-tune: Run `notebooks/AALM_QLoRA_Finetune.ipynb`. It will prefer the local dataset at `DATASET_DIR` (defaults to `data/aalm-adminlaw-semchunk`) and otherwise build a tiny fallback sample from the Corpus for sanity checks.
- Inference/Merge: Use `notebooks/AALM_Inference_Merge.ipynb` to load the adapter for inference or merge it into base weights.

Environment knobs (override via env vars in notebooks):
- `DATASET_DIR`: path to prepared dataset (`data/aalm-adminlaw-semchunk`).
- `CORPUS_DATASET`: HF dataset id (`isaacus/open-australian-legal-corpus`).
- `CHUNK_SIZE`/`OVERLAP`: semchunk parameters (defaults 1024 and 0.2).
