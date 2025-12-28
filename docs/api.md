# API Reference

## RAGKernel

The core controller for the system.

### `memalloy.RAGKernel(db_path=None, chunk_size=500, overlap=50)`
*   **db_path** *(str, optional)*: Path to store LanceDB data. Defaults to `./.memalloy_data`.
*   **chunk_size** *(int)*: Max characters per chunk.
*   **overlap** *(int)*: Character overlap between chunks.

### `add_document(content, metadata=None) -> str`
Ingest a raw string document.
*   **content** *(str)*: Text to ingest.
*   **metadata** *(dict, optional)*: Key-value metadata.
*   **Returns**: Document ID.

### `search(query, top_k=5) -> List[Tuple[Document, float]]`
Semantic search.
*   **Returns**: List of `(Document, score)` tuples.

### `count() -> int`
Returns total number of chunks embedded.

---

## FileWatcher

Automates ingestion from a directory.

### `memalloy.FileWatcher(rag_kernel, watch_path, extensions=None, recursive=True)`
*   **rag_kernel**: Instance of `RAGKernel`.
*   **watch_path**: Directory to watch.
*   **extensions**: List of file extensions (e.g., `["md", "txt"]`).

### `sync()`
Scans the folder and ingests all matching files immediately.

### `start_watching()`
Starts a background thread to watch for changes.

### `stop_watching()`
Stops the background thread.

---

## Document

Data container.

### Properties
*   `id`: Unique ID.
*   `content`: Text content.
*   `metadata`: Dictionary of metadata.
