# PDF-Insight-AI

A Streamlit-based application that allows users to upload multiple PDF files, process their content, and ask questions based on the extracted text. The app uses Google's Generative AI and FAISS for vector storage to enable conversational question-answering capabilities.

## Features

- Upload multiple PDF files for processing.
- Extract text from PDFs and split it into manageable chunks.
- Generate embeddings using Google's Generative AI model (`embedding-001`).
- Store text embeddings in a FAISS vector store for efficient similarity search.
- Answer user questions based on the content of the uploaded PDFs using a conversational chain powered by `gemini-pro`.
- User-friendly Streamlit interface with a sidebar for file uploads and a main panel for querying.

## Prerequisites

- Python 3.8+
- A Google API key with access to Google Generative AI services.
- A `.env` file with the `GOOGLE_API_KEY` environment variable set.

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/madhav0110/PDF-Insight-AI.git
   cd PDF-Insight-AI
   ```

2. Create a virtual environment and activate it:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install the required dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Create a `.env` file in the project root and add your Google API key:
   ```bash
   GOOGLE_API_KEY=your-google-api-key
   ```

## Dependencies

The project relies on the following Python libraries:
- `streamlit`: For the web interface.
- `PyPDF2`: For extracting text from PDF files.
- `langchain`: For text splitting, embeddings, and question-answering chains.
- `langchain_google_genai`: For Google Generative AI integration.
- `faiss-cpu`: For vector storage and similarity search.
- `python-dotenv`: For loading environment variables.

Install them using:
```bash
pip install streamlit PyPDF2 langchain langchain-google-genai faiss-cpu python-dotenv
```

## Usage

1. Run the Streamlit app:
   ```bash
   streamlit run chatapp.py
   ```

2. Open the app in your browser (typically at `http://localhost:8501`).

3. In the sidebar:
   - Upload one or more PDF files.
   - Click the "Submit & Process" button to extract text, create text chunks, and build the FAISS vector store.

4. In the main panel:
   - Enter a question related to the content of the uploaded PDFs.
   - The app will display the answer based on the context from the PDFs.

If the answer is not found in the provided context, the app will respond with "Answer is not available in the context."

## Project Structure

```
multi-pdf-chatbot/
│
├── chatapp.py                # Main application script
├── .env                  # Environment variables (not tracked in git)
├── faiss_index/          # Directory for FAISS vector store (created after processing)
├── requirements.txt      # List of dependencies
└── README.md             # This file
```

## How It Works

1. **PDF Text Extraction**:
   - The `get_pdf_text` function uses `PyPDF2` to extract text from uploaded PDF files.

2. **Text Chunking**:
   - The `get_text_chunks` function splits the extracted text into smaller chunks using `RecursiveCharacterTextSplitter` with a chunk size of 50,000 characters and 1,000 characters overlap.

3. **Vector Store Creation**:
   - The `get_vector_store` function generates embeddings for the text chunks using Google's `embedding-001` model and stores them in a FAISS vector store.

4. **Conversational Chain**:
   - The `get_conversational_chain` function sets up a question-answering chain using `gemini-pro` with a custom prompt template to ensure accurate answers.

5. **User Query Handling**:
   - The `user_input` function processes user questions by performing a similarity search on the FAISS vector store and passing the relevant documents to the conversational chain.

6. **Streamlit Interface**:
   - The `main` function defines the Streamlit app layout, including a sidebar for file uploads and a text input for user questions.

## Notes

- Ensure your Google API key has access to the Generative AI services.
- The FAISS vector store is saved locally in the `faiss_index` directory for persistence.
- The app uses `allow_dangerous_deserialization=True` when loading the FAISS index. Use caution when deploying in production to ensure the index file is secure.
- The `gemini-pro` model is used with a temperature of 0.3 for balanced response generation.

## Limitations

- The app relies on the quality of text extraction from PDFs, which may vary depending on the PDF format.
- Large PDFs or a high number of files may increase processing time.
- The FAISS index is stored locally and not synced across sessions unless explicitly managed.

