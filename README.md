# ***Semantic Search with FAISS, LangChain, and ChatGroq***

## ***Project Description***
This repository implements a semantic search engine for both products and services, leveraging the following technologies:

- **HuggingFaceEmbeddings**: Utilizing a fine-tuned Sentence-BERT or similar model for embedding generation.
- **FAISS**: For efficient vector storage and retrieval.
- **LangChain**: To manage retrieval processes and a `map_reduce` chain.
- **ChatGroq**: Serving as the Large Language Model (LLM).

The application demonstrates batch ingestion of documents into FAISS, storing metadata in each document (e.g., whether it’s a “product” or “service”), and producing a summarized answer referencing retrieved items.

---

## ***Table of Contents***
1. [***Features***](#features)
2. [***Installation***](#installation)
3. [***Usage***](#usage)
4. [***Data Flow***](#data-flow)
5. [***Customization***](#customization)
6. [***License***](#license)

---
## Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/your-repo/semantic-search-engine.git
   cd semantic-search-engine
   ```

2. Create a virtual environment and activate it:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Linux/Mac
   venv\Scripts\activate    # On Windows
   ```

3. Install the required dependencies:
   ```bash
   pip install -r requirements.txt
   ```
## ***Features***

- **Batch Document Ingestion**: Efficiently adds documents to FAISS in batches to handle large datasets.
- **Map-Reduce Chain**: Splits documents into chunks (map step) and merges partial answers (reduce step).
- **Exponential Backoff**: Retries requests if LLM or embedding API calls hit rate limits (e.g., HTTP 429).
- **Metadata Handling**: Each document includes metadata fields (e.g., `docType`, `id`, `name`, `description`) for displaying relevant product/service information in the final output.

---

## ***Installation***

### ***Install Dependencies***

Ensure the following packages are included in your `requirements.txt`:

- `langchain`
- `faiss-cpu` (or `faiss-gpu` if you have GPU support)
- `langchain_groq`
- `sentence_transformers`
- `python-dotenv`
- `pandas`

---

## ***Set Up Environment Variables***

Create a `.env` file in the project root containing:

```bash
GROQ_API_KEY=<YOUR_GROQ_API_KEY>
```

---

## ***Usage***

### ***Prepare Your Data***
Modify or replace the example `df_products` and `df_services` with your own data sources.

### ***Run the Script***
Run the main script (e.g., `python main.py`) to:

1. Load environment variables (notably `GROQ_API_KEY`).
2. Initialize embeddings (`HuggingFaceEmbeddings`) and the ChatGroq LLM.
3. Create DataFrames for products and services, transforming them into LangChain Document objects.
4. Build a FAISS vector store, add documents in batches, and set up a `map_reduce` RetrievalQA chain.

### ***Perform a Search***
Use the `semantic_search_tool(query)` function at the end of your script or in another module:

```python
result = semantic_search_tool("Tell me about painting services")
print(result)
```

---

## ***Data Flow***

### **DataFrames**
- Example: `df_products`, `df_services`

### **Document Creation**
- Functions: `create_product_documents(df_products)`, `create_service_documents(df_services)`

### **FAISS Vector Store**
- Initialized via `build_faiss_vector_store`.
- Documents added in chunks using `add_documents_to_store`.

### **Map-Reduce RetrievalQA**
- Chain utilizes custom `map_prompt` and `reduce_prompt`.
- Function: `qa_chain.invoke(query)` performs retrieval, partial summarization, and merging of results.

### **Semantic Search Tool**
- Calls `qa_chain.invoke(query)`.
- Displays the final LLM-generated answer and relevant metadata.

---

## ***Customization***

- **Prompts**: Edit `map_prompt` and `reduce_prompt` to change how partial answers are summarized and the final output is structured.

- **Filtering**: Add filters for documents (e.g., filter by product category like “Men’s jeans”) post-retrieval or within the retriever.

- **LLM Settings**: Adjust `temperature`, `max_tokens`, or `model_name` in ChatGroq to customize output style and length.

- **Embeddings**: Swap out `HuggingFaceEmbeddings` for another embedding model or point to a Hugging Face hub model name.

---

## ***License***
This project is licensed under the MIT License. See the LICENSE file for details.

