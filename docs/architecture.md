# Architecture

MemAlloy is designed as a hybrid Rust-Python system. The heavy lifting happens in Rust, while the interface remains Pythonic.

## System Components

| Layer | Component | Technology | Description |
| :--- | :--- | :--- | :--- |
| **Interface** | Python Bindings | **PyO3 + Maturin** | Exposes Rust structs as Python classes with zero-copy overhead where possible. |
| **Control** | Async Runtime | **Tokio** | Handles concurrency for file watching and DB operations. |
| **Senses** | File Watcher | **Notify** | recursively watches directories for primitive file events. |
| **Processing** | Embeddings | **FastEmbed-rs** | Runs quantized ONNX models (e.g., `AllMiniLML6V2`) locally on the CPU via `ort`. |
| **Storage** | Vector DB | **LanceDB** | Stores vectors and metadata on disk using Apache Arrow format for high performance. |

## Data Flow

1.  **Ingestion**: `FileWatcher` detects a file change -> Content is read.
2.  **Chunking**: Text is split into `ChunkingStrategy` (default: Sentence) blocks.
3.  **Embedding**: `FastEmbedModel` computes 384d vector for each chunk.
4.  **Storage**: Vectors + Metadata + Content are written to a LanceDB table.
5.  **Retrieval**: `search()` query is embedded -> ANN search in LanceDB -> Results returned.
