# 📌 UDST Policy Chatbot (v2)

This **v2** of the **UDST Policy Chatbot** enhances the original RAG-based solution with **streamlined embeddings storage** and a **better chat interface**. When a user first runs the app, the system **embeds all policy chunks** (if none are found in the pickle) and **caches** them in `rag_data.pkl`. **Subsequent runs** skip re-embedding entirely, ensuring faster performance and reduced API usage.

---

## **📂 Project Structure**

```
📦 UDST-Policy-Chatbot
├── assets
│   ├── combined_documents.txt   # Merged policy documents
│   ├── rag_data.pkl             # Pickled policy embeddings (updated to store chunk_embeddings)
│   ├── rag_index.faiss          # FAISS vector database
├── app.py                       # v2 of the Streamlit chatbot application
├── notebook.ipynb               # Jupyter notebook for preprocessing & testing
└── README.md                    # Project documentation
```

---

## **🛠 How It Works**

1. **Data Collection**  
   - Policy text is initially **scraped** from **official UDST webpages**.
   - The content is **merged** into `combined_documents.txt` to create a unified text corpus.

2. **Text Embedding & Indexing**  
   - The **Mistral AI** model converts each chunk of policy text into a **dense embedding vector**.
   - These embeddings are saved in **`rag_data.pkl`** (under the key `chunk_embeddings`) for **long-term storage**.
   - A **FAISS** index (`rag_index.faiss`) is built for rapid **similarity-based** retrieval.

3. **Initial vs. Subsequent Runs**  
   - On the **first run**, if `chunk_embeddings` are **missing** in `rag_data.pkl`, the system will:
     1. **Embed** all policy chunks using **Mistral**.
     2. Save (`pickle`) those embeddings back into `rag_data.pkl` under the key `chunk_embeddings`.
   - **Subsequent runs** simply **load** the existing embeddings, **avoiding** re-embedding and **reducing** API calls.

4. **Query Processing**  
   - A user enters their question in the **chat input** (`st.chat_input`) inside the **Streamlit** app (`app.py`).
   - **Agentic Step**: The system queries **Mistral** to decide which **two** policies are most relevant to the question.
   - **RAG Pipeline**:  
     1. **Filter** the FAISS index to chunks from those selected policies.  
     2. Retrieve the top matching chunks.  
     3. Construct a context-based prompt.  
     4. Generate a final answer via **Mistral**.

5. **Output & Policy Cards**  
   - The chatbot displays **which policies** were deemed most relevant.
   - The **answer** is shown in the main chat area.
   - A **grid of policy cards** (with emojis, descriptions, and “Read Policy” links) is listed **below** the chat box, making it easy to see available policies.

---

## **🚀 Setup & Installation**

### **🔧 Prerequisites**
1. **Python 3.8+**  
2. The required dependencies (in `requirements.txt`).

```bash
pip install -r requirements.txt
```

### **📌 Running the Chatbot**
1. Place your **Mistral API Key** in the environment or `.env` file.
2. Ensure `rag_data.pkl` and `rag_index.faiss` are in the **assets** folder.
3. Launch the **Streamlit** application:

```bash
streamlit run app.py
```

### **🔑 Getting your API key**
Visit [**Mistral AI's API Keys Page**](https://console.mistral.ai/api-keys) to generate a new API key. Put that key in your environment, or update `rag_data.pkl` accordingly.

### **📊 Running the Notebook**
If you want to re-scrape or update policy data, use the **Jupyter notebook**:

```bash
jupyter notebook notebook.ipynb
```

---

## **📂 Data Files**
- **`combined_documents.txt`** → Unified collection of all policy text.  
- **`rag_index.faiss`** → **FAISS** vector index for similarity search.  
- **`rag_data.pkl`** → Pickle storing:
  - **`chunks`** & **`sources`**: policy text chunks and their metadata  
  - **`chunk_embeddings`**: dense embeddings for each chunk (added on first run)  

---

## **🔗 References**
- **Mistral AI**: [https://mistral.ai](https://mistral.ai)  
- **FAISS**: [https://faiss.ai](https://faiss.ai)  
- **Streamlit**: [https://streamlit.io](https://streamlit.io)  

---
