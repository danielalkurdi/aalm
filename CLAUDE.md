# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

AALM (Australian Administrative Law Model) - A machine learning project focused on training models on Australian administrative law data. This is currently an early-stage research project using Jupyter notebooks for exploration and development.

## Environment Setup

The project uses Python 3.12 with a virtual environment containing data science and ML packages.

```bash
# Activate virtual environment
source .venv/bin/activate

# Start Jupyter lab (preferred) or notebook
jupyter lab
# or
jupyter notebook

# Deactivate when done
deactivate
```

## Current Project Structure

```
aalm/
├── .venv/                    # Python virtual environment with ML packages
├── .ipynb_checkpoints/       # Jupyter notebook checkpoints (auto-generated)
├── Untitled.ipynb          # Main development notebook (currently empty)
└── pyproject.toml          # Python project configuration (currently empty)
```

## Key Dependencies

The virtual environment includes essential packages for data science and ML:

**Core Data Science Stack:**
- `pandas==2.3.2` - Data manipulation and analysis
- `numpy==2.3.2` - Numerical computing
- `matplotlib-inline`, `ipython_pygments_lexers` - Visualization support

**Jupyter Environment:**
- `jupyter==1.1.1` - Complete Jupyter suite
- `jupyterlab==4.4.6` - Modern notebook interface (recommended)
- `notebook==7.4.5` - Classic notebook interface
- `ipykernel==6.30.1` - Python kernel for Jupyter

**Web & HTTP:**
- `requests==2.32.5` - HTTP requests
- `httpx==0.28.1` - Modern async HTTP client
- `beautifulsoup4==4.13.5` - HTML parsing

## Development Commands

```bash
# Start development environment
source .venv/bin/activate && jupyter lab

# Check installed packages
pip list

# Install additional packages (add to requirements.txt when stable)
pip install package_name

# Check Python environment
python --version  # Should be 3.12.x
```

## Architecture Notes

**Early Development Stage:** The project is currently in initial setup phase with basic notebook infrastructure. No specific ML models or data processing pipelines have been implemented yet.

**Notebook-Driven Development:** Following a notebook-first approach typical for ML research and experimentation. As the project matures, consider extracting reusable code into Python modules.

**Australian Administrative Law Focus:** The project is specifically targeting Australian administrative law, suggesting the need for:
- Legal document processing capabilities
- Australian legal system understanding
- Compliance with Australian data handling regulations
- Domain-specific terminology and legal precedent processing

## Recommended Next Steps (Development Guidance)

When developing this project:

1. **Data Collection:** Consider Australian legal databases (AustLII, Federal Court decisions, AAT decisions)
2. **Text Processing:** May require legal document parsing, case law extraction, citation analysis
3. **Model Training:** Consider transformer models fine-tuned for legal text (Legal-BERT variants)
4. **Compliance:** Ensure adherence to Australian privacy laws and legal data usage requirements

## Working with Legal Data

Legal ML projects require special considerations:
- **Citation Parsing:** Australian legal citations follow specific formats
- **Hierarchical Structure:** Administrative law has jurisdictional hierarchies (Federal → State → Local)
- **Temporal Factors:** Legal precedent has time-based relevance and supersession rules
- **Privacy/Confidentiality:** Some administrative decisions may contain sensitive information

## Jupyter Workflow

- Use meaningful notebook names instead of "Untitled.ipynb"
- Consider organizing notebooks by purpose: data_exploration.ipynb, model_training.ipynb, evaluation.ipynb
- Use markdown cells extensively to document legal concepts, methodology, and findings
- Save intermediate results and models to avoid re-computation of expensive operations

## Future Structure Considerations

As the project grows, consider organizing into:
```
aalm/
├── data/                    # Legal datasets, processed documents
├── notebooks/               # Jupyter notebooks for exploration
├── src/aalm/               # Reusable Python modules
├── models/                 # Trained ML models
├── requirements.txt        # Dependency specification
└── README.md              # Project documentation
```