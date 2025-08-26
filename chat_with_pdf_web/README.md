# 📘 Multi-Source AI Agent with PDF + Web Integration  

## 🔑 Keywords  
- **LangChain**  
- **PDF QA**  
- **FAISS Vector Store**  
- **Embeddings**  
- **Retrieval-Augmented Generation (RAG)**  
- **Multi-Tools Agent**  
- **Business / Finance / HR Expert**  
- **Wikipedia Integration**  

## 📖 Overview  
This project implements an **AI-powered agent** using **LangChain** that can answer questions from multiple knowledge sources:  

1. **Company PDFs** (processed with `PyPDFLoader`, chunked, and embedded into a **FAISS vector database**).  
2. **External tools** such as **Wikipedia**.  
3. **Expert agents** for **Business**, **Finance**, and **HR** insights.  

The system combines **document retrieval (RAG)** with **LLM reasoning** to provide accurate and contextual answers.  

## ⚙️ Workflow  
1. **Load PDFs** → Parse and preprocess company documents.  
2. **Split & Embed** → Use `RecursiveCharacterTextSplitter` + `OpenAIEmbeddings`.  
3. **Vector Store** → Build FAISS for semantic search.  
4. **Retriever** → Query documents with semantic similarity.  
5. **QA Chain** → Create `RetrievalQA` for PDF-based question answering.  
6. **Tools** → Define domain-specific tools:  
   - 📝 **EntreprisePDF** → Answers from company PDFs.  
   - 💼 **BusinessExpert** → Strategy & management insights.  
   - 📊 **FinanceExpert** → Accounting & corporate finance insights.  
   - 👥 **HRExpert** → HR & employee management insights.  
   - 🌍 **Wikipedia** → External knowledge search.  
7. **Agent** → Multi-tool agent (`ZERO_SHOT_REACT_DESCRIPTION`) selects the right tool for each query.  

## 🚀 Example Queries  
- From PDF:  
  - `Quel est le numéro de téléphone de l'entreprise RHODES?`  
  - `Quelle est la capitale actions émis de l'entreprise EIREAN?`  
- From Web:  
  - `Who is Mr Beast?`  
  - `Give me some informations about Microsoft`  

## ✅ Features  
- 📂 Automatic **PDF ingestion & search**  
- 🔎 **Semantic search** with FAISS  
- 🤖 **Hybrid agent** combining documents + experts + Wikipedia  
- 🔄 Extendable with new tools & domains  

---
