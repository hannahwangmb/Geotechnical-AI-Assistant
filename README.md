# 📘 Project Summary Report: Geotechnical AI Assistant for Design Calculation Automation and Knowledge Sharing

## 🏗️ 1. Project Background and Objectives

With the rapid development of AI, the traditional geotechnical engineering workflow is evolving toward automation and intelligence. This project explores the application of large language models (LLMs) in geotechnical engineering, with dual focuses on academic research and industrial adoption.

The first phase of the project targets two key areas: **Design Calculation Automation** and **Knowledge Sharing**.

For the **Design Calculation Automation** part, we selected **CPT-based footing design** as the starting point to explore the potential of automating foundation design using LLMs. The ultimate goal is to develop an intelligent system, supervised by senior engineers, that can autonomously complete the entire design calculation process and generate result reports—integrating raw data upload, parameter extraction, execution of calculations, and report generation into a streamlined workflow.

### Project Deliverables

- 💡 Full-stack system integrating React frontend + Python backend + GPT engine  
- 🧠 Natural language interface for interactive foundation design  
- 📊 Automated file upload → data visualization → report generation  
- 🧾 Solid foundation for future extension to domain-specific knowledge bases and other design scenarios

### Engineering Value

- Significantly lowers the barrier for junior engineers to perform analysis  
- Saves time on repetitive manual operations  
- Improves standardization and traceability of results  
- Serves as technical validation for intelligent geotechnical consulting services

The system comprises two core modules:

- ⛏️ **LLM-based CPT-footing design automation module**  
  - Automatically reads CPT data files from a specified path and performs initial processing  
  - Executes bearing capacity and settlement analysis  
  - Outputs standardized charts and summarized data  
  - Communicates results and suggestions to engineers via natural language

- 📚 **Domain knowledge sharing module**  
  - Combines LLM capabilities with an internal knowledge base (via RAG architecture)  
  - Supports storing, retrieving, explaining, and updating geotechnical knowledge  
  - Enhances internal knowledge transfer and onboarding efficiency

---

## 🧠 2. Technical Implementation

### 2.1 Frontend (Next.js + TypeScript + React)

**Responsibilities:**

- User interface for natural language inputs, parameter entry, and file upload  
- Result display via chart and data links  
- GPT-based summarization and explanation  
- API communication with the backend

**Structure & Features:**

- Conversation thread list and history  
- Add/delete conversations, edit conversation names  
- Share and join threads via thread ID  

*Knowledge Sharing System:*
- File upload and delete  
- Uses OpenAI’s cloud-embedded vector DB (file type limitations apply):  
  [OpenAI Supported File Types](https://platform.openai.com/docs/assistants/tools/file-search#supported-files)

*Design Calculation System:*
- User input for design parameters (e.g., Load N, FS, allowable settlement, etc.)  
- Result visualizations:
  - Charts for bearing capacity, settlement vs. footing width  
  - File format for design input is regulated by Python scripts  

---

### 2.2 Backend API (Node.js)

- Receives uploaded files and parameters  
- Packs and sends requests to local Python scripts  
- Caches processing status and outputs (file paths, JSON summaries)

---

### 2.3 Data Processing Engine (Python)

**A. `cpt_reduce.py` – Data Preprocessing**


**B. `cpt_footing.py` – Foundation Design Calculations**


**C. `cpt_lcpc.py` – LCPC Analysis for Deep Foundations**


**📁 Code Structure:**

  ```
  /app/
    /api/ # Backend APIs 
      /assistants/ # GPT Assistant creation 
        /files/ # File management for GPT (upload, list, delete) 
        /threads/ # Thread management 
      /cpt_footing/ # Shallow foundation design (CPT-based) 
      /cpt_lcpc/ # Deep foundation design (LCPC method) 
      /reduce_cpt_data/ # CPT raw data preprocessing 
      /file-manage/ # Local file handling 
    /components/ # React UI components 
    /pages/ # Routing 
    /utils/ 
      /gpt_in/ # Input file path storage 
      /gpt_out/ # Output file path storage 
      /cpt_footing.py # Main design calculation script 
      /CPT_footing.xlsx # Template spreadsheet 
      /...
  ```

---

### 2.4 GPT Assistant (OpenAI Assistant API)

- Based on GPT-4o-mini  
- Functionalities:
  - Understands natural language design queries  
  - Uses function calling to trigger Python functions  
  - Interprets results and provides summaries  
  - Offers interaction suggestions  
  - Retrieves relevant content from uploaded files using built-in RAG system  

---

## 🔁 3. System Interaction Workflow

1. User uploads CPT Excel file and optionally provides parameters (e.g., Load N, FS, GWL)  
2. GPT Assistant identifies intent and checks if all required parameters are provided  
3. Calls Python scripts to:
   - Preprocess CPT data
   - Execute design calculations
   - Generate chart and summary files  
4. Returns:
   - `cpt_reduce_complete_results.json`
   - `*_combined_plot.png`
   - `reduced_*.xlsx`
5. GPT Assistant interprets the JSON results and recommends next steps:
   - Proceed with shallow foundation design  
   - Visualizes:
     - Bearing capacity vs. footing width
     - Settlement vs. width (with limit line)
6. GPT assesses whether design meets criteria or suggests deep foundation analysis

### Sequence Diagram:

  ```
  User ->> WebUI: Upload CPT file + input instructions 
  WebUI ->> API: Send file and parameters 
  API ->> GPT: Request function calling suggestion (if needed) 
  GPT -->> API: Return function and parameter recommendations 
  API ->> Python: Execute script with parameters 
  Python -->> API: Return JSON + charts + Excel 
  API -->> WebUI: Deliver result files to frontend 
  WebUI -->> User: Display charts and summaries 
  GPT -->> User: Output explanation and design recommendations
  ```

---


## ⚠️ 4. Error Handling & Logging

- File format validation and error notifications  
- Auto-revert on Excel read/write failure  
- Forced Excel instance shutdown to prevent memory leaks  

**Log Sample:**

```json
{
  "summary": {
    "total_files": ,
    "successful": ,
    "failed": 
  }
}
```

---

## 🧰 5. Future Development Suggestions

To improve system usability and extensibility, the following enhancements are proposed:

### 1. Local Deployment of Lightweight LLMs

Reduce dependency on OpenAI services by exploring open-source models like LLaMA or DeepSeek.
Establish private knowledge base + on-premise inference server to enhance data security and response speed.

#### Deployment Options:

- AllianceCan (RAS resources applied, AllianceCan should have emailed the response to Dr.Lin)
  - https://ccdb.alliancecan.ca
  - https://docs.alliancecan.ca/wiki/Technical_documentation
- Cloud Services (e.g., AWS)
- MacStudio

### 2. Expand Engineering Knowledge via RAG or Fine-tuning

- Import internal codes, historical design reports, standards
- Improve assistant's reasoning and recommendation abilities

### 3. Extend Design Capabilities to Other Scenarios

- Update complete calculation for deep foundation design (LCPC method)

### 4. Add User Management & Project Archiving

- Implement account system for saving user progress and projects
- Enable multi-user collaboration and version tracking
- Auto-generate project folder structure for audit readiness

### 5. Enhance Input Robustness & Output Reliability

- Handle unit conversion and missing fields gracefully
- Add boundary condition checks to avoid unreasonable design outputs
- **Provide source references for LLM answers when applicable**

### 6. Integrate Elsevier API for Data Extraction & Analysis

- Automate geotechnical parameter extraction from academic papers: https://www.nature.com/articles/s41467-024-45914-8#Tab1

- Follows pipeline:

  1. Sentence tokenization
  2. LLM-based filtering
  3. Single/multi-value structured extraction
  4. Accuracy verification
  5. Output cleaning with Pandas

Elsevier API Docs：https://dev.elsevier.com/sd_api_spec.html

### 7. Advanced Agent for In-depth Research

Explore academic topics with GPT agents: https://github.com/dzhng/deep-research

### 8. Additional Tools and References
- 🔍 [Deep Searcher (RAG pipeline)](https://github.com/zilliztech/deep-searcher)
- 🧠 [Dify](https://github.com/langgenius/dify)

---
