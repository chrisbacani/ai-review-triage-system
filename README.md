# AI Review Triage System

A production-oriented proof of concept demonstrating how high-volume guest reviews can be automatically triaged, structured, risk-scored, grounded in policy context, and transformed into brand-safe responses.

This system simulates a real-world customer support workflow using modular LLM orchestration and lightweight retrieval components.

---

## Overview

Customer review streams often contain:

- Mixed sentiment  
- Operational complaints  
- Policy-related questions  
- Escalation-worthy issues  

This notebook demonstrates a clean, end-to-end pipeline that:

1. Extracts structured signals from free-text reviews  
2. Applies hybrid risk detection (rule-based + model-based)  
3. Grounds responses in FAQ policy context via semantic retrieval  
4. Generates guardrailed, brand-aligned replies  
5. Executes deterministically from a single orchestrator function  

The emphasis is on **controlled decision flow**, not raw text generation.

---

## System Architecture

The pipeline consists of the following components:

### 1. Structured LLM Analysis
- Summary extraction  
- Sentiment classification  
- Numeric sentiment scoring  
- Model-based escalation detection  

### 2. Hybrid Risk Logic
Rule-based keyword triggers are combined with model reasoning to reduce false positives and improve escalation reliability.

### 3. Semantic FAQ Retrieval
- Sentence embeddings (MiniLM)  
- Cosine similarity matching  
- Threshold-based grounding  
- Prevents hallucinated policy responses  

### 4. Guardrailed Response Generation
Responses are drafted using:
- Park-specific context  
- Escalation awareness  
- Explicit constraints against liability admission  
- Controlled tone and formatting  

### 5. End-to-End Orchestration
A single `process_review()` function executes the full workflow and returns structured output including:

- Summary  
- Sentiment label  
- Sentiment score  
- Risk flag and reason  
- FAQ match (if applicable)  
- Final drafted response  

---

## Example Output

The system returns a structured object:

```json
{
  "summary": "...",
  "sentiment_label": "negative",
  "sentiment_score": -0.8,
  "risk_flag": false,
  "risk_reason": null,
  "faq_question": null,
  "faq_score": null,
  "response": "..."
}
```

## Setup & Reproducibility

To run this project locally:

1. Clone the repository and navigate into the project directory.
2. Create and activate a virtual environment.

### macOS / Linux

```bash
python -m venv venv
source venv/bin/activate
```

### Windows

```bash
python -m venv venv
venv\Scripts\activate
```

3. Install dependencies.

```bash
pip install -r requirements.txt
```

4. Create a `.env` file in the project root containing:

```env
OPENAI_API_KEY=your_generated_api_key
```

Generate an API key at https://platform.openai.com

Do not commit your `.env` file to version control.

5. Open the notebook, restart the kernel, and select “Restart & Run All.”

The notebook executes cleanly from top to bottom without manual cell reordering.

## Notes

- LLM calls are executed synchronously for clarity and reproducibility.
- The first embedding call may take slightly longer while the model initializes.
- The demo processes small batches to control API usage costs.
````
