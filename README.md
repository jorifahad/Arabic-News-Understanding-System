# Unified Arabic News Understanding System

This project is a unified Arabic Natural Language Processing system that analyzes news articles through three parallel tasks:

- Classifies the article topic using CAMeLBERT-MSA
- Generates an abstractive three-point summary using AraT5
- Extracts Who, What, When, Where, and Source using Gemini 2.5 Flash

Each model processes the same cleaned article independently. The outputs are then combined into one end-to-end news analysis result.

## ⚙️ System Workflow

```text
Arabic News Article
        │
        ▼
Shared Preprocessing
        │
        ├── CAMeLBERT-MSA → Topic Classification
        ├── AraT5 → Abstractive Summarization
        └── Gemini 2.5 Flash → 5W Extraction
        │
        ▼
Unified News Analysis Output
```

The models run independently in parallel, so an error in one component does not propagate to the others.

## 🧠 Models and Tasks

| Task | Model | Output |
|---|---|---|
| Topic Classification | CAMeLBERT-MSA | Predicted news category |
| Abstractive Summarization | AraT5v2-base-1024 | Three-point Arabic summary |
| 5W Information Extraction | Gemini 2.5 Flash | Who, What, When, Where, and Source |

## 📊 Results

### Topic Classification

CAMeLBERT-MSA was compared with a BiLSTM baseline.

| Model | Accuracy | Macro-F1 |
|---|---:|---:|
| BiLSTM | 0.85–0.89 | 0.82–0.86 |
| CAMeLBERT-MSA | **0.98** | **0.98** |

CAMeLBERT-MSA performed better on long articles, rare Arabic vocabulary, and semantically similar categories.

### Abstractive Summarization

Three model and dataset configurations were evaluated.

| Model and Dataset | ROUGE-1 | ROUGE-2 | ROUGE-L |
|---|---:|---:|---:|
| AraT5 on XL-Sum | 0.180 | 0.040 | 0.170 |
| mT5-small on AsDs | 0.034 | 0.007 | 0.034 |
| AraT5 on Arabic News + Gemini | **0.380** | **0.231** | **0.370** |

The Arabic News dataset with Gemini-generated summaries achieved the best results, showing that high-quality training summaries were more effective than larger noisy datasets.

### 5W Extraction

Gemini produced structured JSON containing:

- Who
- What
- When
- Where
- Source

The model showed strong formatting consistency and returned `غير مذكور` when information was unavailable instead of inventing unsupported details.

## 🚀 How to Run the Project

### 1. Install Dependencies

```bash
pip install torch transformers datasets pandas numpy matplotlib scikit-learn google-generativeai sentencepiece accelerate
```

### 2. Configure the Gemini API Key

Do not place the real key directly in `config.py`.

Use:

```python
import os

GEMINI_API_KEY = os.getenv("GEMINI_API_KEY")
```

Set the key before running the notebook.

Windows PowerShell:

```powershell
$env:GEMINI_API_KEY="your_api_key"
```

Mac/Linux:

```bash
export GEMINI_API_KEY="your_api_key"
```

### 3. Update Model Paths

Update the saved model paths inside `News_Ai.ipynb`:

```python
CAMEL_MODEL_DIR = "Models/CAMeLBERT_classifier"
ARAT5_MODEL_DIR = "Models/AraT5_Summarization"
```

### 4. Run the End-to-End System

![End-to-End Result](End_To_End.jpeg)
