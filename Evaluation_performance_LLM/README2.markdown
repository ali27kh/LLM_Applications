# 📊 LangChain: Evaluation Framework

## 🔑 Keywords
- **LangChain**
- **LLM Evaluation**
- **RetrievalQA**
- **CSVLoader**
- **DocArrayInMemorySearch**
- **QAGenerateChain**
- **QAEvalChain**
- **ROUGE / F1 / Semantic Similarity**
- **LLM-assisted Evaluation**

---

## 📖 Overview
This project demonstrates how to **evaluate a LangChain QA system** using multiple methods:

1. **Example Generation** (hard-coded + LLM-generated).
2. **Manual Evaluation** with debugging mode.
3. **LLM-Assisted Evaluation** with `QAEvalChain`.
4. **Extended Performance Metrics**:
   - ✅ Exact Match
   - ✅ F1 Score
   - ✅ ROUGE-L
   - ✅ Semantic Similarity (cosine similarity of embeddings)

The evaluation shows how **strict lexical metrics** (Exact Match, ROUGE) may underestimate performance, while **semantic similarity** better reflects LLM quality.

---

## ⚙️ Workflow

1. **Setup**
   - Load environment variables (`.env`) with `dotenv`.
   - Handle **model versioning** depending on the date (`gpt-3.5-turbo` vs `gpt-3.5-turbo-0301`).

2. **QA Application**
   - Load CSV file with `CSVLoader`.
   - Create vector index with `DocArrayInMemorySearch`.
   - Build `RetrievalQA` chain using **OpenAI Chat model**.

3. **Examples Creation**
   - Add **hardcoded QA pairs**.
   - Use `QAGenerateChain` to generate new examples from dataset.

4. **Evaluation**
   - **Manual Evaluation** with debug mode.
   - **LLM-Assisted Evaluation** using `QAEvalChain`.
   - **Custom Metrics**:
     - Exact Match
     - F1 Score
     - ROUGE-L
     - Semantic Similarity

---

## 🚀 Example Usage

### Hardcoded Examples
```python
examples = [
    {
        "query": "Do the Cozy Comfort Pullover Set have side pockets?",
        "answer": "Yes"
    },
    {
        "query": "What collection is the Ultra-Lofty 850 Stretch Down Hooded Jacket from?",
        "answer": "The DownTek collection"
    }
]
```

### ✅ LLM-Assisted Evaluation
```python
from langchain.evaluation.qa import QAEvalChain

llm = ChatOpenAI(temperature=0, model=llm_model)
eval_chain = QAEvalChain.from_llm(llm)

graded_outputs = eval_chain.evaluate(examples, predictions)
```

### ✅ Extended Metrics
```python
results = evaluate(examples, predictions)
print(results)
```

### 📊 Sample Results
- **Exact Match**: 0.14 → Only 14% of answers are strictly identical.
- **F1 Score**: 0.52
- **ROUGE-L**: 0.54
- **Semantic Similarity**: 0.91 ✅

#### 👉 Interpretation:
- LLM answers are semantically accurate but not always lexically identical.
- Semantic Similarity confirms strong performance despite low token-based scores.

---

## ✅ Features
- 📂 Easy CSV ingestion for QA tasks
- 🔎 Evaluation pipeline with manual + automated grading
- 📊 Rich metrics for text evaluation
- 🔄 Extendable to other datasets and evaluation strategies