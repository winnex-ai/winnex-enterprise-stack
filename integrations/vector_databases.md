# Winnex Enterprise Stack — Integration Guide

## Supported Vector Databases

| Database | Type | Integration Method | Audit Support |
|----------|------|-------------------|---------------|
| **FAISS** | Local index | C++/Python library | Full |
| **Pinecone** | Cloud SaaS | Python SDK | Full |
| **Milvus** | Self-hosted | Python SDK | Full |
| **Weaviate** | Self-hosted | Python SDK | Full |
| **Qdrant** | Self-hosted | Python SDK | Full |
| **PostgreSQL pgVector** | Self-hosted | Python SDK | Full |
| **Elasticsearch** | Self-hosted | Python SDK | Partial |
| **OpenSearch** | Self-hosted | Python SDK | Partial |

## Integration Pattern

```python
# Step 1: Build your existing vector index (as usual)
import faiss
index = faiss.IndexFlatIP(128)
index.add(vectors)

# Step 2: Initialize Winnex Audit Engine
from winnex_audit_py import AuditEngine, Config
engine = AuditEngine(Config())
engine.build(vectors)  # 0.09s for 50K vectors

# Step 3: For each search — get both results AND proof
query = vectors[0]

# FAISS returns fast results
faiss_indices = index.search(query.reshape(1,-1), 10)

# Winnex returns mathematical proof
audit_result = engine.search(query, k=10)
proof_json = engine.audit_json(query)

# Step 4: Submit audit trail to compliance dashboard
compliance.record_search(query_id, proof_json)
```
