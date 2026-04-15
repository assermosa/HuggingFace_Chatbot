# 🤗 HuggingFace Docs RAG Assistant

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.8+-blue?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/LangChain-RAG-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white"/>
  <img src="https://img.shields.io/badge/LLaMA_3.2-1B_Instruct-blueviolet?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/FAISS-Vector_Store-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/HuggingFace-Transformers-FFD21F?style=for-the-badge&logo=huggingface&logoColor=black"/>
</p>

<p align="center">
  A <strong>Retrieval-Augmented Generation (RAG)</strong> system that answers questions about HuggingFace documentation — powered by <strong>LLaMA 3.2-1B Instruct</strong>, <strong>FAISS</strong> vector search, and <strong>LangChain</strong>.
</p>

---

## 🧠 Overview

Instead of relying solely on an LLM's parametric knowledge, this project builds a RAG pipeline over the HuggingFace documentation corpus. Relevant documentation chunks are retrieved at query time and injected into the prompt — giving the model grounded, accurate answers.

```
User Question
      │
      ▼
 FAISS Retriever  ←── HuggingFace Docs (BAAI/bge-base-en-v1.5 embeddings)
      │
      ▼
  Top-5 Chunks
      │
      ▼
 LLaMA 3.2-1B Instruct  (via HuggingFace Transformers)
      │
      ▼
  Final Answer
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- GPU recommended (FAISS-GPU + LLaMA inference)
- HuggingFace account with access to [meta-llama/Llama-3.2-1B-Instruct](https://huggingface.co/meta-llama/Llama-3.2-1B-Instruct)

### Installation

```bash
# Clone the repository
git clone https://github.com/assermosa/huggingface-docs-rag.git
cd huggingface-docs-rag

# Create a virtual environment
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Requirements

```
langchain
langchain-community
sentence-transformers
faiss-gpu
transformers
torch
huggingface_hub
pandas
numpy
```

---

## 🔑 HuggingFace Authentication

This project uses **LLaMA 3.2**, which requires accepting the model license on HuggingFace.

1. Go to [meta-llama/Llama-3.2-1B-Instruct](https://huggingface.co/meta-llama/Llama-3.2-1B-Instruct) and accept the terms
2. Generate an access token at [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)
3. Login via CLI:

```bash
huggingface-cli login
```

> ⚠️ **Never hardcode your token in the code.** Use environment variables or the CLI login instead.

---

## 💡 Usage

### Run a query

```python
from pipeline import build_rag_chain

rag_chain = build_rag_chain()

question = "How can I deploy a model on HuggingFace?"
response = rag_chain.invoke({"context": "answer concisely", "question": question})
print(response["text"])
```

### Example Output

```
Question: How can I deploy a model?

Answer: You can deploy a model using the HuggingFace Inference API or 
Inference Endpoints. Simply push your model to the Hub with 
`model.push_to_hub("your-model-name")`, then enable the Inference API 
from your model page settings...
```

---

## 📁 Project Structure

```
huggingface-docs-rag/
├── notebook.ipynb          # Main Kaggle notebook
├── pipeline.py             # RAG pipeline (retriever + LLM chain)
├── embeddings.py           # FAISS vector store setup
├── data/
│   └── huggingface_doc.csv # HuggingFace documentation dataset
├── huggingfacemodel.pkl    # Saved LLM chain
├── requirements.txt
└── README.md
```

---

## 🛠️ Tech Stack

| Component | Technology |
|-----------|-----------|
| **LLM** | [LLaMA 3.2-1B Instruct](https://huggingface.co/meta-llama/Llama-3.2-1B-Instruct) |
| **Embeddings** | [BAAI/bge-base-en-v1.5](https://huggingface.co/BAAI/bge-base-en-v1.5) |
| **Vector Store** | FAISS (GPU) |
| **RAG Framework** | LangChain + LangChain-Community |
| **Inference** | HuggingFace Transformers Pipeline |
| **Dataset** | HuggingFace Documentation CSV |

---

## ⚙️ Configuration

| Parameter | Value |
|-----------|-------|
| Retriever | Similarity search, top-k = 5 |
| Temperature | 0.2 |
| Max New Tokens | 300 |
| Embedding Model | `BAAI/bge-base-en-v1.5` |
| LLM | `meta-llama/Llama-3.2-1B-Instruct` |

---

## 🤝 Contributing

Contributions are welcome! Feel free to open an issue or submit a pull request.

1. Fork the project
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

<p align="center">Made with ❤️ by <a href="https://github.com/assermosa">assermosa</a></p>
