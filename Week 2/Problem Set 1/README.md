# Week 2 - Retrieval-Augmented Generation (RAG) & Vector Search

Welcome to Week 2 of the Generative AI Track!

This week focuses on the core techniques that allow LLMs to answer questions about your own documents – without retraining. You will work with embeddings, vector databases, and build a mini RAG pipeline. These exercises bridge the gap between raw LLM API calls and document‑aware AI systems.

By completing these exercises, you will understand how to represent text as vectors, perform semantic search, chunk documents intelligently, and ground LLM responses with retrieved context.

---

# Task 1: Cosine Similarity Between Sentences (Easy)

**Difficulty:** Easy

## Problem Statement

Write a Python program that:

1. Takes two sentences as input from the user.
2. Uses a simple embedding representation (e.g., `sentence-transformers/all-MiniLM-L6-v2`) to convert each sentence into a vector.
3. Computes the **cosine similarity** between the two vectors.
4. Prints the similarity score (0 = unrelated, 1 = identical meaning).

### Example

Input:

```
Sentence A: "The cat sleeps on the mat."
Sentence B: "A cat is resting on the rug."
```

Output:

```
Cosine Similarity: 0.87
```

## Learning Objectives

- Understand what embeddings are.
- Compute cosine similarity manually or via a library.
- See how semantic meaning can be captured numerically.

> **Hint:** Install `sentence-transformers`. The model `all-MiniLM-L6-v2` produces 384‑dimensional vectors.

---

# Task 2: Build a Simple In‑Memory Vector Store (Easy)

**Difficulty:** Easy

## Problem Statement

Create a class `SimpleVectorStore` that:

- Stores documents (strings) along with their embeddings.
- Provides an `add(document)` method – generates embedding and stores it.
- Provides a `search(query, k=3)` method – returns the top‑k most similar documents by cosine similarity.

Test your class with at least 5 short sentences (e.g., facts about Python, AI, and GenAI). Then query with a new sentence and show the top 3 matches.

### Example

Documents:

```
1. "Python is a programming language."
2. "LLMs are trained on large text corpora."
3. "RAG combines retrieval and generation."
4. "Embeddings map text to vectors."
5. "Groq provides fast LLM inference."
```

Query: `"How do we represent text for search?"`

Expected output: The most similar document should be `"Embeddings map text to vectors."`

## Learning Objectives

- Implement a vector similarity search from scratch (without ChromaDB).
- Understand the relationship between embeddings and retrieval.
- Practice using a real embedding model.

> **Note:** Use the same `sentence-transformers` model as in Task 1.

---

# Task 3: Chunking a PDF and Embedding into ChromaDB (Medium)

**Difficulty:** Medium

> You've made it past the warm-up — that's genuinely not nothing. This one will feel a bit more involved, and there will probably be a moment where something doesn't index right or ChromaDB throws a weird error. That's fine, push through it. The concepts you lock in here are the backbone of everything in Task 4.

## Problem Statement

You are given a PDF file (you can use any sample PDF, e.g., a short research paper or lecture notes). Write a Python script that:

1. Loads the PDF using `PyPDFLoader` (from `langchain_community`).
2. Splits the document into overlapping chunks of **500 characters** with **100 character overlap** using `RecursiveCharacterTextSplitter`.
3. Generates embeddings for each chunk using `HuggingFaceEmbeddings` with model `all-MiniLM-L6-v2`.
4. Stores the chunks (text + embeddings + metadata: source filename and page number) into a **persistent ChromaDB** collection.
5. Prints the total number of chunks created and the first 3 chunks with their page numbers.

### Expected Output Format

```
Indexed 47 chunks into ChromaDB (persistent directory: ./chroma_store)

Sample chunk 1 (page 2):
[content preview...]

Sample chunk 2 (page 2):
...
```

## Learning Objectives

- Understand the **indexing phase** of RAG.
- Learn practical chunking strategies (size, overlap, metadata).
- Use ChromaDB as a local vector database.

> **Required packages:** `langchain`, `langchain-community`, `chromadb`, `pypdf`, `sentence-transformers`.
>
> **Hint:** Use `PersistentClient` so the index survives script restarts.

---

# Task 4: Build a Grounded Q&A System (Hard)

**Difficulty:** Hard

> Okay, this is the big one. If Task 3 felt like assembling parts, this is putting the whole machine together. It's going to take some time — the Gradio wiring alone can be fiddly. But when you ask it a question and it answers with the right page number cited, you'll know exactly why RAG matters. Don't skip this one.

## Problem Statement

Combine everything from previous tasks into a **complete RAG query pipeline** and a **simple Gradio UI**.

Your program should:

1. **Index** (if no existing ChromaDB collection exists) – same as Task 3.
2. **Query** – user asks a question via a Gradio textbox.
3. **Retrieve** – fetch the top‑4 most relevant chunks from ChromaDB.
4. **Ground** – construct a system prompt that instructs the LLM to:
   - Answer using ONLY the retrieved context.
   - If the answer is not in the context, respond: *"I don't have that information in the uploaded documents."*
   - Cite sources using `[Source: filename, page X]` after each factual statement.
5. **Generate** – call Groq's `llama-3.3-70b-versatile` (or any Groq model) with temperature = 0.
6. **Display** – show the answer and also an **accordion** ("Retrieved Context") that displays the actual chunks used, with filenames and page numbers.

### UI Requirements (using `gradio.Blocks`)

- A `gr.File` upload component (multiple PDFs allowed) + an **"Index Documents"** button.
- A status label (e.g., "Indexed 3 documents – 142 chunks").
- A `gr.Chatbot` for conversation.
- A `gr.Textbox` for user questions.
- A `gr.Accordion` (openable) that shows the retrieved chunks for the last query.

### Testing & Anti‑hallucination

- Ask a question that is **clearly answered** in your PDFs – the answer must cite correct page numbers.
- Ask a question **not** present in the PDFs (e.g., "What is the capital of France?") – the model must say it doesn't know.

### Example Interaction

**User:** What does the document say about prompt engineering?

**Assistant:** The document states that prompt engineering is the craft of writing instructions to reliably produce desired outputs (Source: lecture_notes.pdf, page 5). It also recommends using few‑shot examples for format control (Source: lecture_notes.pdf, page 6).

## Learning Objectives

- Build a complete, production‑style RAG pipeline.
- Implement **grounding** and **source citations**.
- Understand the separation between indexing and querying.
- Learn to debug retrieval quality via the UI accordion.

> **Hint:** Use `vectorstore.as_retriever(search_kwargs={"k": 4})` for retrieval.
> Store the retrieved chunks (and their metadata) in a Gradio `gr.State` to display them in the accordion after each answer.

---

# Bonus Challenge (Optional)

Extend Task 4 to support **multi‑document filtering**:

- Add a `gr.Dropdown` that lists all indexed document filenames (plus "All Documents").
- When a specific document is selected, restrict retrieval using ChromaDB's `where={"source": filename}`.
- Show how the answer changes when only one PDF is considered.

---

# Submission Guidelines

- Write clean, well‑commented Python code.
- Use a virtual environment and provide a `requirements.txt`.
- Include a `.env.example` (for `GROQ_API_KEY`).
- Push your code to your `genai-soc-2026` repo under a folder named `week2-rag/`.
- Your `README.md` should contain:
  - A screenshot of the Gradio app.
  - The two anti‑hallucination test results (in‑scope and out‑of‑scope questions).
  - A short explanation of how chunking and embeddings work in your own words.

Happy Coding – and welcome to the world of document‑aware AI!
