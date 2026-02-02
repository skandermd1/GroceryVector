# 🛒 Grocery Similarity Search

<div align="center">

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![ChromaDB](https://img.shields.io/badge/ChromaDB-Latest-green.svg)
![SentenceTransformers](https://img.shields.io/badge/Sentence--Transformers-Latest-orange.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

**A semantic search engine for grocery items using ChromaDB and sentence embeddings**

Find similar products • Vector similarity • Intelligent search

</div>

---

## 📌 Overview

**Grocery Similarity Search** is a Python application that demonstrates how to build a semantic search engine using ChromaDB and Sentence Transformers. Unlike traditional keyword search, it understands the **meaning** of grocery items and finds similar products based on semantic similarity.

### 🎯 What It Does

Given a search term like *"apple"*, the system finds semantically similar grocery items:
- ✅ **"fresh red apples"** - Direct match
- ✅ **"golden apple"** - Color variation
- ✅ **"red fruit"** - Semantic similarity

The search understands that "golden apple" and "red fruit" are related to "apple" even though the words are different!

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🧠 **Semantic Understanding** | Uses `all-MiniLM-L6-v2` for intelligent embeddings |
| 🗄️ **Vector Database** | ChromaDB for efficient similarity search |
| 📊 **Cosine Similarity** | Accurate distance metric for text comparison |
| ⚡ **Fast Retrieval** | Quick top-K similar item retrieval |
| 📝 **Metadata Support** | Store additional information with each item |
| 🎓 **Educational** | Clear, well-commented code for learning |

---

## 🚀 Installation

### Prerequisites

- Python 3.8 or higher
- pip package manager

### Setup

```bash
# 1. Clone the repository
git clone https://github.com/yourusername/grocery-similarity-search.git
cd grocery-similarity-search

# 2. Create virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt
```

### requirements.txt

```text
chromadb>=0.4.0
sentence-transformers>=2.2.0
```

---

## 💻 Usage

### Run the Application

```bash
python similarity_search.py
```

### Expected Output

```
Collection created: my_grocery_collection
Collection contents:
Number of documents: 14

Query results for 'apple':
Top 3 similar documents to "apple":
 - ID: food_1, Text: "fresh red apples", Score: 0.2156
 - ID: food_13, Text: "golden apple", Score: 0.3421
 - ID: food_14, Text: "red fruit", Score: 0.4789
```

---

## 📖 How It Works

### Architecture

```
┌─────────────────────────────────────────────────┐
│              User Query: "apple"                │
└──────────────────┬──────────────────────────────┘
                   │
                   ↓
┌─────────────────────────────────────────────────┐
│      Sentence Transformer Embedding             │
│      (all-MiniLM-L6-v2)                        │
│      "apple" → [0.23, -0.45, 0.67, ...]        │
└──────────────────┬──────────────────────────────┘
                   │
                   ↓
┌─────────────────────────────────────────────────┐
│              ChromaDB Collection                │
│   ┌─────────────────────────────────────┐      │
│   │ food_1: "fresh red apples"          │      │
│   │ food_2: "organic bananas"           │      │
│   │ food_13: "golden apple"             │      │
│   │ ...                                 │      │
│   └─────────────────────────────────────┘      │
└──────────────────┬──────────────────────────────┘
                   │
                   ↓
┌─────────────────────────────────────────────────┐
│         Cosine Similarity Search                │
│         Find 3 most similar items               │
└──────────────────┬──────────────────────────────┘
                   │
                   ↓
┌─────────────────────────────────────────────────┐
│              Results Returned                    │
│   1. fresh red apples (0.2156)                  │
│   2. golden apple (0.3421)                      │
│   3. red fruit (0.4789)                         │
└─────────────────────────────────────────────────┘
```

### Key Components

#### 1. **Embedding Function**
```python
ef = embedding_functions.SentenceTransformerEmbeddingFunction(
    model_name="all-MiniLM-L6-v2"
)
```
Converts text into 384-dimensional vectors that capture semantic meaning.

#### 2. **ChromaDB Collection**
```python
collection = client.create_collection(
    name="my_grocery_collection",
    configuration={"hnsw": {"space": "cosine"}}
)
```
Creates a vector database optimized for cosine similarity search.

#### 3. **Document Storage**
```python
collection.add(
    documents=texts,
    metadatas=[{"source": "grocery_store", "category": "food"}],
    ids=ids
)
```
Stores grocery items with automatic embedding generation.

#### 4. **Similarity Search**
```python
results = collection.query(
    query_texts=["apple"],
    n_results=3
)
```
Finds the top 3 most similar items using vector similarity.

---


## 🔧 Customization

### Change the Search Query

Edit the `query_term` variable in `similarity_search.py`:

```python
query_term = "bread"  # Try: "coffee", "meat", "vegetables", etc.
```

### Add More Items

Add new items to the `texts` array:

```python
texts = [
    'fresh red apples',
    'organic bananas',
    # Add your items here:
    'fresh strawberries',
    'organic milk',
    'whole grain pasta'
]
```

### Change Number of Results

Modify the `n_results` parameter:

```python
results = collection.query(
    query_texts=[query_term],
    n_results=5  # Get top 5 results instead of 3
)
```

### Use Different Embedding Model

Change the embedding function:

```python
ef = embedding_functions.SentenceTransformerEmbeddingFunction(
    model_name="paraphrase-multilingual-MiniLM-L12-v2"  # Multilingual support
)
```

**Available models:**
- `all-MiniLM-L6-v2` - Fast and efficient (default)
- `all-mpnet-base-v2` - Higher quality, slower
- `paraphrase-multilingual-MiniLM-L12-v2` - Multilingual support

---

## 📊 Sample Data

The application includes 14 grocery items:

```python
texts = [
    'fresh red apples',       # Fruits
    'organic bananas',
    'ripe mangoes',
    'whole wheat bread',      # Bakery
    'farm-fresh eggs',        # Dairy
    'natural yogurt',
    'frozen vegetables',      # Frozen
    'grass-fed beef',         # Meat
    'free-range chicken',
    'fresh salmon fillet',    # Seafood
    'aromatic coffee beans',  # Beverages
    'pure honey',            # Condiments
    'golden apple',          # Variations
    'red fruit'
]
```

---

## 🎓 Educational Value

This project demonstrates:

### Core Concepts
- ✅ **Vector Embeddings** - Converting text to numerical representations
- ✅ **Semantic Search** - Understanding meaning beyond keywords
- ✅ **Vector Databases** - Efficient storage and retrieval of embeddings
- ✅ **Cosine Similarity** - Measuring similarity between vectors
- ✅ **Metadata Management** - Storing additional information with vectors

### Skills Learned
- Using ChromaDB for vector storage
- Implementing semantic search with Sentence Transformers
- Working with embeddings and similarity metrics
- Building practical AI applications

---

## 🧪 Example Queries and Results

### Query: "apple"
```
1. fresh red apples     (Score: 0.2156) ✅ Direct match
2. golden apple        (Score: 0.3421) ✅ Color variation
3. red fruit           (Score: 0.4789) ✅ Semantic similarity
```

### Query: "meat"
```
1. grass-fed beef      (Score: 0.1823)
2. free-range chicken  (Score: 0.2145)
3. fresh salmon fillet (Score: 0.3567)
```

### Query: "breakfast"
```
1. farm-fresh eggs     (Score: 0.3245)
2. whole wheat bread   (Score: 0.3891)
3. aromatic coffee beans (Score: 0.4123)
```

**Note:** Lower scores indicate higher similarity!

---

## 🐛 Troubleshooting

### Issue: ChromaDB not installed

```bash
pip install chromadb
```

### Issue: Sentence Transformers model download fails

```bash
# The model will download automatically on first run
# If it fails, try manually:
from sentence_transformers import SentenceTransformer
model = SentenceTransformer('all-MiniLM-L6-v2')
```

### Issue: Collection already exists

The script creates a new collection each time. To avoid errors, either:

1. Delete the collection before re-running:
```python
client.delete_collection(name=collection_name)
```

2. Or get the existing collection:
```python
collection = client.get_collection(name=collection_name)
```

---

## 🚀 Advanced Usage

### Persistent Storage

Save your collection to disk:

```python
import chromadb

# Use persistent client
client = chromadb.PersistentClient(path="./chroma_db")
```

### Filtering Results

Add filters to your query:

```python
results = collection.query(
    query_texts=["apple"],
    n_results=3,
    where={"category": "food"}  # Filter by metadata
)
```

### Batch Queries

Search for multiple terms at once:

```python
results = collection.query(
    query_texts=["apple", "bread", "coffee"],
    n_results=3
)
```



## 🤝 Contributing

Contributions are welcome! Feel free to:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -m 'Add improvement'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Open a Pull Request

### Ideas for Contribution

- 🌍 Add support for multiple languages
- 📊 Create visualization of embeddings
- 🔍 Implement advanced filtering options
- 📱 Add a simple web interface
- 🧪 Add unit tests
- 📈 Performance benchmarking

---

## 📝 License

This project is licensed under the MIT License.

```
MIT License

Copyright (c) 2025 Iskander Mdimegh

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 👨‍💻 Author

**Iskander Mdimegh**

---

## 🙏 Acknowledgments

- **ChromaDB Team** - For the excellent vector database
- **Sentence Transformers** - For the embedding models
- **Hugging Face** - For hosting the models

---

## 📊 Quick Start

```bash
# Install dependencies
pip install chromadb sentence-transformers

# Run the application
python similarity_search.py

# You should see:
# Collection created: my_grocery_collection
# Top 3 similar documents to "apple":
#  - ID: food_1, Text: "fresh red apples", Score: 0.2156
#  - ID: food_13, Text: "golden apple", Score: 0.3421
#  - ID: food_14, Text: "red fruit", Score: 0.4789
```

---

<div align="center">

**Made with 🧠 and Python**

[⭐ Star this repo](https://github.com/yourusername/grocery-similarity-search) • [🐛 Report Bug](https://github.com/yourusername/grocery-similarity-search/issues) • [💡 Request Feature](https://github.com/yourusername/grocery-similarity-search/issues)

**Happy Searching! 🛒**

</div>
