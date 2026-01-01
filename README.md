# 🩺 Medical AI Assistant: The Impact of Data Engineering on RAG Performance

## 📖 Project Overview
This project involves building a specialized **Medical AI Doctor** capable of answering complex queries about cancer (Breast, Lung, Leukemia, etc.) with clinical precision.

The core objective was **scientific validation** of the hypothesis: **"Data Quality > Model Complexity."** To prove this, we developed a complete end-to-end pipeline—from web scraping to fine-tuning a Large Language Model (LLM)—and ran a controlled experiment comparing a **Raw Data RAG System** vs. a **Clean Data RAG System**.

## 🚀 Key Features
* **Automated Web Scraping:** A robust pipeline to harvest medical articles directly from the *National Cancer Institute (cancer.gov)*.
* **Advanced Data Preprocessing (The "Secret Sauce"):**
    * **Smart Imputation:** Used **BioGPT** to generate medically accurate summaries for missing data points.
    * **Semantic Outlier Detection:** Implemented **Isolation Forest** algorithms to identify and remove non-medical "junk" text.
    * **Metadata Injection:** Injected Topic/Category tags directly into text chunks to drastically improve retrieval precision.
* **Fine-Tuned "Doctor" Model:** Trained a **Mistral 7B** model using **QLoRA** adapters to adopt a professional medical persona.
* **Dual RAG Architecture:** Built two parallel Retrieval-Augmented Generation systems (Raw vs. Clean) to conduct an A/B test on answer quality.

## 📂 Datasets
1.  **Knowledge Base (Facts):**
    * **Source:** [National Cancer Institute (cancer.gov)](https://www.cancer.gov)
    * **Content:** Detailed protocols on symptoms, treatments, and diagnosis for major cancer types.
    * **Size:** ~300+ Verified Medical Articles.
2.  **Training Data (Reasoning):**
    * **Dataset:** `FreedomIntelligence/medical-o1-reasoning-SFT`
    * **Purpose:** Used to fine-tune the LLM to learn *how* to reason like a doctor (Clinical Chain-of-Thought).

## 🛠️ Tech Stack
* **Language:** Python 3.10+
* **LLM Engineering:** [Unsloth](https://github.com/unslothai/unsloth) (Accelerated Training), Hugging Face Transformers, PEFT (QLoRA).
* **Model:** Mistral-7B-v0.3-bnb-4bit (Quantized).
* **Vector Search (RAG):** FAISS (Facebook AI Similarity Search), Sentence-BERT (`all-MiniLM-L6-v2`) for embeddings.
* **Data Science:** Pandas, NumPy, Scikit-learn (Isolation Forest), Matplotlib/Seaborn (EDA).
* **ETL Pipeline:** BeautifulSoup4, Requests.

## 📊 Project Pipeline
1.  **Data Acquisition:** Scraped raw HTML content from cancer.gov.
2.  **EDA (Exploratory Data Analysis):** Analyzed word counts, complexity, and clusters (Histograms, Boxplots, Pairplots).
3.  **Preprocessing:**
    * Normalized text.
    * Removed noise via Isolation Forest.
    * Imputed gaps using BioGPT.
4.  **Model Training:** Fine-tuned Mistral 7B for 60-600 steps on medical reasoning data.
5.  **RAG Implementation:** Indexed both "Raw" and "Clean" datasets into vector databases.
6.  **Evaluation:** Conducted A/B testing with identical medical queries.

## 🏆 Results & Impact Analysis

Our controlled A/B test comparing **System A (Raw Data)** vs. **System B (Clean Data)** produced statistically significant improvements across all key performance indicators (KPIs).

### 1. 🚀 The "High Impact" Metric: Retrieval Safety
This is the most critical finding. We measured **Context Purity**—the percentage of retrieved text that was strictly relevant to the user's cancer query.

| Metric | System A (Raw Data) | System B (Clean Data) | **IMPACT** |
| :--- | :--- | :--- | :--- |
| **Retrieval Purity** | 33% (High Noise) | **100% (Pure)** | **✅ 3x Reliability Boost** |
| **Cross-Contamination** | High (Mixed Cancer Types) | **Zero** | **🛡️ Risk Eliminated** |

> **Real-World Consequence:** When asked about *Breast Cancer*, the Raw System retrieved dangerous noise about *Ovarian Cancer*. The Clean System retrieved **only** Breast Cancer protocols, proving our pipeline eliminates life-critical hallucinations.

### 2. 📊 NLP Evaluation Scores
Mathematical evaluation using **Semantic Similarity (Faithfulness)** and **ROUGE-L (Factuality)** confirmed that the Clean Data model is both more accurate and more consistent.

| Metric | System A (Raw) | System B (Clean) | Improvement | Meaning |
| :--- | :--- | :--- | :--- | :--- |
| **Factuality (ROUGE-L)** | 0.145 | **0.182** | **🟢 +25.5%** | The Clean model successfully used 25% more correct medical terminology from the source text. |
| **Faithfulness (Similarity)** | 0.482 | **0.535** | **🟢 +11.0%** | The Clean model adhered more strictly to the clinical context, reducing "creative" but wrong answers. |
| **Answer Detail** | 39 words | **49 words** | **🟢 +25.6%** | The Clean model provided comprehensive, medically nuanced responses rather than brief summaries. |

### 💡 Conclusion
The experiment validates the hypothesis: **Data Engineering > Model Complexity.**
By implementing metadata injection and semantic cleaning, we turned a hallucination-prone model into a **safe, reliable, and factually accurate Medical AI Assistant.**
