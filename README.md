# RAG-Instruct-Chatbot: An Advanced NLP Project for Question Answering

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1Xt273_sH-Epx2pI6ypQH66oqBkbZAcNg)

This repository presents an end-to-end Natural Language Processing (NLP) project focused on building and evaluating a robust Retrieval-Augmented Generation (RAG) system. The project leverages a synthetic Q&A dataset derived from Wikipedia to develop a powerful question-answering chatbot capable of retrieving relevant information and generating concise, accurate responses.

## ✨ Key Features

*   **Comprehensive Data Analysis:** In-depth exploration of the RAG-Instruct dataset, including length distributions, vocabulary statistics, PCA for dimensionality reduction, K-Means clustering, and Latent Dirichlet Allocation (LDA) for topic modeling.
*   **Hybrid Search Engine:** Implementation and evaluation of a multi-stage retrieval system combining:
    *   **Keyword Search:** BM25 and DirichletLM using PyTerrier.
    *   **Dense Retrieval:** Alibaba GTE and Sentence-Transformer models (MiniLM-L6) with FAISS and HNSWLIB for Approximate Nearest Neighbor (ANN) search.
    *   **Re-ranking:** LambdaMART for learning-to-rank, and Cross-Encoder (BGE-reranker) for fine-grained relevance scoring.
*   **Custom Word Embeddings:** Training and intrinsic/extrinsic evaluation of Word2Vec and FastText models on the project's corpus.
*   **Large Language Model (LLM) Fine-tuning:** Fine-tuning the `Meta-Llama-3.1-8B` model using Unsloth and LoRA adapters on the RAG-Instruct Q&A pairs for enhanced domain-specific generation.
*   **LLM-powered Chatbot:** Development of a conversational chatbot using LangChain, integrating the developed retrieval system with a Google Gemini 1.5 Flash LLM for response generation.
*   **Robust Evaluation Frameworks:**
    *   **Information Retrieval (IR) Metrics:** Precision@K, Average Precision (AP), and Mean Average Precision (MAP) for search engine performance.
    *   **Natural Language Generation (NLG) Metrics:** BLEU, ROUGE, METEOR, and Perplexity for evaluating the fine-tuned model's generation quality.
    *   **LLM-as-a-Judge:** A novel approach using DeepSeek LLM to qualitatively assess chatbot responses across various criteria (fluency, coherence, engagingness, etc.).
    *   **Heuristic-based Ground Truth:** An alternative method to identify relevant documents for evaluation, complementing LLM-judged ground truth.

## 🛠️ Technical Stack

*   **Programming Language:** Python 3.x
*   **Machine Learning/NLP Frameworks:**
    *   `transformers`, `sentence-transformers`, `unsloth` (for LLM operations)
    *   `gensim` (Word2Vec, FastText, LDA)
    *   `pyterrier` (Information Retrieval)
    *   `faiss-gpu`, `hnswlib` (Vector Databases / ANN Search)
    *   `scikit-learn` (Clustering, PCA, TF-IDF)
    *   `lightgbm` (LambdaMART reranker)
    *   `langchain`, `langchain-google-genai` (Chatbot orchestration)
*   **Data Handling:** `pandas`, `datasets` (Hugging Face Datasets)
*   **Visualization:** `matplotlib`, `seaborn`, `pyLDAvis`, `umap-learn`
*   **Core Libraries:** `nltk`, `spacy`, `numpy`, `tqdm`, `warnings`
*   **LLMs Used:**
    *   `Meta-Llama-3.1-8B` (Fine-tuned)
    *   `Gemini 1.5 Flash` (Chatbot core)
    *   `DeepSeek v3 Base` (LLM-as-a-Judge for chatbot evaluation)
    *   `Alibaba-NLP/gte-large-en-v1.5` (Dense Retrieval)
    *   `sentence-transformers/multi-qa-MiniLM-L6-cos-v1` (Dense Retrieval)
    *   `cross-encoder/ms-marco-MiniLM-L-6-v2` (Cross-encoder reranking)

## 📊 Dataset Overview

The project utilizes the [FreedomIntelligence/RAG-Instruct](https://huggingface.co/datasets/FreedomIntelligence/RAG-Instruct) dataset, comprising ~40,000 Q&A pairs synthesized from Wikipedia. Each entry includes:
*   `question`: Natural-language query generated by GPT-4o.
*   `answer`: One-paragraph response from GPT-4o referencing seed passages.
*   `documents`: Exactly 10 passages (seed D\* + 8–9 distractors D⁻), shuffled.


## 📈 Results and Insights

### Data Analysis
The dataset primarily consists of short questions (avg. 20 words) and longer answers (avg. 91 words). PCA and KMeans clustering identified a dominant cluster, suggesting the dataset covers a few core topics with variations. LDA revealed 15 distinct topics, with terms like "model", "language", "data", "system", and "protein" being prominent.

### Search Engine
The project implemented a hybrid retrieval strategy. Initial keyword search (BM25, DirichletLM) was combined with dense retrieval (Alibaba GTE, MiniLM-L6) using FAISS/HNSWLIB. A LambdaMART reranker significantly improved the ranking of relevant documents, demonstrating the power of supervised learning for information retrieval.

### Word Embeddings
Custom Word2Vec and FastText models were trained on the corpus. Both performed well on intrinsic (word analogy, similarity) and extrinsic (retrieval-at-k) benchmarks, confirming their utility for capturing semantic relationships specific to the dataset's domain. FastText showed better handling of OOV words.

### Retrieval-Augmented Generation (RAG)
The combined retrieval pipeline (dense + reranking) was effective in fetching relevant documents. The adaptive thresholding mechanism in the `combined_search` function dynamically selects the most relevant documents based on their fused scores, ensuring high-quality context for the LLM.

### LLM Fine-tuning
The `Meta-Llama-3.1-8B` model was successfully fine-tuned using Unsloth's LoRA implementation. The training loss curve showed convergence. Evaluation using standard NLG metrics (BLEU, ROUGE, METEOR, Perplexity) demonstrated a demonstrable improvement in the fine-tuned model's ability to generate relevant and coherent answers, closely aligning with the original dataset's answers.

### Chatbot and LLM-as-a-Judge Evaluation
The LangChain-powered chatbot, leveraging the hybrid retrieval system and Gemini 1.5 Flash, provided engaging and informative responses. A cutting-edge "LLM-as-a-Judge" framework (using DeepSeek LLM) was implemented for qualitative evaluation. This unbiased assessment revealed high scores across criteria like `fluency`, `making_sense`, `avoiding_repetition`, and `listening`, indicating a performant and human-like conversational agent.

## 📺 Video Demo

A video demonstration of the project and its functionalities is available here:
[Project Demo Video](https://polimi365-my.sharepoint.com/personal/10968409_polimi_it/_layouts/15/stream.aspx?id=%2Fpersonal%2F10968409%5Fpolimi%5Fit%2FDocuments%2Fnlp%2Dproj%2Dragnroll%201%2Emov&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZytMaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZytMaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0=&ga=1)

