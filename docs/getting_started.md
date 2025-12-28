# Getting Started

## Installation

### Prerequisites
*   Python 3.8+
*   Rust (cargo) installed (for building from source)

### Install from Source

```bash
# Install maturin (Rust-Python build tool)
pip install maturin

# Clone and build
git clone https://github.com/your-username/memalloy
cd memalloy
maturin develop --release
```

## Quick Start
Create a `demo.py` file:

```python
from memalloy import RAGKernel

# Initialize the RAG kernel (auto-downloads model on first run)
rag = RAGKernel()

# Add knowledge
rag.add_document("Rust is a systems programming language that runs blazingly fast.")
rag.add_document("Python is great for high-level AI logic.")

# Search
results = rag.search("fast language", top_k=1)

for doc, score in results:
    print(f"File: {doc.metadata.get('file_name', 'manual data')}")
    print(f"Content: {doc.content}")
    print(f"Score: {score}")
```

Run it:
```bash
python3 demo.py
```
