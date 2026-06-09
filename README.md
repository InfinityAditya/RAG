# 🤖 Simple Local RAG

> Build a **local retrieval-augmented generation (RAG)** pipeline from scratch — from PDF ingestion to "chat with PDF" features — using only open-source tools and your NVIDIA GPU.

![Simple Local RAG Workflow](images/simple-local-rag-workflow-flowchart.png)

---

## 🎯 What You'll Build

**NutriChat**: A RAG workflow that lets you query a **1,200-page Nutrition Textbook** (PDF) and get LLM-generated responses based on actual passages from the source material.

### 🔧 Pipeline Overview

1. **Document Ingestion** → PDFs (e.g., nutrition textbook) → chunks (10 sentences each)
2. **Embedding Creation** → Sentence Transformers → `torch.tensor` format
3. **Query Processing** → Embed query → similarity search → retrieve relevant passages
4. **Generation** → LLM (Gemma 7B) + context → answer generation
5. **Interaction** → Optional chat web app interface

**Hardware**: Runs locally on NVIDIA RTX 4090 (5GB+ VRAM)  
**Stack**: 100% open-source (PyTorch, Hugging Face, Sentence Transformers)

---

## 📚 Quick Links

| Resource | Description |
|----------|-------------|
| 📄 **PDF Source** | [Human Nutrition 2.0](https://pressbooks.oer.hawaii.edu/humannutrition2/) (1,200 pages) |
| 🚀 **Google Colab** | [Run in Colab](https://colab.research.google.com/github/mrdbourke/simple-local-rag/blob/main/00-simple-local-rag.ipynb) (NVIDIA GPU included) |
| 🎥 **Video Tutorial** | [Code Walkthrough on YouTube](https://youtu.be/qN_2fnOPY-M) (follow every line) |
| 💾 **GitHub Repo** | [mrdbourke/simple-local-rag](https://github.com/mrdbourke/simple-local-rag) |

---

## 🚀 Getting Started

Choose your setup:

### Option 1: Local NVIDIA GPU ⚡
**Requirements**: NVIDIA GPU with 5GB+ VRAM (tested on RTX 4090)

```bash
# Clone repository
git clone https://github.com/mrdbourke/simple-local-rag.git
cd simple-local-rag

# Create & activate environment
python -m venv venv
source venv/bin/activate  # Linux/macOS
# .\venv\Scripts\activate  # Windows

# Install requirements
pip install -r requirements.txt

# Launch notebook
jupyter notebook
# OR in VS Code: code .
``

---

## 📋 Prerequisites

- ✅ Comfortable writing **Python code**
- ✅ 1-2 **beginner ML/DL courses** completed
- ✅ Familiar with **PyTorch** → See [beginner PyTorch video](https://youtu.be/Z_ikDlimN6A?si=NIkrslkvHaNdlYgx)

---

## 🔧 Setup Notes

### ⚠️ Important Installation Tips

1. **PyTorch with CUDA** (required for faster attention):
   ```bash
   # Windows (CUDA 12.1)
   pip3 install -U torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
   ```
   → Official guide: [pytorch.org/get-started/locally](https://pytorch.org/get-started/locally/)

2. **Gemma LLM Authentication** (required for Gemma 7B models):
   - [Agree to terms](https://huggingface.co/google/gemma-7b-it) on Hugging Face
   - Authorize locally: `huggingface-cli login` or `huggingface_hub.login()`
   - Colab users: Add token to **Secrets** tab

3. **Flash Attention 2** (optional speedup, ~5min-3hr compile):
   ```bash
   pip install flash-attn
   ```
   → Windows users: See [GitHub issue](https://github.com/Dao-AILab/flash-attn/issues/595)  
   → *Note: `flash-attn` is commented out in `requirements.txt` due to compile time*

### 🛠️ Tested Environment
- **Python**: 3.11
- **OS**: Windows 11
- **GPU**: NVIDIA RTX 4090
- **CUDA**: 12.1

---

## 🤔 What is RAG?

**RAG** = **Retrieval-Augmented Generation**

Introduced in: [*Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks*](https://arxiv.org/abs/2005.11401)

### The 3-Step Process

| Step | What It Does | Example |
|------|--------------|---------|
| **🔍 Retrieval** | Search for relevant info from source given query | Get Wikipedia passages for a question |
| **✏️ Augmentation** | Use retrieved info to modify LLM input | Add context to prompt |
| **🎨 Generation** | LLM generates output from input | Generate answer passage |

---

## 🎯 Why Use RAG?

### Primary Benefits

| Benefit | Problem Solved | How RAG Helps |
|---------|----------------|---------------|
| **🚫 Prevents Hallucinations** | LLMs generate plausible-but-wrong facts | Provides **factual retrieved inputs** → more factual outputs |
| **📊 Works with Custom Data** | Base LLMs lack domain-specific knowledge | Injects **medical/company/textbook data** → customized outputs |

### Key Advantage Over Fine-Tuning
> RAG is **quicker to implement** than fine-tuning an LLM on specific data.

### From the Original Paper
> "RAG could be employed in a wide variety of scenarios with direct benefit to society, for example by endowing it with a medical index and asking it open-domain questions on that topic, or by helping people be more effective at their jobs."

---

## 💡 Real-World Applications

| Use Case | Description | Real Example |
|----------|-------------|--------------|
| **🏢 Customer Support Q&A** | "Chatbot for your documentation" | [Klarna saves $40M/year](https://www.klarna.com/international/press/klarna-ai-assistant-handles-two-thirds-of-customer-service-chats-in-its-first-month/) |
| **📨 Email Chain Analysis** | Extract structured claims from email threads | Insurance companies analyze agent-customer threads |
| **🏭 Internal Company Docs** | Index company info + answer questions + provide references | Large enterprises with scattered documentation |
| **📚 Textbook Q&A** | Get answers + references while studying | Your personal exam prep assistant |

**Common Pattern**: Retrieve resources → present understandably via LLM  
**LLM = Calculator for words**

---

## 🏠 Why Run Locally?

| Factor | Benefit |
|--------|---------|
| **🔒 Privacy** | No sensitive data sent to APIs |
| **⚡ Speed** | No API queues/downtime; runs when hardware runs |
| **💰 Cost** | Higher upfront cost, minimal ongoing costs |
| **🎮 Control** | Full control over models and data |

> **Performance Note**: LLM APIs may outperform local open-source models on general tasks, but **smaller focused models are increasingly outperforming larger ones**.

---

## 📖 Key Terms Cheat Sheet

| Term | Definition | Quick Example |
|------|------------|---------------|
| **Token** | Sub-word text piece | `"hello, world!"` → `["hello", ",", "world", "!"]`<br>1 token ≈ 4 chars ≈ 0.75 words |
| **Embedding** | Numerical representation of data | Sentence → vector with 768 values<br>Similar text → similar values |
| **Embedding Model** | Model outputting numerical representation | 384 tokens → vector size 768 |
| **Similarity Search** | Find vectors close in high-dimensional space | Similar text → high similarity score<br>Measures: dot product, cosine similarity |
| **LLM** | Model trained on text patterns | Generative LLM continues sequence:<br>`"hello, world!"` → `"we're building a RAG pipeline!"` |
| **Context Window** | Max tokens LLM accepts as input | GPT-4: 32k tokens (96 pages)<br>Gemma: 8,192 tokens (24 pages) |
| **Prompt** | Input to generative LLM | "[Prompt engineering](https://en.wikipedia.org/wiki/Prompt_engineering)" = structure input for ideal output |

---

## 🚧 Roadmap

### TODO
- [ ] Finish setup instructions
- [ ] Add intro to RAG info in README
- [ ] Add extensions to README

### ✅ Completed
- [x] Make header image of workflow
- [x] Record video of code writing/walkthrough


---

<div align="center">

**Made with LOVE By Aditya**

⭐ If this project helped you, consider giving it a star!

[![GitHub Stars](https://img.shields.io/github/stars/mrdbourke/simple-local-rag?style=social)](https://github.com/mrdbourke/simple-local-rag)
[![GitHub Issues](https://img.shields.io/github/issues/mrdbourke/simple-local-rag)](https://github.com/mrdbourke/simple-local-rag/issues)
[![License](https://img.shields.io/github/license/mrdbourke/simple-local-rag)](https://github.com/mrdbourke/simple-local-rag)

</div>
