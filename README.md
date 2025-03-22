# Semantic Search - ChromaDB Integration

## Overview
This repository demonstrates how to set up and use ChromaDB for efficient semantic search in a Retrieval-Augmented Generation (RAG) pipeline. ChromaDB is a fast and scalable vector database designed for similarity search.

## Installation
To install the necessary dependencies, run:
```bash
pip install chromadb
```

## Libraries Used
- `chromadb` - Vector database for storing and retrieving embeddings
- `google.colab` - To mount Google Drive (if using Google Colab)
- `os` - For handling file paths

## Importing Persisted Data using ChromaDB

### Mount Google Drive (if using Google Colab)
```python
from google.colab import drive
drive.mount('/content/drive')
```

### Define Input Path
```python
input_path = '/content/drive/MyDrive/AI/Semantic_Search_using_CromaDB/Semantic_Search_with_Chromadb/'
```

### Install ChromaDB
```bash
pip install chromadb
```

### Import ChromaDB and Get the Client
```python
import chromadb

chroma_client = chromadb.PersistentClient(path=input_path)

# List available collections
chroma_client.list_collections()

# Retrieve a specific collection
collection = chroma_client.get_collection(name="Semantic_Search_with_Chromadb")

# Peek into the collection
collection.peek()
```

## Methods of ChromaDB
ChromaDB provides various methods to interact with the database:

### 1. **Adding Data**
To insert new documents into a collection:
```python
new_documents = ["This is a new document for semantic search."]
collection.add(documents=new_documents, ids=["doc1"])
```

### 2. **Querying Data**
ChromaDB allows different ways to query data:

#### **Basic Query**
```python
query = "What is retrieval-augmented generation?"
results = collection.query(query_texts=[query], n_results=5)
print(results)
```

#### **Query with `where` Clause**
To filter results based on metadata:
```python
results = collection.query(
    query_texts=[query],
    n_results=3,
    where={'Section': 'Critical response'}
)
print(results)
```

#### **Query with `where_document` Clause**
To filter results based on document content:
```python
results = collection.query(
    query_texts=[query],
    n_results=3,
    where_document={"$or": [{"$contains": "positive"}, {"$contains": "review"}]}
)
print(results)
```

#### **Query with Both `where` and `where_document` Clauses**
Combining metadata and document content filters:
```python
results = collection.query(
    query_texts=[query],
    n_results=3,
    where={'Section': 'Critical response'},
    where_document={"$or": [{"$contains": "positive"}, {"$contains": "review"}]},
    include=['documents', 'distances', 'metadatas']
)
print(results)
```

### 3. **Updating Data**
To modify an existing document:
```python
updated_documents = ["This is an updated document for semantic search."]
collection.update(documents=updated_documents, ids=["doc1"])
```

### 4. **Deleting Data**
To remove a document from the collection:
```python
collection.delete(ids=["doc1"])
```

## Data Preparation
Before querying ChromaDB, ensure that the data is properly structured and stored as embeddings. If you need to add new data, follow these steps:
1. Convert text data into embeddings using a transformer model.
2. Store the embeddings in ChromaDB.

## Contributing
Feel free to submit issues or pull requests to improve this repository.

## License
This project is licensed under the MIT License.

