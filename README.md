Chatbot PDF Reader

A conversational RAG (Retrieval-Augmented Generation) chatbot built with Streamlit and LangChain. Upload one or more PDFs and ask questions about their content. The app keeps chat history per session, so follow-up questions are understood in context.

How it works
PDFs are uploaded and loaded with PyPDFLoader.
Text is split into chunks and embedded using a HuggingFace sentence-transformer model (all-MiniLM-L6-v2).
Chunks are stored in a Chroma vector store for similarity search.
On each question, the chat history is used to reformulate the question into a standalone query (so it doesn't lose context from prior turns).
The reformulated query retrieves relevant chunks, which are passed to a Groq-hosted LLM to generate the answer.
Chat history is stored per session ID in Streamlit's session state.
Requirements
Python 3.10 or later
A Groq API key
A Hugging Face access token
Dependencies

Inferred from the imports in app.py. Verify against the repo's actual requirements.txt if one exists, since it wasn't accessible for this README.

streamlit
langchain
langchain-classic
langchain-chroma
langchain-community
langchain-core
langchain-groq
langchain-huggingface
langchain-text-splitters
pypdf
chromadb
sentence-transformers
python-dotenv

Install with:

bash
pip install -r requirements.txt
Setup
Clone the repo:
bash
   git clone https://github.com/ramakmn/chatbot_pdf_reader.git
   cd chatbot_pdf_reader
Create and activate a virtual environment:
bash
   python -m venv venv
   # Windows
   venv\Scripts\activate
   # macOS/Linux
   source venv/bin/activate
Install dependencies:
bash
   pip install -r requirements.txt
Create a .env file in the project root with:
   HF_TOKEN=your_huggingface_token_here
   GROQ_API_KEY=your_groq_api_key_here

Both are required. The app will crash on startup if HF_TOKEN is missing, and will fail when generating an answer if GROQ_API_KEY is missing.

Running the app
bash
streamlit run app.py

This opens the app in your browser, usually at http://localhost:8501.

Usage
Enter a session ID (or use the default) to keep separate conversation threads.
Upload one or more PDF files.
Type a question in the input box.
The app displays the answer, along with the raw session store and full chat history.
Known limitations
The app requires at least one PDF to be uploaded before asking a question. Asking a question with no PDF uploaded will error out, since the retrieval chain is only built inside the file-upload branch.
Uploaded PDFs are written to a temporary file (./temp.pdf) that is not cleaned up after processing.
The vector store is rebuilt from scratch on every Streamlit rerun, which is inefficient for large PDFs or long sessions.
No validation is performed on missing API keys before they're used, so misconfiguration produces raw stack traces rather than clear error messages.
License

No license file was found in the repository. Add one (for example MIT or Apache 2.0) if you intend for others to reuse this code.
