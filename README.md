# clinical-reasoning-mistral-finetune
# 🩺 Medical AI Assistant: The Impact of Data Engineering on RAG Performance

## 📖 Project Overview
This project involves building a specialized **Medical AI Doctor** capable of answering complex queries about cancer (Breast, Lung, Leukemia, etc.) with clinical reasoning. 

The core objective was **scientific validation**: verifying the hypothesis that **"Data Quality > Model Complexity."** To prove this, we developed a complete end-to-end pipeline—from web scraping to fine-tuning a Large Language Model (LLM)—and ran a controlled experiment comparing a **Raw Data RAG System** vs. a **Clean Data RAG System**.

## 🚀 Key Features
* **Automated Web Scraping:** A robust pipeline to harvest medical articles directly from the *National Cancer Institute (cancer.gov)*.
* **Advanced Data Preprocessing:**
    * **Smart Imputation:** Used **BioGPT** to generate medical summaries for missing data points.
    * **Outlier Detection:** Implemented **Isolation Forest** algorithms to semantically identify and remove non-medical "junk" text.
    * **Feature Engineering:** Injected metadata (Topic/Category tags) directly into text chunks to improve retrieval accuracy.
* **Fine-Tuned "Doctor" Model:** Trained a **Mistral 7B** model using QLoRA adapters to adopt a professional medical persona.
* **Dual RAG Architecture:** Built two parallel Retrieval-Augmented Generation systems (Raw vs. Clean) to conduct an A/B test on answer quality.

## 📂 Datasets Used
1.  **Knowledge Base (Facts):** * Source: [National Cancer Institute (cancer.gov)](https://www.cancer.gov)
    * Content: Detailed information on symptoms, treatments, and diagnosis for various cancer types.
    * Size: ~300+ Medical Articles.
2.  **Training Data (Reasoning):**
    * Dataset: `FreedomIntelligence/medical-o1-reasoning-SFT`
    * Purpose: Used to fine-tune the LLM to learn *how* to think and reason like a doctor (clinical chain-of-thought).

## 🛠️ Technologies & Tools
* **Language:** Python 3.10+
* **LLM Frameworks:** [Unsloth](https://github.com/unslothai/unsloth) (for 2x faster training), Hugging Face Transformers, PEFT (LoRA).
* **Model:** Mistral-7B-v0.3-bnb-4bit (Quantized).
* **Vector Search (RAG):** FAISS (Facebook AI Similarity Search), Sentence-BERT (`all-MiniLM-L6-v2`) for embeddings.
* **Data Science Stack:** Pandas, NumPy, Scikit-learn (Isolation Forest), Matplotlib/Seaborn (EDA).
* **Web Scraping:** BeautifulSoup4, Requests.

## 📊 Project Pipeline
1.  **Data Acquisition:** Scraped raw HTML content from cancer.gov.
2.  **EDA (Exploratory Data Analysis):** Analyzed word counts, complexity, and clusters (Histograms, Boxplots, Pairplots).
3.  **Preprocessing (The "Secret Sauce"):** * Normalized text.
    * Removed noise via Isolation Forest.
    * Imputed gaps using BioGPT.
4.  **Model Training:** Fine-tuned Mistral 7B for 60-600 steps on medical reasoning data.
5.  **RAG Implementation:** indexed both "Raw" and "Clean" datasets into vector databases.
6.  **Evaluation:** Asked identical medical questions to both systems to compare performance.

## 🏆 Results & Conclusion
The project successfully demonstrated the **"Garbage In, Garbage Out"** principle in AI.

| Metric | System A (Raw Data) | System B (Clean Data) |
| :--- | :--- | :--- |
| **Retrieval Quality** | Retrievals often included irrelevant headers, footers, or wrong cancer types. | Retrieved precise, category-specific paragraphs (e.g., "Treatment" vs. "Symptoms"). |
| **Answer Quality** | Hallucinated details or gave generic info. | Provided clinically accurate, structured, and professional answers. |
| **Factuality (ROUGE)** | Lower score (High noise). | **Higher score** (High factual overlap). |
| **Faithfulness** | Low semantic similarity to context. | **High semantic similarity** to context. |

**Conclusion:** Advanced preprocessing—specifically **Metadata Injection** and **Semantic Outlier Removal**—significantly outperforms raw data pipelines, proving that data engineering is as critical as model architecture in Medical AI.

---
*Created by [Your Name/Group Name]*
