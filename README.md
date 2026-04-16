# RAG-AI: Retrieval-Augmented Document Assistant

A full-stack Retrieval-Augmented Generation (RAG) system that allows users to upload documents, query them in natural language, and receive context-aware, source-grounded answers — including support for multi-document comparison and visual analysis of PDF pages.

---

## 🌐 Live Demo

This project is deployed on Render and can be tested here:

[RAG-AI Demo](https://rag-ai-qfhq.onrender.com/)

### Demo Credentials
A standard user demo account is available for exploring the application:

```text
username: demo
password: demo
```

The demo includes a small preloaded database of documents that can be queried through the RAG interface.

> For safety, administrative credentials are not shared publicly.

## 🚀 Features

- 📄 **Document Upload & Indexing**
  - Upload PDFs (and other formats)
  - Automatic text extraction + chunking
  - Stored in Chroma vector database

- 🧠 **Context-Aware Question Answering**
  - Uses embeddings + semantic search
  - Retrieves relevant document sections
  - Answers grounded strictly in source content

- 🔍 **Smart File Routing**
  - LLM determines which document(s) to query
  - Supports:
    - Single file queries
    - Multi-file comparison
    - "All files" queries

- 🧩 **Session Memory**
  - Tracks recent conversation context
  - Maintains continuity across queries

- 🖼️ **Vision-Based PDF Analysis**
  - Automatically renders PDF pages as images
  - Uses image + text together for better answers
  - Handles scanned/image-only documents

- 🛠️ **Admin Panel**
  - User management (create/delete/promote)
  - Database management (create/delete/switch)
  - File deletion & system reset
  - Chat log inspection

- 🔐 **Authentication System**
  - Role-based access (admin vs user)
  - Secure password hashing

---

## 🏗️ Architecture

RAG-AI uses a layered architecture that separates web routing, business logic, retrieval, and persistence.

```text
Frontend (Jinja HTML / JS / CSS)
        ↓
FastAPI Application
        ↓
Routes Layer
  - ask_question
  - upload
  - admin
  - auth
        ↓
Services Layer
  - LLM service
  - Chroma service
  - file processing
  - session memory
        ↓
Persistence Layer
  - ChromaDB for vector retrieval
  - SQLite for users, chat history, and session state
```

### End-to-End Request Flow

```text
User Query
   ↓
FastAPI route receives request
   ↓
Session context is loaded from SQLite
   ↓
LLM determines relevant file(s)
   ↓
Relevant chunks retrieved from ChromaDB
   ↓
Optional PDF page rendering for image-based analysis
   ↓
LLM generates grounded response
   ↓
Response returned with source references
```

### End-to-End Upload Flow

```text
Document Upload
   ↓
File saved to uploads/
   ↓
Text extracted page by page
   ↓
Embeddings generated
   ↓
Chunks + metadata stored in ChromaDB
   ↓
Document becomes searchable
```

---

## 📂 Project Structure

```
app/
├── core/
│   ├── db.py
│   ├── deps.py
│   ├── security.py
│   └── settings.py
│
├── routes/
│   ├── ask_question.py
│   ├── upload.py
│   ├── admin.py
│   ├── auth.py
│   └── ...
│
├── services/
│   ├── chroma_service.py
│   ├── files_service.py
│   ├── llm_service.py
│   ├── llm_provider.py
│
├── templates/
│   ├── index.html
│   └── admin.html
│
├── static/
│   ├── script.js
│   └── style.css
│
├── memory.py
├── models.py
└── main.py

databases/        # Chroma DB storage
uploads/          # Uploaded files
```

---

## ⚙️ How It Works

### 1. Upload
- File is saved locally
- Text is extracted and chunked
- Embeddings are generated
- Stored in Chroma DB

---

### 2. Query
- User submits a natural language question
- System:
  1. Determines relevant file(s) using LLM
  2. Retrieves relevant chunks from Chroma
  3. Builds structured context
  4. Sends to LLM for final answer

---

### 3. Memory
- Stores recent conversation turns
- Maintains session continuity across queries

---

### 4. Vision (Advanced)
- If a page is referenced:
  - Converts PDF page → images
  - Sends images to LLM
  - Enables diagram/schematic understanding

---

## 🔐 Admin Capabilities

- Create / delete users
- Promote users to admin
- Manage document databases
- Reset vector database
- View and clear chat logs

---

## 🧪 Example Use Cases

- 📘 Ask questions about technical PDFs
- 📊 Compare multiple documents
- 🧾 Extract structured data from reports
- 🔧 Analyze schematics and diagrams
- 🗂️ Manage internal document knowledge bases

---

## 🛠️ Tech Stack

- **Backend:** FastAPI
- **Vector DB:** ChromaDB
- **LLM:** OpenAI (GPT + embeddings)
- **Database:** SQLite (SQLAlchemy)
- **Frontend:** HTML + JS (Jinja templates)
- **PDF Processing:** Custom extraction + rendering

---

## 🚀 Running the Project

### Install dependencies
```bash
pip install -r requirements.txt
```

### Start the server
```bash
uvicorn app.main:app --reload
```

### Access the app
Open your browser and go to:

```
http://127.0.0.1:8000
```
