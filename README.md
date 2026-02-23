
#  RAG vs Non-RAG Question Answering System

### (FLAN-T5 + FAISS + MiniLM Embeddings)

This project compares two approaches for answering Machine Learning questions:

1.  **Non-RAG (Pure LLM)**
2.  **RAG (Retrieval-Augmented Generation)**

The system answers questions from *The Hundred-Page Machine Learning Book* using both methods and demonstrates how retrieval improves factual accuracy.

---

## Objective

Large Language Models (LLMs) can:

*  Hallucinate
*  Forget exact book details
*  Provide generic answers

RAG solves this by:

* Retrieving relevant content from a document
* Supplying it as context to the LLM
* Generating grounded answers

This project demonstrates that difference clearly.

---

# 1️⃣ Non-RAG System

## 🔹 How It Works

* Loads `google/flan-t5-base`
* Takes a user question
* Directly generates an answer
* No external knowledge retrieval

### Model Used

* `google/flan-t5-base`

### Prompt Style

* 3–5 sentence explanation
* Simple language
* Teaching style

### Pipeline

```text
User Question → FLAN-T5 → Generated Answer
```

### ✅ Advantages

* Simple
* Fast
* No document processing required

### Limitations

* May hallucinate
* Not grounded in specific textbook content

---

# 2️⃣ RAG System (Retrieval-Augmented Generation)

This system enhances the LLM using document retrieval.

---

## 🔹 Architecture Overview

```text
PDF → Chunking → Embeddings → FAISS Index
                                ↓
User Question → Embed → Retrieve Top Chunks
                                ↓
             Context + Question → FLAN-T5 → Final Answer
```

---

## Technologies Used

| Component       | Model / Tool          |
| --------------- | --------------------- |
| Generator       | `google/flan-t5-base` |
| Embeddings      | `all-MiniLM-L6-v2`    |
| Vector Database | FAISS                 |
| PDF Reader      | PyPDF                 |
| Framework       | Transformers          |

---

## 🔹 Step-by-Step RAG Pipeline

### 1️⃣ Load PDF

Reads:

```
2019BurkovTheHundred-pageMachineLearning.pdf
```

Extracts all text.

---

### 2️⃣ Text Chunking

Splits book into small chunks (220 words each).

Why smaller chunks?

* Better retrieval precision
* Less topic mixing
* More accurate answers

---

### 3️⃣ Create Embeddings

Uses:

```
all-MiniLM-L6-v2
```

Converts text chunks into vector embeddings.

---

### 4️⃣ Store in FAISS

* Builds a FAISS index
* Enables fast similarity search

---

### 5️⃣ Question Answering

When user asks:

1. Convert question to embedding
2. Retrieve top 3 relevant chunks
3. Combine into context
4. Send to FLAN-T5
5. Generate final answer

---

## Non-RAG vs RAG Comparison

| Feature            | Non-RAG | RAG            |
| ------------------ | ------- | -------------- |
| Uses Book Content  | ❌ No    | ✅ Yes          |
| Hallucination Risk | High    | Low            |
| Factual Accuracy   | Medium  | High           |
| Setup Complexity   | Low     | Medium         |
| Memory Efficient   | Yes     | Requires FAISS |

---

## Installation

```bash
pip install transformers
pip install sentence-transformers
pip install faiss-cpu
pip install pypdf
```

---

## ▶️ How to Run

### 🔹 Run Non-RAG System

```bash
python non_rag.py
```

Ask questions like:

```
What is overfitting?
Explain gradient descent.
```

Type `exit` to stop.

---

### 🔹 Run RAG System

Make sure the PDF file is in the same folder.

```bash
python rag_system.py
```

Ask:

```
What is bias-variance tradeoff?
Explain regularization.
```

Type `exit` to stop.

---

## Project Structure

```
RAG-vs-NonRAG/
│
├── non_rag.py
├── rag_system.py
├── 2019BurkovTheHundred-pageMachineLearning.pdf
└── README.md
```

---

## Educational Value

This project demonstrates:

* How LLMs work without retrieval
* How RAG improves factual grounding
* Vector databases (FAISS)
* Embedding models
* Prompt engineering
* Context window management

Perfect for:

* NLP coursework
* Mini RAG system demonstration
* LLM research experiments
* AI viva preparation
* Resume projects

---

## Key Concepts Explained

* Large Language Models (LLMs)
* Prompt Engineering
* Embeddings
* Vector Similarity Search
* Retrieval-Augmented Generation
* Context Injection
* Hallucination Reduction

---

## Possible Improvements

* Add Streamlit UI
* Add memory/chat history
* Use GPU for faster inference
* Replace FAISS with Pinecone/Weaviate
* Use larger LLM (FLAN-T5-Large)
* Add evaluation metrics (Faithfulness, Context Precision)
