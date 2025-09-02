# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

AALM (Australian Administrative Law Model) - A machine learning project for training models on Australian administrative law data. The project implements a complete MLOps pipeline from data preparation through fine-tuning to inference, using `openai/gpt-oss-20b` as the base model with QLoRA fine-tuning.

## MLOps Workflow

The project follows a three-stage pipeline implemented in Jupyter notebooks:

1. **Data Preparation**: `notebooks/AALM_Data_Preparation_Semchunk.ipynb` - Filters and chunks legal corpus data
2. **Fine-tuning**: `notebooks/AALM_QLoRA_Finetune.ipynb` - QLoRA fine-tuning on GPT-OSS-20B 
3. **Inference/Merge**: `notebooks/AALM_Inference_Merge.ipynb` - Model inference and adapter merging

## Environment Setup

```bash
# Activate virtual environment (Python 3.12)
source .venv/bin/activate

# Start Jupyter lab for development
jupyter lab

# Deactivate when done
deactivate
```

## Core Development Commands

```bash
# Complete MLOps pipeline execution (run notebooks in order):
# 1. Data preparation
jupyter nbconvert --to notebook --execute notebooks/AALM_Data_Preparation_Semchunk.ipynb

# 2. Fine-tuning (requires GPU with adequate VRAM for QLoRA)
jupyter nbconvert --to notebook --execute notebooks/AALM_QLoRA_Finetune.ipynb

# 3. Inference and optional merge
jupyter nbconvert --to notebook --execute notebooks/AALM_Inference_Merge.ipynb

# Check GPU availability and VRAM
python -c "import torch; print(f'CUDA available: {torch.cuda.is_available()}, GPU: {torch.cuda.get_device_name() if torch.cuda.is_available() else None}')"
```

## Project Architecture

**Data Pipeline**: Uses `isaacus/open-australian-legal-corpus` → keyword filtering → `semchunk` chunking → local dataset at `data/aalm-adminlaw-semchunk`

**Model Architecture**: 
- Base: `openai/gpt-oss-20b` (20B parameter model)
- Fine-tuning: QLoRA (4-bit quantization) with LoRA adapters
- Target modules: `q_proj`, `k_proj`, `v_proj`, `o_proj` for GPT-OSS architecture
- Chat template: Uses model's built-in "harmony" template

**Administrative Law Filtering**: Keywords include tribunals (AAT, NCAT, VCAT), judicial review concepts, FOI, procedural fairness, natural justice, etc.

## Key Dependencies

**ML/Training Stack:**
- `transformers` - Hugging Face transformers library
- `datasets` - Dataset management and processing
- `peft` - Parameter-Efficient Fine-Tuning (LoRA/QLoRA)
- `bitsandbytes` - 4-bit quantization support
- `trl` - Transformers Reinforcement Learning (SFTTrainer)
- `accelerate` - Distributed training support

**Text Processing:**
- `semchunk` - Semantic chunking for legal texts
- `tiktoken` - OpenAI tokenizer utilities

**Core Infrastructure:**
- `torch` - PyTorch ML framework
- `jupyter` / `jupyterlab` - Notebook environment

## Configuration Environment Variables

All notebooks support environment variable overrides for key parameters:

**Data Preparation:**
- `CORPUS_DATASET`: HF dataset ID (default: `isaacus/open-australian-legal-corpus`)
- `OUTPUT_DIR`: Local dataset path (default: `data/aalm-adminlaw-semchunk`)
- `CHUNK_SIZE`: Token chunk size (default: 1024)
- `OVERLAP`: Chunk overlap ratio (default: 0.2)
- `DOC_LIMIT`: Document limit for testing (default: 0 = unlimited)

**Fine-tuning:**
- `BASE_MODEL`: Base model (default: `openai/gpt-oss-20b`)
- `DATASET_DIR`: Prepared dataset path (default: `data/aalm-adminlaw-semchunk`)
- `OUTPUT_DIR`: Adapter output (default: `outputs/aalm-gpt-oss-20b-qlora`)
- `MAX_STEPS`: Training steps (default: 300)
- `BATCH_SIZE`: Per-device batch size (default: 1)
- `GRAD_ACCUM`: Gradient accumulation steps (default: 32)
- `MAX_SEQ_LENGTH`: Sequence length (default: 2048)
- `LEARNING_RATE`: Learning rate (default: 2e-4)

**Hardware Requirements:**
- Fine-tuning requires GPU with sufficient VRAM (tested with QLoRA 4-bit quantization)
- Data preparation can run on CPU
- Inference requires GPU for practical performance

## Legal Domain Specifics

**Australian Administrative Law Coverage:**
- Administrative Appeals Tribunal (AAT) decisions
- Civil and Administrative Tribunals (NCAT, VCAT, QCAT, ACAT)
- Judicial review and procedural fairness concepts
- Freedom of Information (FOI) matters
- Delegated legislation and ministerial decisions

**System Prompt**: Model is configured as "AALM, the Australian Administrative Law Model" with instructions to answer legal questions accurately and cite sources appropriately.

## Development Workflow

**Iterative Development:**
1. Modify filtering keywords or chunking parameters in data prep notebook
2. Re-run data preparation if dataset changes needed
3. Adjust training hyperparameters in fine-tuning notebook
4. Monitor training loss and adjust MAX_STEPS as needed
5. Test inference quality and iterate

**GPU Memory Management:**
- Uses 4-bit quantization to reduce VRAM requirements
- Gradient checkpointing enabled to trade compute for memory
- Batch size of 1 with high gradient accumulation for stability

**Output Management:**
- Prepared datasets saved to `data/` directory
- Model adapters saved to `outputs/` directory  
- Merged models saved to separate output directories
- All outputs are git-ignored via `.gitignore`