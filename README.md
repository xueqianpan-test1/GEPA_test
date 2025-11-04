# GEPA Experiment Code README

## Project Overview

This notebook demonstrates an Agent Routing optimization experiment using DSPy's GEPA (Generative Prompt Adaptation). It routes user messages to appropriate agents (shopping, music/video, web search, image search, travel, etc.).

## Environment Requirements

### Python Version
- Python 3.12.1 (recommended)

### Required Packages

```bash
pip install dspy-ai pandas scikit-learn openai python-dotenv
```

Key dependencies:
- `dspy-ai`: DSPy framework
- `pandas`: Data processing
- `scikit-learn`: Dataset splitting (train_test_split)
- `openai`: OpenAI API client
- `python-dotenv`: Environment variable management

### Environment Variables Configuration

1. Create a `.env` file in the project root:
```bash
OPENAI_API_KEY=your_openai_api_key_here
```

2. SSL Certificate Configuration (if using a proxy):
   - Place SSL certificate at `~/.certs/netskope_openai.pem`
   - If no proxy is needed, comment out or remove SSL-related lines

## Dataset Preparation

1. Dataset Location:
   - Default path: `/Users/xueqian.pan/Documents/code/project/shopping/ninjaeval_mlflow/ninjaeval_mlflow/dataset`
   - Requires `Handoff*.json` files in this directory

2. Dataset Format:
   - JSON files containing a `conversations` array
   - Each conversation contains a `conversation` array
   - User messages must include `role: "user"` and a `target_handoff` field

3. Dataset Files:
   - Loads all `Handoff*.json` files (excludes `Handoff.json` and `HandoffAll.json`)
   - Includes: `HandoffShopping.json`, `HandoffMusicsearch.json`, `HandoffVideosearch.json`, `HandoffAiwebsearch.json`, `HandoffImagesearch.json`, `HandoffHotelsearch.json`, `HandoffWeather.json`, etc.

## Configuration

### GEPA Configuration Parameters

You can adjust the following parameters in Step 3 of the notebook:

```python
config_set = {
    "base_model": "openai/gpt-4.1",           # Inference model
    "base_temperature": 1,                     # Temperature parameter
    "base_max_tokens": 5000,                   # Max tokens
    "base_top_p": 1,                           # Top-p parameter
    "base_seed": 42,                           # Random seed
    "reflection_model": "openai/gpt-4.1",      # Reflection model (for prompt optimization)
    "classification": False,                    # Whether to use classification mode
    "classes": [...],                          # List of available agents
    "reflection_minibatch_size": 30,           # Number of samples per iteration
    "opt_mode": 'light',                      # Optimization mode: 'light', 'medium', 'heavy'
    "prompt_max_tokens": 500,                  # Max prompt length
}
```

### Optimization Mode

- `light`: ~1k metric calls, fast testing
- `medium`: 3k~5k metric calls, balanced
- `heavy`: 10k+ metric calls, thorough optimization (recommended)

## Usage Steps

1. Environment Setup (Step 1)
   - Load environment variables
   - Configure OpenAI API and SSL certificates

2. Load Dataset (Step 2)
   - Load data from JSON files
   - Data preprocessing and cleaning
   - Split into train/validation/test sets

3. GEPA Setup (Step 3)
   - Configure DSPy model
   - Define Signature and Module
   - Set up evaluation metrics

4. Model Evaluation (Optional)
   - Evaluate initial model performance

5. GEPA Optimization
   - Run optimization process
   - View optimization history
   - Evaluate optimized model

## Important Notes

1. API Costs
   - Uses OpenAI API, which incurs costs
   - GEPA optimization makes multiple API calls; monitor budget
   - `heavy` mode uses more calls

2. Dataset Path
   - Modify `base_dir` to your actual path
   - Ensure dataset files exist and are correctly formatted

3. SSL Certificate
   - If no proxy is needed, remove or comment out SSL-related configuration
   - If using a proxy, ensure the certificate path is correct

## Output Results

- `df_history`: Contains validation scores and prompt text for each iteration
- `df_result`: Contains prediction results, correctness, etc. for each test message
- `optimized_program`: Optimized program that can be used directly for inference
