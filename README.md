# GenAI

🧠 Aspect-Based Sentiment Analysis with FLAN-T5, LLM, GPT

Developed for AT Kearney AI Use Case — Big Data Structuring & Sentiment Analysis
This repository provides an end-to-end pipeline for aspect-based sentiment analysis (ABSA). It was developed for an internal AI project at AT Kearney, focused on structuring and analyzing large-scale conversation data extracted from product and brand-related sources.

The pipeline leverages a fine-tuned FLAN-T5 model to detect brand-aspect pairs and classify sentiment polarity, supporting both production and evaluation (dev) workflows.

🔧 Project Purpose

Designed to support big data preprocessing and sentiment understanding across diverse datasets for market intelligence, this AI module was built as part of the AI strategy team at AT Kearney.
📁 File Structure (example)

File	Description
Aspect_Extraction_explicit.xlsx	Human-labeled data (brand, aspect, sentiment)
Aspect_Extraction_implicit.xlsx	Automatically extracted (inferred) data
<your_checkpoint_path>/	Fine-tuned FLAN-T5 model directory
[0] Finetuning (InstructABSA)_FINAL.ipynb	Main cleaned notebook with full pipeline
🔄 Pipeline Overview

0. Load and Preprocess Data
Load .xlsx files from <your_drive>
Flatten nested list columns (e.g., brand or aspect lists)
Clean and normalize missing values
1. Load Sentiment Model
Load FLAN-T5 model and tokenizer from local or remote checkpoint
Initialize device (GPU/CPU)
Set model to inference-ready mode (eval())
2. Sentiment Inference (Batch)
For each sentence and corresponding (brand, aspect), construct prompts:
In the sentence: "___", is the sentiment toward "___" positive, negative, or neutral?
Batch inference using T5-style generation
Output includes predicted label and placeholder confidence
3. Postprocessing and Export
Strip and clean predicted aspect fields
Merge with original metadata (media title, URL, etc.)
If in dev mode:
Merge with true labels (True_Aspect, True_Sentiment)
Include implicit flag for human review
Save to Excel: Aspect_Sentiment_Analysis_Result.xlsx
⚙️ Modes

Mode	Description
prod	Production: inference only, no label evaluation
dev	Evaluation: includes true label comparison, implicit flags
🧠 Model Information

Model: FLAN-T5 Large
Checkpoint: fine-tuned on ABSA-style instructions
Batching: supported (GPU preferred)
Max input length: 128 tokens
Max generation tokens: 3
📦 Output Format

Column	Description
Media Title	Source or headline
Conversation	Target sentence
Predicted_Aspect	Detected aspect
Predicted_Sentiment	Sentiment label (positive/neutral/negative)
Confidence	Currently placeholder 1.0
True_Aspect (dev)	Ground-truth aspect (optional)
True_Sentiment (dev)	Ground-truth sentiment (optional)
Implicit (dev)	Flag for human post-labeling
🚀 How to Use

# Load notebook in Google Colab or JupyterLab
# Make sure your model checkpoint and Excel files are in the right paths

# Set mode
MODE = "dev" or "prod"

# Run all cells from top to bottom

📌 Notes

The pipeline assumes that brand/aspect prediction is precomputed and available in Excel files.
You can integrate this with an upstream BrandAspectClassifier to extract (brand, aspect) from raw text.
This solution is modular and can be adapted to other LLMs such as LLaMA, GPT-4, etc.
Let me know if you’d like this as a README.md file or Notion-compatible format!
