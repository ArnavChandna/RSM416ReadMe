# RAG-Powered Job Application Helper Tool Prototype

## 📌 Overview

This prototype implements a **Retrieval-Augmented Generation (RAG)**
system using **LangChain**, **FastEmbed embeddings**, and a **local LLM
backend** (e.g., Qwen).
The system allows a user to:

1.  **Upload a resume**
2.  **Paste a job description**
3.  **Embed & chunk the resume**
4.  **Retrieve the most relevant resume segments**
5.  **Generate LLM-powered tailored outputs**, such as:
    -   Alignment summaries
    -   Customized bullet points
    -   Resume improvements
    -   Role-specific messaging

The notebook also includes **UI widgets** for interaction and a
**full-pipeline test**.

------------------------------------------------------------------------

## 📦 Features

-   Local LLM usage
-   FastEmbed vector embeddings
-   LangChain RAG pipeline
-   UI widgets for resume & job description
-   End-to-end system test

------------------------------------------------------------------------

## 🛠 Installation & Setup

### 1. Clone the project

``` bash
git clone <repo-url>
cd <repo-folder>
```

### 2. Install dependencies

``` python
%pip install langchain langchain-community fastembed ipywidgets
```

### 3. Enable widgets

``` bash
jupyter nbextension enable --py widgetsnbextension
```

### 4. Open the notebook

``` bash
jupyter notebook Group_Project_Prototype_v2.ipynb
```

------------------------------------------------------------------------

## ▶️ How to Run

1.  Run all cells\
2.  Upload resume\
3.  Paste job description\
4.  Generate tailored output\
5.  Optional: run the Full System Test

------------------------------------------------------------------------

## 📥 Expected Input

| Component                 | Description                                      |
| ------------------------- | ------------------------------------------------ |
| **Resume**                | PDF or text input loaded via widget or file path |
| **Job Description**       | Free-form text pasted into widget                |
| **User Query (optional)** | Prompts to the LLM chain for customized outputs  |

### Expected Input Formats:
```bash
Job Description:
We are seeking a Data Analyst with SQL, Python, and dashboarding experience...
```
```bash
chain.invoke({
    "resume": resume_text,
    "job_description": jd_text
})
```

------------------------------------------------------------------------

## 📤 Expected Output
The system generates structured text such as:
1. Extracted relevant resume content
2. Alignment analysis
3. Customized achievements or bullet points
4. A rewritten resume section tailored to the job
5. Summary of keyword matches

### Sample LLM Output
```bash
Top Resume Segments for this Job:
1. SQL-based data modeling experience...
2. Python automation project reducing processing time by 30%...

Tailored Bullet Points:
• Built dashboards in PowerBI to monitor KPIs...
```

------------------------------------------------------------------------

## 🔧 System Architecture Diagram

    User → Preprocessing → Embedding → Vector Store → Retriever → LLM Chain → Output
                     ┌──────────────────────────┐
                     │        User Inputs       │
                     │(Resume Upload + Job Desc │
                     └─────────────┬────────────┘
                                   │
                      ┌────────────▼─────────────┐
                      │ Document Preprocessing   │
                      │  - PDF/Text Loading      │
                      │  - Chunking (splitter)   │
                      └────────────┬─────────────┘
                                   │
                        ┌──────────▼───────────┐
                        │   Embedding Model    │
                        │ (FastEmbed vectors)  │
                        └──────────┬───────────┘
                                   │
                    ┌──────────────▼──────────────┐
                    │       Vector Store          │
                    │ (In-memory index for RAG)   │
                    └──────────────┬──────────────┘
                                   │
                      ┌────────────▼──────────────┐
                      │         Retriever         │
                      │ (Top-k relevant chunks)   │
                      └────────────┬──────────────┘
                                   │
                      ┌────────────▼──────────────┐
                      │        LLM Chain          │
                      │ (Local LLM, LangChain)    │
                      └────────────┬──────────────┘
                                   │
                      ┌────────────▼──────────────┐
                      │        Final Output       │
                      │ (Tailored writing, summary│
                      │    alignment analysis)    │
                      └───────────────────────────┘


------------------------------------------------------------------------

## ⚠️ Known Limitations

| Area                        | Limitation                                                        |
| --------------------------- | ----------------------------------------------------------------- |
| **Local Model Quality**     | Local LLM may underperform vs. API models (GPT-4, Claude)         |
| **Embedding Store**         | Currently in-memory; scales poorly with large resumes or datasets |
| **UI Widgets**              | Only works in Jupyter; no standalone app yet                      |
| **PDF Parsing**             | Formatting loss possible for complex resumes                      |
| **No persistent vector DB** | Index resets each run                                             |
| **No authentication layer** | Prototype not production-secure                                   |


------------------------------------------------------------------------

## 🛡 Risks & Mitigations

  | Risk                             | Impact                                   | Mitigation                                        |
| -------------------------------- | ---------------------------------------- | ------------------------------------------------- |
| **Model privacy leakage**        | Sensitive resume data processed by model | Uses **local LLM** by default; API usage optional |
| **Malformed job descriptions**   | Poor retrieval & generation              | Add validation & fallback prompts                 |
| **Incorrect embeddings**         | Poor relevance matching                  | Tune chunk size & embedding model                 |
| **Key exposure (if using API)**  | Security vulnerability                   | Use `.env` files; avoid hardcoding keys           |
| **User uploads arbitrary files** | Parsing errors or injection attacks      | Restrict file types & sanitize inputs             |

------------------------------------------------------------------------
