# 📚 PDF Chat (RAG) – Multilingual AI Document Assistant

An AI-powered PDF question-answering application that allows users to upload multiple PDF documents, ask questions using text or voice, and receive streaming answers with page-level citations. It supports 12 languages, text-to-speech, speech-to-text, and Retrieval-Augmented Generation (RAG) to provide answers grounded in uploaded documents.

![App Screenshot](docs/screenshot.png)

## 🎯 Project Objectives

- Enable users to interact with PDF documents using natural language.
- Generate concise, context-aware answers from uploaded documents.
- Retrieve relevant information using semantic search.
- Provide source page citations for improved transparency.
- Support multilingual text and voice interactions.
- Simplify studying, document analysis, and information retrieval.

## ✨ Key Features

- **Multi-PDF Chat:** Upload and ask questions across multiple PDF documents.
- **RAG-Based Question Answering:** Retrieve relevant document passages before generating answers.
- **Page-Level Citations:** Display source references such as `p.3` with access to the supporting passage.
- **Voice Input:** Ask questions using microphone recording and Gemini speech-to-text.
- **Text-to-Speech:** Listen to generated answers using a Listen button or automatic playback.
- **12-Language Support:** English, Hindi, Malayalam, Tamil, Telugu, Kannada, Bengali, Marathi, Spanish, French, German, and Arabic.
- **Streaming Responses:** Display answers progressively, with Stop and Try Again controls.
- **Responsive Interface:** HTML, CSS, and JavaScript frontend with light and dark themes.
- **Indexing Progress:** Track document processing and indexing status.

## 📂 Dataset and Data Sources

This project does not require a fixed, pre-existing dataset. Users upload their own PDF documents, which serve as the knowledge source for question answering.

### Supported Document Sources

- Study notes and educational materials
- Textbooks and lecture notes
- Research papers
- Technical documentation
- Reports and reference documents

### Document Processing

1. Extract text from PDFs using `pypdf`.
2. Divide extracted text into overlapping chunks.
3. Generate semantic embeddings using Sentence Transformers.
4. Store and search embeddings using FAISS.
5. Retrieve the most relevant passages for each question.
6. Generate answers using Google Gemini with retrieved context.

**Note:** Text-based PDFs are supported by the described pipeline. Scanned PDFs may require OCR before processing.

## 🛠️ Tech Stack

| Component | Tools and Technologies |
|---|---|
| Programming Language | Python |
| Backend API | FastAPI |
| ASGI Server | Uvicorn |
| PDF Processing | pypdf |
| Text Embeddings | Sentence Transformers |
| Embedding Model | all-MiniLM-L6-v2 |
| Vector Search | FAISS |
| LLM and Speech-to-Text | Google Gemini API (`google-genai`) |
| Text-to-Speech | gTTS |
| Frontend | HTML, CSS, JavaScript |
| Configuration | Environment variables / `.env` |
| Version Control | Git, GitHub |

## 🔄 System Workflow

```text
User Uploads PDF Documents
          |
          v
PDF Text Extraction (pypdf)
          |
          v
Text Chunking with Overlap
          |
          v
Embedding Generation (MiniLM)
          |
          v
FAISS Vector Index
          |
          v
User Question (Text or Voice)
          |
          v
Question Translation (if required)
          |
          v
Semantic Search and Top-K Retrieval
          |
          v
Google Gemini Generates an Answer
          |
          v
Streaming Response + Page Citations
          |
          v
Optional Text-to-Speech Output
```

## 🧠 How RAG Works

Retrieval-Augmented Generation combines information retrieval with language generation.

1. **Retrieval:** The user's question is converted into an embedding and compared with indexed PDF chunks.
2. **Context Selection:** The most relevant chunks are retrieved from FAISS.
3. **Answer Generation:** Gemini receives the retrieved context and generates a response based on the available document information.
4. **Citation:** The application associates answers with source-page references.
5. **Response Delivery:** The answer is streamed to the interface and can optionally be read aloud.

This approach helps ground responses in uploaded documents, although it cannot guarantee that every answer is completely accurate.

## 📊 Data Processing and Visualization

This is primarily a document-processing and conversational AI project rather than a traditional data analytics project. Its main outputs are retrieved passages, generated answers, citations, and document-indexing progress.

Useful visualizations and monitoring components include:

- **Document Processing Status:** Displays the progress of PDF indexing.
- **PDF Library Overview:** Shows the documents indexed in the current session.
- **Source Citation View:** Displays the page and supporting passage used for an answer.
- **Retrieval Results:** Can be extended to display the most relevant chunks and their similarity scores.
- **Response Monitoring:** Can be extended to measure response time, retrieval latency, and answer-generation duration.

The retrieval scores, performance metrics, and charts listed as possible extensions should only be included as implemented features if they are actually present in the code.

## 🌐 Supported Languages

The application supports the following answer languages:

1. English
2. Hindi
3. Malayalam
4. Tamil
5. Telugu
6. Kannada
7. Bengali
8. Marathi
9. Spanish
10. French
11. German
12. Arabic

The embedding model `all-MiniLM-L6-v2` is English-oriented. Non-English questions are translated into English for retrieval according to the described workflow. Translation and answer quality may vary by language.

## 📁 Project Structure

```text
PDF-Chat-RAG/
│
├── server.py              # FastAPI API and frontend serving
├── rag.py                 # PDF extraction, chunking, embeddings,
│                          # FAISS retrieval and Gemini integration
├── requirements.txt       # Python dependencies
├── .env.example           # Environment variable template
│
├── frontend/
│   ├── index.html         # User interface
│   ├── style.css          # Styling and responsive design
│   └── app.js             # Frontend interactions
│
├── docs/
│   └── screenshot.png     # Application screenshot
│
└── README.md
```

## ⚙️ Installation and Setup

### Prerequisites

- Python 3.11 or 3.12 recommended
- Google AI Studio API key
- Internet connection
- Git installed on your system

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
```

Replace the placeholders with your actual GitHub username and repository name.

### 2. Create a Virtual Environment

**Windows:**

```bash
python -m venv venv
venv\Scripts\activate
```

**macOS / Linux:**

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure the API Key

Create a `.env` file in the project root:

```env
GOOGLE_API_KEY=your_google_api_key
GEMINI_MODEL=gemini-2.5-flash
```

Use a currently available Gemini model supported by your API account. Never commit your actual API key to GitHub.

Alternatively, use the application's Settings panel if API-key entry is implemented in your version.

### 5. Run the Application

```bash
python server.py
```

Open the application in your browser:

```text
http://localhost:8000
```

## 💬 How to Use

1. Open the application in your browser.
2. Upload one or more PDF documents.
3. Wait until document indexing finishes.
4. Enter a question in the chat interface or use the microphone.
5. Read the generated answer and check its page citations.
6. Click a citation to inspect the supporting passage.
7. Use the Listen button to hear the answer.
8. Select your preferred response language.

## 🔌 API Endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/languages` | Retrieve supported languages |
| GET | `/api/library` | List PDFs indexed in the current session |
| POST | `/api/upload` | Upload a PDF document |
| GET | `/api/jobs/{id}` | Check indexing progress |
| POST | `/api/chat` | Generate a streamed answer |
| POST | `/api/transcribe` | Convert voice input to text |
| POST | `/api/tts` | Convert answer text to speech |
| POST | `/api/reset` | Clear the session's PDF library |

The chat endpoint streams responses in NDJSON format, according to the described implementation.

Requests use browser-session identification through the `X-Session` header, and the API key can be supplied through the `X-API-Key` header.

## 🔒 Privacy and Limitations

- Uploaded PDFs are maintained in server memory for each browser session; restarting the server clears the session library.
- The embedding model is English-oriented, so non-English questions are translated for retrieval.
- Gemini receives retrieved PDF passages and questions to generate answers.
- Text submitted for speech synthesis is sent to the configured speech service.
- Voice output requires an internet connection.
- Scanned PDFs require OCR before text extraction.
- Answers can contain errors or omit relevant information; verify important information against the cited source pages.
- API keys must be kept private and excluded from version control.

## 🚀 Future Improvements

- OCR support for scanned PDFs
- Persistent document indexing using FAISS or ChromaDB
- Multilingual embedding models
- Improved citation verification
- Document summarization and topic extraction
- Retrieval and response-performance analytics
- User authentication and document management
- Docker deployment and cloud hosting

## 📌 Conclusion

PDF Chat (RAG) demonstrates how Retrieval-Augmented Generation, semantic search, large language models, and speech technologies can be combined to create an interactive document assistant. By enabling multilingual text and voice queries with page-level citations, the application makes studying, researching, and retrieving information from PDF documents more convenient.

---

**Developed using Python, FastAPI, FAISS, Sentence Transformers, Google Gemini, and JavaScript.**
