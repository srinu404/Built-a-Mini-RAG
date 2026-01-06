# 🏗️ Indecimal Mini RAG Assistant

A Retrieval-Augmented Generation (RAG) chatbot designed to answer questions based on internal construction documents (policies, package pricing, and specifications). This project was built as part of an assignment to demonstrate semantic search and grounded LLM generation.

## 🚀 Features
* **RAG Pipeline:** Retrieves relevant document chunks based on user queries to prevent hallucinations.
* **Transparency:** Displays the exact "Source Context" used to generate the answer, ensuring explainability.
* **Multi-Model Support:** Configurable to use Google Gemini's latest models (Flash/Pro).
* **Vector Search:** Uses FAISS for efficient, local similarity search.

## 🛠️ Tech Stack
* **Language:** Python 3.10+
* **LLM:** Google Gemini (`gemini-flash-latest` / `gemini-pro`)
* **Embeddings:** `sentence-transformers/all-MiniLM-L6-v2` (Hugging Face)
* **Vector Store:** FAISS (Facebook AI Similarity Search)
* **Orchestration:** LangChain
* **Interface:** Streamlit

---

## ⚙️ Design Decisions (Assignment Requirements)

### 1. Model Choices
* **LLM:** We selected **Gemini 1.5 Flash** via the Google GenAI API.
    * *Reason:* It offers a high token rate limit for the free tier, low latency, and strong reasoning capabilities comparable to larger models, making it ideal for a real-time chatbot interface.
* **Embeddings:** We used **`all-MiniLM-L6-v2`**.
    * *Reason:* It is a lightweight, open-source model that runs efficiently on a CPU. It provides a distinct balance of speed and semantic accuracy, which is sufficient for this dataset size without requiring heavy GPU resources.

### 2. Chunking & Retrieval Strategy
* **Chunking:** Implemented `RecursiveCharacterTextSplitter` with a chunk size of **500 characters** and an overlap of **50 characters**.
    * *Why:* Construction documents often contain dense pricing data (e.g., "Essential Package: ₹1,851/sqft"). Smaller chunks ensure that specific numbers are not lost in large blocks of text, while the overlap preserves context across sentence boundaries.
* **Retrieval:** We use **FAISS** with Euclidean distance (L2) to find the top **3** most similar chunks to the user's query.

### 3. Grounding & Hallucination Prevention
* **System Prompting:** The LLM is explicitly instructed to answer *only* based on the provided context and to reply "I don't know" if the information is missing.
* **Transparency UI:** The application includes a "View Retrieved Context" expander. This allows users to verify the answer against the raw text chunks, ensuring trust in the system's output.

---

## 💻 Installation & Setup

### Prerequisites
* Python 3.10 or higher
* A Google Gemini API Key

### Steps
1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/your-username/indecimal-mini-rag.git](https://github.com/your-username/indecimal-mini-rag.git)
    cd indecimal-mini-rag
    ```

2.  **Install Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Prepare Data:**
    * Place your PDF documents (`doc1.pdf`, `doc2.pdf`, `doc3.pdf`) inside a folder named `data/` in the root directory.

4.  **Run Ingestion (Build the Index):**
    * This script processes the PDFs and creates the local FAISS index.
    ```bash
    python ingestion.py
    ```

5.  **Run the Application:**
    ```bash
    streamlit run app.py
    ```

---

## 📂 Project Structure
```text
├── data/                  # Source PDF documents
├── vector_store/          # FAISS index (generated automatically)
├── app.py                 # Streamlit chat application
├── ingestion.py           # Script to chunk text and build vector store
├── requirements.txt       # Python dependencies
└── README.md              # Project documentation
