<p align="center">
  <img src="assets/saudi-labor-law-header.png" alt="Saudi Labor Law Q&A Project Header" width="100%">
</p>

# Saudi Labor Law Q&A

### Transforming complex Saudi labor regulations into structured, searchable, and AI-ready legal knowledge.

The **Saudi Labor Law Q&A** project is a data science and Arabic legal NLP project that transforms Saudi Labor Law content into structured **Question–Answer (Q&A)** data. The project was designed to make legal information easier to access, analyze, search, and use in intelligent legal assistance systems.

Rather than relying on users to manually search through long legal documents written in specialized language, the project organizes legal content into a question–answer format and evaluates retrieval and large language model approaches for producing answers that remain grounded in official legal text.

---

## Project Motivation

Saudi Labor Law is essential for employees, employers, HR professionals, students, researchers, and legal practitioners. However, finding a specific rule can be difficult because the law is distributed across long legal articles, regulations, explanatory platforms, and FAQs.

This project addresses that challenge by creating a structured legal knowledge resource that can support:

- Faster access to labor-law information.
- Legal question answering.
- Thematic analysis of labor regulations.
- Intelligent legal search.
- Future AI legal assistants.
- Educational and research applications.
- HR and compliance-related use cases.

### Research Question

> **How can the articles of the Saudi Labor Law be transformed into a structured Question–Answer dataset that supports quick access to legal information and serves different beneficiary groups?**

---

## Project Pipeline

```text
Legal Data Sources
        |
        v
Data Collection & Web Scraping
        |
        v
Cleaning & Structuring
        |
        v
Exploratory Data Analysis
        |
        v
Q&A Dataset Construction
        |
        v
Baseline Retrieval (BM25)
        |
        v
Gemini Prompting Experiments
Zero-Shot | One-Shot | Three-Shot
        |
        v
Automated + Human Evaluation
        |
        v
Final Model Selection
```

---

## Data Sources

The project collected and studied Saudi labor-law information from reliable official and academic sources.

### 1. Bureau of Experts at the Council of Ministers — BOE

The official Saudi Labor Law text was used as the primary legal reference.

The cleaned BOE dataset contains structured fields such as:

- Section
- Chapter
- Article
- Legal text
- Status
- Source

### 2. Istitlaa Platform

Istitlaa provided draft amendments to the implementing regulations of Saudi Labor Law published for public consultation.

This source was useful for studying proposed legal changes and comparing current and proposed regulatory text.

### 3. Qiwa Platform

Qiwa provided structured labor-law information, practical explanations, and public-facing labor-law content.

### 4. Labor Law FAQ

Frequently asked questions were used to represent the type of practical questions users commonly ask about labor regulations.

### 5. King Saud University Digital Library

Academic studies and master's theses related to Saudi Labor Law were reviewed as an additional research source during data collection.

---

## Data Collection

Different collection techniques were used because the sources had different structures.

### Web Scraping

Dynamic web content was collected using tools such as:

- Selenium
- BeautifulSoup
- Requests
- WebDriver

The collected information was stored in structured CSV files while preserving source traceability.

### PDF-Based Sources

Legal and academic PDF documents were also reviewed as part of the project data collection process.

### Key Collection Challenges

The team handled several practical challenges, including:

- Dynamic JavaScript-based pages.
- Different data formats across sources.
- Web scraping restrictions.
- Arabic text encoding issues.
- Maintaining traceability between extracted content and its original source.

UTF-8-SIG encoding was used where needed to ensure Arabic text was displayed correctly.

---

## Data Cleaning & Preprocessing

The raw datasets were cleaned and standardized before analysis.

The preprocessing stage included:

- Removing unnecessary symbols and formatting.
- Standardizing Arabic text.
- Handling missing and duplicated values.
- Structuring legal articles and metadata.
- Preparing Q&A datasets.
- Preserving article and source references.
- Creating cleaned datasets for downstream analysis and modeling.

### Cleaned Data

```text
data cleaned/
├── boe_cleaned.csv
├── istitlaa_cleaned.csv
├── labor_law_faq_cleaned.csv
└── qiwa_data_cleaned.csv
```

The main cleaned datasets include **248 BOE legal records**, **16 Istitlaa records**, **16 FAQ records**, and **16 Qiwa content records**.

---

## Exploratory Data Analysis

EDA was conducted after cleaning to understand how Saudi labor-law information differs across official, explanatory, and public-facing sources.

The analysis included:

- Descriptive statistics.
- Text-length analysis.
- Word-frequency analysis.
- Arabic word clouds.
- Question and answer length comparison.
- TF-IDF analysis.
- Cross-source comparisons.
- Bias and metadata review.

### Example Insight

The FAQ analysis showed that questions tend to use more user-oriented and inquiry-based language, while answers contain more formal legal terminology.

This difference supports the value of a Q&A dataset: it creates a bridge between **how users naturally ask questions** and **how legal information is formally written**.

---

## Q&A Dataset Construction

The project transformed legal content into structured question–answer pairs that could be used for retrieval, evaluation, and future legal AI applications.

The Q&A datasets include:

```text
qa_datasets/
├── boe_qa.csv
├── istitlaa_qa.csv
├── labor_law_faq_cleaned.csv
└── qiwa_qa.csv
```

A **65-question ground-truth test set** was created for evaluation.

The test set was deliberately distributed across the different sources to reduce source bias:

| Source | Test Questions |
|---|---:|
| BOE Q&A | 23 |
| Qiwa Q&A | 21 |
| Istitlaa Q&A | 18 |
| Labor Law FAQ | 3 |
| **Total** | **65** |

The test questions cover a variety of labor-law topics, including working hours, leave, contracts, wages, penalties, disputes, training, occupational safety, and other employment-related regulations.

---

# Modeling & Evaluation

Because the project focuses on legal information retrieval and answer generation rather than traditional classification or regression, the modeling stage compared a retrieval baseline with multiple LLM prompting strategies.

## 1. BM25 Baseline

**BM25** was used as the baseline retrieval model.

The model:

1. Combines cleaned legal datasets into one searchable corpus.
2. Tokenizes Arabic legal text.
3. Compares a user's question with documents in the corpus.
4. Retrieves the **top three most relevant legal chunks**.

The baseline provides an important reference point because it retrieves existing legal content without generating new language.

---

## 2. Gemini 2.5 Pro

The project evaluated **Gemini 2.5 Pro** using three prompting strategies.

### Zero-Shot

The model answers the legal question without receiving an example first.

This experiment tests the model's general ability to interpret Saudi labor-law questions.

### One-Shot

The model receives one example Q&A pair before answering.

The example helps establish:

- Expected answer structure.
- Legal tone.
- Article citation style.
- Response format.

### Three-Shot

The model receives three example Q&A pairs before answering.

This provides stronger context for:

- Legal grounding.
- Consistent formatting.
- Accurate article references.
- Numerical and procedural rules.
- Deadlines, deductions, penalties, and calculations.

---

## Evaluation Methodology

The project combines **automated metrics** with **human evaluation**.

### Automated Evaluation

Generated answers were compared with official BOE legal text using:

- **Jaccard Overlap** — word-level similarity.
- **Answer-in-Article Coverage** — how much of the generated answer is supported by the legal article.
- **Article-in-Answer Coverage** — how much of the article appears in the answer.
- **Relevance thresholds** — measures whether generated answers reach minimum similarity or grounding levels.
- **Answer Length** — used to identify overly short or overly verbose responses.

### Human Evaluation

For each generative strategy, **50 Q&A pairs** were manually evaluated according to:

- **Correctness (0–2)**
- **Grammar (1–5)**
- **Style (0–2)**

Special attention was given to articles containing numerical and procedural requirements, where legal precision is especially important.

---

## Results

### Automated Results

| Model | Avg. Jaccard | Answer-in-Article Coverage | Main Observation |
|---|---:|---:|---|
| BM25 Baseline | 0.05 | 0.06 | Retrieves relevant raw text but has very low answer similarity |
| Zero-Shot | ~0.14 | ~0.32 | Moderate grounding but less stable |
| One-Shot | ~0.14 | ~0.32 | Better structure and writing style |
| **Three-Shot** | **0.13** | **0.39** | **Strongest legal grounding** |

The **Three-Shot** configuration achieved the highest answer-in-article coverage, meaning its generated answers were the most consistently grounded in the official legal content.

### Human Evaluation Summary

| Model | Correctness | Grammar | Style |
|---|---|---|---|
| Zero-Shot | Lowest | Medium | Weak |
| One-Shot | Good | Best | Strong |
| **Three-Shot** | **Highest** | Very Good | **Most Consistent** |

Three-Shot performed especially well on questions involving:

- Deadlines.
- Penalties.
- Numerical thresholds.
- Wage deductions and calculations.
- Procedural legal rules.

---

## Final Model Selection

### **Selected Approach: Three-Shot Prompting**

Three-Shot was selected as the strongest generative configuration because it provided the best overall balance of:

- Legal accuracy.
- Grounding in official law.
- Stability.
- Coverage.
- Article citation consistency.
- Writing quality.
- Handling of numerical legal rules.

The project concludes that this configuration is the most suitable among the evaluated approaches for supporting a future **Saudi Labor Law Q&A assistant**.

---

## Key Contributions

This project demonstrates an end-to-end data science workflow for Arabic legal information:

- Multi-source legal data collection.
- Arabic text preprocessing.
- Web scraping and structured data construction.
- Exploratory analysis of legal language.
- Q&A dataset generation.
- Ground-truth test-set construction.
- Information retrieval using BM25.
- LLM prompting experimentation.
- Automated legal grounding metrics.
- Human evaluation of generated answers.
- Model comparison and final selection.

---

## Repository Structure

```text
Data-Science-Project/
│
├── Notebooks/
│   ├── Phase-1/
│   │   └── IT362_project.ipynb
│   │
│   ├── Phase-2/
│   │   ├── BOE_Istitlaa_clean.ipynb
│   │   ├── EDA.ipynb
│   │   ├── EDA-edit.ipynb
│   │   ├── The_new_comparasion.ipynb
│   │   └── qiwa_cleaned.ipynb
│   │
│   └── Phase-3/
│       ├── Baseline.ipynb
│       ├── models_and_evaluation.ipynb
│       └── results/
│
├── data cleaned/
│   ├── boe_cleaned.csv
│   ├── istitlaa_cleaned.csv
│   ├── labor_law_faq_cleaned.csv
│   └── qiwa_data_cleaned.csv
│
├── qa_datasets/
│   ├── boe_qa.csv
│   ├── istitlaa_qa.csv
│   ├── qiwa_qa.csv
│   └── labor_law_faq_cleaned.csv
│
├── Models_Results/
├── raw data/
├── test_set_ground_truth.csv
├── Final report of saudi law.pdf
├── Final Logbook.pdf
└── README.md
```

---

## Technologies & Libraries

### Data Collection

- Selenium
- BeautifulSoup
- Requests
- WebDriver Manager

### Data Processing & Analysis

- Python
- Pandas
- NumPy
- Regular Expressions
- NLTK
- Scikit-learn

### Visualization

- Matplotlib
- Seaborn
- WordCloud
- Arabic Reshaper
- Python-Bidi

### Modeling

- BM25 / `rank-bm25`
- Gemini 2.5 Pro
- Zero-Shot Prompting
- One-Shot Prompting
- Three-Shot Prompting

### Development Environment

- Jupyter Notebook
- Google Colab

---

## Potential Applications

The structured dataset and evaluation workflow can support future applications such as:

- Saudi Labor Law AI assistants.
- Legal information retrieval systems.
- HR compliance tools.
- Intelligent legal search.
- Student study assistants.
- Legal research tools.
- Arabic legal NLP research.

---

## Team Members

- **Nora Saleh Alkhudair — نوره صالح الخضير**
- **مارية صالح النفيسة**
- **رغد نوري الرشيد**
- **لين خالد الدبيس**
- **ملاك طلال باسلوم**

---

## Academic Context

**King Saud University**  
College of Computer and Information Sciences  
Department of Information Technology  
**IT 362 — Principles of Data Science**

---

## Disclaimer

This project was developed for **academic and research purposes**.

The generated answers and experimental models are not a substitute for professional legal advice. Legal information should always be verified against the latest official Saudi regulations and authoritative government sources.
