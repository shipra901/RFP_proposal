# 🤖 AI-Powered RFP Proposal Generator

This project automates the generation of professional proposals in response to RFP (Request for Proposal) documents. It uses a local Large Language Model (LLM) like **Mistral 7B** through **LM Studio**, combines document parsing, semantic search with FAISS, and generates `.docx` proposal files via a user-friendly **Streamlit web app**.

---

## ✅ Features

- Upload RFP documents (**PDF**, **DOCX**, or **TXT**)  
  _OR_ manually input RFP details in the provided fields  
- Automatically extract and structure key information such as:
  - **Title**
  - **Scope of Work**
  - **Deadline**
  - **Contact Info**
  - **Budget**
- Retrieve similar examples from past proposals using **FAISS + RAG**
- Generate tailored proposals using a **local LLM (Mistral via LM Studio API)**
- Download the result as:
  - A structured **`.json` file**  
  - A complete proposal in **`.docx` format**
- Editable proposal generation with **chat-style prompt tuning** *(coming soon!)*

---

## 🗂 Project Structure

```bash
.
├── app.py                       # Streamlit frontend app
├── build_proposal_vector_store.py  # Builds FAISS index from training proposals
├── docx_writer.py              # Converts proposal sections to .docx format
├── extract_rfp_fields.py       # Extracts fields from uploaded RFPs
├── llm.py                      # Calls local Mistral LLM via API
├── rag_helper.py               # Retrieves similar past examples using FAISS
├── requirements.txt            # Python dependencies
├── data/
│   ├── rfp_data_raw.json       # Raw extracted RFP JSON (from dataset or file)
│   ├── faiss_index/            # FAISS vector store directory
│   └── proposal_output/        # Final generated `.docx` proposals
```

---

## 🔹 Phase 1: Data Preparation & Understanding

1. **Initial Dataset Collection**
   - Collected RFPs from sources like **CommBuys**.
   - Downloaded them as PDFs using `download_pdf.py`.
   - Converted them into `.txt` format for parsing and analysis.

2. **Structured JSON Extraction**
   - Extracted key fields such as:
     - Title
     - Description
     - Scope of Work
     - Deadline
     - Contact Info
     - Budget
   - Saved structured entries into a JSON file: `rfp_data.json`

---

## 🔹 Phase 2: Semantic Search (RAG)

3. **Embedding Past Proposals**
   - Used Sentence Transformers to embed past RFP JSON entries.
   - Built a FAISS index for efficient similarity search.

4. **Helper Function**
   - Created `rag_helper.py` to:
     - Load FAISS index
     - Retrieve top-2 similar examples for a new RFP input

---

## 🔹 Phase 3: LLM Integration

5. **LLM Setup**
   - Chose **Mistral 7B Instruct v0.3** for offline generation.
   - Used **LM Studio** with API server running on `http://localhost:1234`.

6. **Prompt Design**
   - Designed a detailed prompt structure to feed:
     - Extracted fields
     - Similar past examples
   - Output a fully formed proposal section-wise (executive summary, approach, timeline, etc.)

7. **LLM Call Layer**
   - Created `llm.py` to call the local Mistral model using `.env` variables.

---

## 📄 Phase 4: Document Generation

8. **DOCX Writer**
   - Created `docx_writer.py` to:
     - Format each section cleanly
     - Write into a well-structured `.docx` file with proper styling

---

## 🌐 Phase 5: Web UI (Streamlit)

9. **Frontend with Streamlit**
   - Developed `app.py` to:
     - Upload `.pdf`, `.docx`, or `.txt` RFP files
     - OR manually input field values
     - Display extracted fields
     - Trigger proposal generation
     - Enable download as `.docx` or `.json`

---

## ⚙️ Phase 6: Automation Utilities

10. **Script to Build FAISS Index**
    - `build_proposal_vector_store.py` embeds training data and builds vector store

11. **Extract RFP Fields**
    - `extract_rfp_fields.py` parses uploaded RFPs and extracts relevant fields

---

# 🧠 LM Studio Setup Guide for Offline RFP Proposal Generation

This guide explains how to set up **LM Studio** with the **Mistral 7B Instruct** model to enable offline LLM-powered proposal generation.

The local API will be used by this project to generate high-quality proposal content without relying on paid services like OpenAI.

---

## ✅ Step 1: Download and Install LM Studio

1. Visit the official LM Studio website:  
   👉 https://lmstudio.ai

2. Choose the appropriate version for your OS:
   - macOS (Intel or Apple Silicon)
   - Windows
   - Linux

3. Download and install LM Studio on your machine.

---

## ✅ Step 2: Download Mistral 7B Model

Inside LM Studio:

1. Go to the **“Models”** tab.
2. Search for:
   ```
   Mistral-7B-Instruct-v0.3
   ```
3. Download a GGUF quantized version such as:
   ```
   mistral-7b-instruct-v0.3.Q4_K_M.gguf
   ```

---

## ✅ Step 3: Run Local API Server

1. Go to the **“Developer”** tab in LM Studio.
2. Click on **“Start Server”**
3. The API server will be available at:
   ```
   http://localhost:1234/v1
   ```

---

## ✅ Step 4: Configure Environment Variables

Create a `.env` file in your project root with the following contents:

```env
OPENAI_API_KEY=lm-studio
OPENAI_API_BASE=http://localhost:1234/v1
```

These settings will be used by the project to connect with the local Mistral model.

---

You're now ready to run the proposal generator offline with full functionality!


## 📦 Installation
```bash
git clone https://github.com/shipra901/RFP_proposal
cd RFP_proposal
pip install -r requirements.txt

