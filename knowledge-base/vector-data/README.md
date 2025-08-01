# Vector Knowledge Base Data

This directory contains preprocessed data optimized for vector search and RAG applications.

## Structure

- `chunks-*.json`: Document chunks with metadata, split into batches
- `file-summaries.json`: Summary information for each processed file
- `vector-index.json`: Index metadata and statistics
- `sample-queries.json`: Example queries for testing

## Usage with Vector Databases

### Pinecone Example
```javascript
const chunks = require('./chunks-0000.json');
for (const chunk of chunks.chunks) {
  await pinecone.upsert({
    id: chunk.id,
    values: await getEmbedding(chunk.content),
    metadata: chunk.metadata
  });
}
```

### Weaviate Example
```javascript
const chunks = require('./chunks-0000.json');
for (const chunk of chunks.chunks) {
  await client.data.creator()
    .withClassName('Document')
    .withId(chunk.id)
    .withProperties({
      content: chunk.content,
      ...chunk.metadata
    })
    .do();
}
```

### ChromaDB Example
```python
import chromadb
chunks = json.load(open('./chunks-0000.json'))
collection.add(
    documents=[c['content'] for c in chunks['chunks']],
    metadatas=[c['metadata'] for c in chunks['chunks']],
    ids=[c['id'] for c in chunks['chunks']]
)
```

## Metadata Fields

Each chunk contains:
- `content`: The actual text content
- `metadata.filePath`: Original file path
- `metadata.classifications`: Academic field classifications
- `metadata.codeStructure`: Extracted code elements
- `metadata.chunkIndex`: Position in original file
- `metadata.summary`: Brief description of chunk content

## Academic Classifications

Content is automatically classified into academic fields based on the taxonomy in `academic-fields.js`.
