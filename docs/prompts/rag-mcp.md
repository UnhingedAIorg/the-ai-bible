# RAG & Model Context Protocol (MCP) Prompts

**Ground your AI in truth and personal data.**

This library contains **110+ prompts** for retrieval-augmented generation, knowledge management, context engineering, and MCP-based persistent systems.

---

## Core RAG Mastery

### 1. Perfect RAG Query
Using only the provided context, answer accurately. If the answer is not in the context, say "I don't have enough information" and suggest what additional context would help. Cite your sources using [Source: X]. Be precise and don't hallucinate.

### 2. Context Compression
Summarize the following documents into a dense, query-optimized knowledge base for future retrieval. Preserve key facts, relationships, and nuance while reducing token count by 80%. Structure for efficient embedding and retrieval.

### 3. Personal Knowledge Vault
You have access to my entire life archive (emails, notes, photos metadata, reading history, journal entries). Answer as if you are me, with perfect recall and my personal style. Reference specific memories when relevant. Maintain my voice and perspective completely.

### 4. Multi-Source Synthesis
Synthesize answers from multiple retrieved sources:
1. Identify relevant information from each source
2. Note contradictions between sources
3. Reconcile conflicting information with confidence levels
4. Combine complementary insights
5. Cite all sources appropriately
6. Flag when sources are insufficient

### 5. Query Expansion for Retrieval
Given user query: [query], generate 10 semantically similar queries that might retrieve additional relevant documents. Include synonyms, rephrasings, and related concepts. Use all variations to broaden search coverage.

### 6. Hypothetical Document Embedding (HyDE)
Given query: [query], first generate a hypothetical ideal document that would answer this query perfectly. Then use this hypothetical document to retrieve actual similar documents from the corpus.

### 7. Step-Back Prompting for RAG
Original question: [specific question]. First, ask a broader, more general question to retrieve relevant background context. Then answer the specific question using both the broad context and specific retrieved chunks.

### 8. Chain-of-Verification RAG
Generate initial answer from retrieved context. Then:
1. Identify claims made in the answer
2. Verify each claim against retrieved sources
3. Flag any claims not supported by context
4. Revise answer, removing unsupported claims
5. Note what additional information would be needed

---

## Chunking & Document Processing

### 9. Semantic Chunking Strategy
Process this document using semantic chunking:
1. Identify natural boundaries (paragraphs, sections)
2. Maintain context within chunks (overlap previous chunk)
3. Preserve semantic coherence (don't split mid-thought)
4. Optimize chunk size for embedding model (target 512 tokens)
5. Add metadata (source, position, section)
6. Generate summary for each chunk

### 10. Hierarchical Chunking
Create multi-level chunks for [document]:
- **Level 1**: Sections/chapters (coarse)
- **Level 2**: Paragraphs (medium)
- **Level 3**: Sentences (fine)

Enable retrieval at appropriate granularity based on query type.

### 11. Document Enrichment
Enrich documents before embedding:
1. Extract entities (people, places, organizations, concepts)
2. Identify relationships between entities
3. Generate hypothetical questions this doc answers
4. Create keyword tags
5. Summarize in 3 sentences
6. Add temporal metadata if relevant

### 12. Parent-Child Chunking
Implement parent-child chunking:
1. Small child chunks for precise retrieval (128-256 tokens)
2. Parent chunks containing broader context (1024 tokens)
3. Retrieve children, return parent for context
4. Balance precision with context richness

### 13. Sliding Window Chunking
Process [long document] with sliding window:
1. Window size: 512 tokens
2. Step size: 256 tokens (50% overlap)
3. Maintain continuity across boundaries
4. Tag with position and surrounding context

### 14. Structured Data Extraction
Extract structured information from [document]:
1. Tables → JSON/CSV
2. Key-value pairs
3. Named entities with types
4. Dates and temporal information
5. Relationships and references
6. Store structured alongside raw text

---

## Embedding & Retrieval Optimization

### 15. Hybrid Search Query
Execute hybrid retrieval:
1. **Dense retrieval**: Embedding similarity for semantic match
2. **Sparse retrieval**: BM25/TF-IDF for keyword match
3. **Re-ranking**: Cross-encoder for final ranking
4. **Fusion**: Combine scores (typically α=0.5)
5. Return top-k with scores and source types

### 16. Query Embedding Optimization
Optimize queries for embedding retrieval:
1. Remove stop words unless meaningful
2. Preserve key entities and nouns
3. Consider query expansion with hyponyms/hypernyms
4. Use natural language (not keyword soup)
5. Include context if query is ambiguous
6. Test with hypothetical matches

### 17. Multi-Vector Representation
For [document], create multiple embeddings:
1. **Dense passage embedding**: Overall meaning
2. **ColBERT-style token embeddings**: Fine-grained matching
3. **Sentence embeddings**: Key sentence representations
4. **Summary embedding**: High-level concept
5. Use appropriate representation per query type

### 18. Embedding Model Selection
Recommend embedding model for [use case]:
1. **OpenAI**: text-embedding-3-large (general purpose)
2. **Cohere**: embed-multilingual-v3 (multilingual)
3. **Open source**: BGE, GTE, E5 models (cost/performance)
4. **Domain-specific**: Fine-tuned for legal/medical/technical
5. Consider: dimensionality, context length, cost, latency

### 19. Index Optimization
Optimize vector index for [corpus characteristics]:
1. **Flat (brute force)**: Small corpus (<10k), maximum accuracy
2. **IVF**: Medium corpus, balanced speed/accuracy
3. **HNSW**: Large corpus, approximate search
4. **ScaNN**: Very large corpus, high performance
5. Tune parameters for recall@k requirements

### 20. Re-ranking Strategy
Apply re-ranking to initial retrieval results:
1. Retrieve top-100 with fast embedding search
2. Apply cross-encoder re-ranking (more precise)
3. Return top-10 after re-ranking
4. Consider latency vs. quality trade-off

---

## Context Management & Prompt Engineering

### 21. Context Window Prioritization
Given [large context] and limited window, prioritize:
1. Most semantically similar chunks to query
2. Most recent information (if temporal)
3. Most authoritative sources
4. Diverse perspectives (avoid echo chamber)
5. Recency-weighted relevance scoring

### 22. Dynamic Context Injection
Structure RAG prompt dynamically:
```
System: You are a helpful assistant with access to reference materials.

Context: [Retrieved documents with citation markers]

Guidelines:
- Answer based primarily on provided context
- Cite sources using [1], [2], etc.
- Say "I don't know" if context insufficient
- Synthesize multiple sources when helpful

User Query: [Question]
```

### 23. Contextual Compression
Compress retrieved context before inclusion:
1. Remove redundant information
2. Extract key sentences only
3. Summarize verbose sections
4. Preserve unique facts from each source
5. Maintain citations for provenance

### 24. Provenance Tracking
Ensure every statement can be traced:
1. Tag each context chunk with source ID
2. Require model to cite [Source: X] for facts
3. Validate citations exist in provided context
4. Surface sources to user for verification
5. Enable click-through to original documents

### 25. Contextual Few-Shot Examples
Include relevant examples in context:
1. Retrieve similar past Q&A pairs
2. Show examples matching query type
3. Demonstrate desired output format
4. Include edge case handling examples
5. Update examples based on feedback

---

## Advanced Retrieval Techniques

### 26. Sub-Question Decomposition
For complex query [question]:
1. Decompose into sub-questions
2. Retrieve context for each sub-question
3. Answer sub-questions separately
4. Synthesize sub-answers into final response
5. Show reasoning chain

### 27. Self-RAG (Reflective Retrieval)
Augment RAG with self-reflection:
1. Initial retrieval and answer generation
2. Self-evaluation: Is answer complete/supported?
3. If not satisfied, generate follow-up queries
4. Retrieve additional context
5. Revise answer with new information
6. Repeat until satisfied or max iterations

### 28. Corrective RAG (CRAG)
Implement corrective retrieval:
1. Retrieve initial documents
2. Evaluate relevance of each document
3. If low relevance: trigger web search or knowledge base query
4. Combine retrieved + supplementary sources
5. Generate answer from augmented context

### 29. Fusion-in-Decoding
Multiple retrieval strategies in parallel:
1. Dense retrieval path
2. Sparse retrieval path
3. Knowledge graph path
4. Generate with access to all three contexts
5. Model learns to attend to relevant source

### 30. Speculative RAG
Draft-then-verify approach:
1. Generate draft answer without retrieval (speculative)
2. Identify claims in draft needing verification
3. Retrieve context specifically for claims
4. Verify and correct draft
5. Return verified final answer

---

## Knowledge Graph & Structured RAG

### 31. Knowledge Graph Construction
Build knowledge graph from [documents]:
1. Extract entities (nodes)
2. Identify relationships (edges)
3. Assign properties to nodes/edges
4. Resolve entity references (coreference)
5. Store in graph database
6. Enable graph traversal queries

### 32. GraphRAG Query
Answer using knowledge graph:
1. Parse question for entities and relations
2. Execute graph query (Cypher/GQL)
3. Traverse relevant paths
4. Retrieve connected subgraph
5. Generate answer from graph structure
6. Show reasoning path through graph

### 33. Entity Linking
Link mentions to canonical entities:
1. Identify entity mentions in text
2. Compare to knowledge base entities
3. Disambiguate (context helps)
4. Link to canonical identifier
5. Enrich with KB properties

### 34. Multi-Hop Reasoning
Answer multi-hop questions:
1. Identify starting entity in query
2. First hop: retrieve related entities
3. Second hop: from related to target
4. Connect findings across hops
5. Synthesize multi-hop answer

Example: "What company founded by X's co-founder acquired Y?"

### 35. Temporal Knowledge Graph
Handle time-aware knowledge:
1. Timestamp facts with validity periods
2. Query: "What was true in 2010?"
3. Handle changing relationships over time
4. Reason about temporal order
5. Identify contradictions across time

---

## MCP (Model Context Protocol)

### 36. MCP Server Definition
Define MCP server for [capability]:
```json
{
  "name": "knowledge-base",
  "version": "1.0.0",
  "tools": [
    {
      "name": "search_documents",
      "description": "Search knowledge base",
      "parameters": {
        "query": "string",
        "top_k": "integer"
      }
    }
  ],
  "resources": [
    {
      "name": "document",
      "description": "Full document content",
      "uri": "doc://{id}"
    }
  ]
}
```

### 37. MCP Tool Implementation
Implement MCP tool for [function]:
1. Define input schema (types, required/optional)
2. Implement core logic
3. Handle errors gracefully
4. Return structured output
5. Document side effects
6. Rate limit if needed

### 38. MCP Resource Provider
Provide MCP resources:
1. Define resource URI pattern
2. Implement fetch logic
3. Handle authentication if needed
4. Return appropriate MIME type
5. Support pagination for large resources
6. Cache if appropriate

### 39. MCP Client Prompt
Use MCP server in conversation:
```
You have access to MCP servers:
- filesystem: Read/write files
- git: Repository operations  
- fetch: HTTP requests
- database: SQL queries

When needed, invoke appropriate tools.
Show tool calls and their results.
Integrate tool outputs into responses.
```

### 40. Persistent Memory with MCP
Maintain persistent context:
1. Store conversation summaries in MCP resource
2. Retrieve previous session context on startup
3. Update memory after each interaction
4. Compress old memories into higher-level summaries
5. Enable long-term personalization

---

## Domain-Specific RAG

### 41. Legal Document RAG
Process legal documents:
1. **Citation extraction**: Case law, statutes, regulations
2. **Argument mapping**: Identify claims and support
3. **Precedent analysis**: Similar past cases
4. **Risk assessment**: Potential legal issues
5. **Compliance checking**: Against regulations
6. Maintain confidentiality and privilege awareness

### 42. Medical Literature RAG
Query medical literature:
1. **Evidence grading**: RCT > observational > case report
2. **Conflict of interest**: Flag industry funding
3. **Recency weighting**: Recent research priority
4. **Population matching**: Match patient demographics
5. **Intervention comparison**: Head-to-head when possible
6. **Always include disclaimer**: Not medical advice

### 43. Financial Document RAG
Analyze financial documents:
1. **10-K/10-Q extraction**: Key metrics, risks, MD&A
2. **Earnings call transcript processing**
3. **SEC filing monitoring**
4. **Analyst report synthesis**
5. **Trend identification**: QoQ, YoY comparisons
6. **Risk factor aggregation**

### 44. Technical Documentation RAG
Navigate technical docs:
1. **API endpoint discovery**
2. **Code example retrieval**
3. **Version-specific answers**
4. **Error message lookup**
5. **Configuration guidance**
6. **Troubleshooting pathfinding**

### 45. Academic Research RAG
Synthesize research:
1. **Paper retrieval**: Semantic + keyword
2. **Citation network traversal**
3. **Methodology extraction**
4. **Finding comparison across studies
5. **Research gap identification**
6. **Future work suggestions**

---

## Evaluation & Quality Assurance

### 46. RAG Evaluation Framework
Evaluate RAG system on:
1. **Context relevance**: Retrieved docs relevant to query?
2. **Context recall**: Did we get all needed information?
3. **Faithfulness**: Is answer supported by context?
4. **Answer relevance**: Does it address the question?
5. **Citation accuracy**: Are citations correct?
6. **Hallucination rate**: Unsupported claims?

### 47. Golden Dataset Creation
Create evaluation dataset:
1. Collect representative queries
2. Manually identify relevant documents
3. Write ideal answers
4. Include edge cases and adversarial examples
5. Version and maintain dataset
6. Use for regression testing

### 48. A/B Testing for RAG
Compare RAG configurations:
1. Vary chunking strategies
2. Test embedding models
3. Adjust retrieval parameters
4. Measure on held-out queries
5. Statistical significance testing
6. Deploy winner, document learnings

### 49. Human Evaluation Protocol
Conduct human evaluation:
1. Recruit domain expert judges
2. Define rating rubric (1-5 scale)
3. Blind comparison of systems
4. Measure inter-annotator agreement
5. Collect qualitative feedback
6. Iterate based on findings

### 50. Continuous Monitoring
Monitor RAG in production:
1. Track query distribution
2. Measure latency percentiles
3. Monitor retrieval accuracy
4. Detect drift in query patterns
5. Alert on quality degradation
6. Feedback loop for improvement

---

## Advanced Techniques

### 51. Fine-Tuned Embedding for Domain
Fine-tune embeddings for [domain]:
1. Collect domain-specific corpus
2. Create training pairs (query, relevant doc)
3. Use contrastive learning
4. Evaluate on domain benchmark
5. Deploy fine-tuned model
6. Monitor vs. general embedding performance

### 52. ColBERT Late Interaction
Implement ColBERT:
1. Token-level embeddings for docs
2. Token-level embeddings for queries
3. MaxSim operator for matching
4. Efficient pruning for speed
5. High accuracy with manageable cost

### 53. SPLADE Sparse Retrieval
Use learned sparse retrieval:
1. Expand documents to term importance vectors
2. Expand queries similarly
3. Efficient inverted index lookup
4. Combine with dense for hybrid
5. Better lexical matching than BM25

### 54. Multi-Modal RAG
Retrieve across modalities:
1. **Image-to-text**: Describe images, embed description
2. **Text-to-image**: Retrieve relevant images
3. **Unified embedding space**: CLIP-style
4. **Cross-modal queries**: "Find images like this description"
5. **Fusion**: Combine visual + text evidence

### 55. Conversational RAG
Maintain context across turns:
1. Track conversation history
2. Resolve anaphora ("it", "that")
3. Clarify ambiguous references
4. Retrieve based on full context, not just last turn
5. Handle topic shifts gracefully

---

## System Design & Architecture

### 56. RAG Pipeline Architecture
Design RAG system architecture:
1. **Ingestion pipeline**: Document → Chunks → Embeddings → Index
2. **Query pipeline**: Query → Embedding → Retrieval → LLM → Answer
3. **Storage**: Vector DB + Metadata store
4. **Caching**: Query cache, result cache
5. **Monitoring**: Logs, metrics, tracing
6. **Scaling**: Horizontal scaling strategy

### 57. Real-Time RAG Updates
Handle live document updates:
1. Detect new/updated/deleted documents
2. Incremental embedding computation
3. Index updates without full rebuild
4. Version management for documents
5. Consistency guarantees

### 58. Multi-Tenant RAG
Support multiple tenants:
1. Data isolation per tenant
2. Tenant-specific embeddings
3. Query routing to appropriate index
4. Resource allocation/fairness
5. Tenant-aware caching

### 59. Federated RAG
Query across distributed sources:
1. Query multiple RAG systems
2. Aggregate results
3. Handle different schemas
4. Maintain source attribution
5. De-duplicate across sources

### 60. Edge RAG Deployment
Deploy RAG at edge:
1. Lightweight embedding models
2. Compressed indices
3. On-device inference
4. Sync with central knowledge base
5. Handle offline scenarios

---

## Personal & Life RAG

### 61. Second Brain RAG
Query personal knowledge base:
1. Notes, highlights, bookmarks
2. Conversations and ideas
3. Reading and watch lists
4. Projects and goals
5. Retrieve with personal context

### 62. Journal RAG
Query journal entries:
1. Temporal queries: "What was I thinking in March?"
2. Thematic: "When have I felt this way before?"
3. Pattern: "What do I often struggle with?"
4. Growth: "How has my perspective changed?"
5. Privacy: Local-first, encrypted storage

### 63. Email Archive RAG
Search email history:
1. Semantic search across years of email
2. Contact knowledge: "What did John say about X?"
3. Project tracking: Find all emails about [project]
4. Attachment indexing: Search document contents
5. Thread reconstruction: Follow conversation flow

### 64. Health & Fitness RAG
Query health data:
1. Workout history and patterns
2. Nutrition logs
3. Sleep data
4. Symptom tracking
5. Trends and correlations
6. *With appropriate privacy safeguards*

### 65. Family Knowledge RAG
Family information system:
1. Medical histories
2. Important dates and preferences
3. Recipe collection
4. Home maintenance records
5. Travel history and recommendations

---

## Enterprise RAG

### 66. Enterprise Search RAG
Company-wide knowledge:
1. **Documents**: Policies, procedures, templates
2. **People**: Skills, projects, org chart
3. **Systems**: Documentation, APIs
4. **Conversations**: Slack, Teams, meetings
5. **Access control**: Respect permissions

### 67. Customer Support RAG
Support knowledge base:
1. Past tickets and resolutions
2. Product documentation
3. Known issues and workarounds
4. Escalation paths
5. Customer-specific context

### 68. HR Policy RAG
Employee self-service:
1. Benefits information
2. Time-off policies
3. Expense guidelines
4. Career development paths
5. Compliance requirements

### 69. IT Helpdesk RAG
Technical support:
1. Troubleshooting guides
2. Common error resolutions
3. System status and maintenance
4. Software documentation
5. Asset information

### 70. Sales Enablement RAG
Sales knowledge:
1. Product information
2. Competitive positioning
3. Case studies and testimonials
4. Pricing and contracts
5. Objection handling

---

## Specialized RAG Patterns

### 71. Summary RAG
Retrieve pre-generated summaries:
1. Chunk-level summaries
2. Document-level summaries
3. Collection-level overviews
4. Hierarchical summarization
5. Answer from appropriate summary level

### 72. Table RAG
Query tabular data:
1. Table extraction from documents
2. Structured embedding of rows
3. SQL-like querying over tables
4. Aggregation and computation
5. Visual rendering of results

### 73. Code RAG
Code-aware retrieval:
1. Function/method embeddings
2. AST-aware chunking
3. Import/dependency graph
4. Code example retrieval
5. Documentation lookup

### 74. Multi-Lingual RAG
Cross-lingual retrieval:
1. Unified embedding space
2. Query in language A, retrieve from language B
3. Answer in query language
4. Handle code-switching
5. Translation quality considerations

### 75. Time-Aware RAG
Temporal reasoning:
1. Document timestamps
2. Query time ranges: "What happened in Q2?"
3. Recency weighting
4. Temporal ordering of facts
5. Handle outdated information

---

## Prompt Engineering for RAG

### 76. RAG System Prompt
```
You are a helpful assistant with access to a knowledge base.

Instructions:
1. Base your answers on the provided context
2. Cite sources using [1], [2], etc.
3. If context is insufficient, say so and suggest what would help
4. Synthesize multiple sources when relevant
5. Be precise - don't add information not in context

Context:
{retrieved_documents}

User: {query}
```

### 77. Citation Format Prompt
```
When using information from context:
1. Include citation immediately after claim: [1]
2. Multiple sources: [1][2][3]
3. Specific parts: [1, Section A]
4. List sources at end if many citations
5. Never cite sources not in provided context
```

### 78. Uncertainty Expression Prompt
```
When answering from retrieved context:
- High confidence (directly stated): State clearly
- Medium confidence (inferred): "This suggests..."
- Low confidence (unclear): "The documents mention... but it's unclear if..."
- No information: "The provided context doesn't contain information about..."
```

### 79. Multi-Step RAG Prompt
```
For complex questions requiring multiple lookups:
1. Break question into sub-questions
2. For each sub-question:
   - Identify what needs to be retrieved
   - Retrieve relevant context
   - Answer sub-question
3. Synthesize sub-answers into final response
4. Show your reasoning chain
```

### 80. Constrained Generation Prompt
```
Generate answer with constraints:
1. Use ONLY provided context
2. Maximum length: 200 words
3. Include at least 2 citations
4. No speculation beyond context
5. If multiple perspectives in context, present all
```

---

## Integration & Tools

### 81. LangChain RAG Chain
Build RAG chain:
```python
from langchain import OpenAIEmbeddings, Chroma, RetrievalQA

# Components
embeddings = OpenAIEmbeddings()
vectorstore = Chroma(embedding_function=embeddings)
llm = ChatOpenAI()

# Chain
qa_chain = RetrievalQA.from_chain_type(
    llm=llm,
    retriever=vectorstore.as_retriever(),
    return_source_documents=True
)
```

### 82. LlamaIndex RAG Setup
Initialize RAG with LlamaIndex:
```python
from llama_index import VectorStoreIndex, SimpleDirectoryReader

# Load
documents = SimpleDirectoryReader("data").load_data()

# Index
index = VectorStoreIndex.from_documents(documents)

# Query
query_engine = index.as_query_engine()
response = query_engine.query("Your question")
```

### 83. Haystack Pipeline
Build Haystack pipeline:
```python
from haystack import Pipeline
from haystack.components.retrievers import InMemoryEmbeddingRetriever
from haystack.components.generators import OpenAIGenerator
from haystack.components.builders import PromptBuilder

# Define pipeline
pipeline = Pipeline()
pipeline.add_component("retriever", InMemoryEmbeddingRetriever())
pipeline.add_component("prompt_builder", PromptBuilder(template=...))
pipeline.add_component("llm", OpenAIGenerator())
```

### 84. Vercel AI SDK RAG
Implement RAG with Vercel AI SDK:
```typescript
import { OpenAIStream, StreamingTextResponse } from 'ai';
import { openai } from '@ai-sdk/openai';

// Retrieve context
const context = await retrieveContext(query);

// Stream response
const response = await openai.chat.completions.create({
  model: 'gpt-4',
  messages: [
    { role: 'system', content: `Context: ${context}` },
    { role: 'user', content: query }
  ],
  stream: true
});
```

---

## Security & Privacy

### 85. Data Sanitization
Sanitize documents before indexing:
1. Detect and redact PII (names, SSNs, emails)
2. Remove confidential information
3. Apply data classification labels
4. Role-based access to chunks
5. Audit access to sensitive documents

### 86. Query Sanitization
Sanitize user queries:
1. Detect prompt injection attempts
2. Filter out malicious patterns
3. Rate limiting per user
4. Query logging for audit
5. Anomaly detection

### 87. Access Control in RAG
Enforce permissions:
1. Index with ACL metadata
2. Filter retrieval by user permissions
3. Respect document-level security
4. Audit trail for sensitive queries
5. Automatic redaction of unauthorized content

### 88. Privacy-Preserving RAG
Protect privacy:
1. Differential privacy for queries
2. Homomorphic encryption options
3. Federated retrieval (query local index)
4. On-device embedding computation
5. Secure multi-party computation

---

## Performance Optimization

### 89. Caching Strategy
Implement multi-level caching:
1. **Query cache**: Exact match → instant response
2. **Embedding cache**: Avoid recomputing embeddings
3. **Retrieval cache**: Cache frequent retrievals
4. **LLM cache**: Cache common responses
5. **Cache invalidation**: Handle document updates

### 90. Latency Optimization
Reduce response time:
1. Async embedding computation
2. Parallel retrieval (multiple indices)
3. Streaming responses (first token fast)
4. Approximate retrieval (HNSW)
5. Pre-computed common queries

### 91. Cost Optimization
Reduce RAG costs:
1. Smaller embedding models for retrieval
2. Filter before embedding (keyword pre-filter)
3. Caching at all layers
4. Batching embedding requests
5. Tiered retrieval (cheap first, expensive if needed)
6. Model distillation

### 92. Scalability Patterns
Scale RAG systems:
1. Horizontal index sharding
2. Read replicas for query serving
3. Async document processing
4. Queue-based ingestion
5. Auto-scaling based on load
6. CDN for static embeddings

---

## Evaluation Metrics Deep Dive

### 93. MRR (Mean Reciprocal Rank)
```
MRR = (1/rank of first relevant doc) averaged across queries

Example:
Query 1: Relevant doc at position 2 → 1/2 = 0.5
Query 2: Relevant doc at position 1 → 1/1 = 1.0
Query 3: No relevant docs → 0
MRR = (0.5 + 1.0 + 0) / 3 = 0.5
```

### 94. NDCG (Normalized Discounted Cumulative Gain)
```
DCG = sum(relevance_i / log2(i + 1)) for i = 1 to n
IDCG = ideal DCG (perfect ranking)
NDCG = DCG / IDCG

Accounts for position and graded relevance
```

### 95. Answer Faithfulness Metrics
Measure answer grounding:
1. **Claim extraction**: Break answer into atomic claims
2. **Evidence matching**: Match claims to context
3. **Hallucination detection**: Claims without support
4. **Contradiction detection**: Claims contradicting context
5. **Coverage**: What context claims weren't used

### 96. RAGAS Metrics
Automated RAG evaluation:
- **Faithfulness**: Answer grounded in context?
- **Answer Relevance**: Answer addresses question?
- **Context Precision**: Retrieved context relevant?
- **Context Recall**: All relevant context retrieved?
- **Context Entity Recall**: Entities covered?

### 97. LLM-as-Judge for RAG
Prompt for evaluation:
```
Evaluate this RAG output:
Question: {query}
Context: {retrieved_context}
Answer: {generated_answer}

Score 1-5 on:
1. Accuracy (vs. context)
2. Completeness
3. Relevance to question
4. Citations correct
5. No hallucinations

Explain each score.
```

---

## Future Directions

### 98. Agentic RAG
RAG as agent capability:
1. Agent decides when to retrieve
2. Formulates own queries
3. Evaluates if retrieved info is sufficient
4. Iterates with follow-up retrievals
5. Maintains retrieval history

### 99. Multi-Agent RAG
Specialized retrieval agents:
1. Query understanding agent
2. Retrieval strategy agent
3. Re-ranking agent
4. Synthesis agent
5. Fact-checking agent
6. Orchestrator coordinates

### 100. Adaptive RAG
Self-improving retrieval:
1. Monitor which retrievals led to good answers
2. Learn query → optimal strategy mapping
3. Adjust chunking based on query types
4. Fine-tune embeddings on feedback
5. Continuous improvement loop

### 101. Multimodal RAG
Beyond text:
1. Video retrieval (scene understanding)
2. Audio retrieval (transcript + semantic)
3. 3D model retrieval
4. Code + documentation unified
5. Sensory data (IoT, biometrics)

### 102. Real-Time RAG
Live knowledge:
1. Stream processing of new documents
2. Incremental index updates
3. Sub-second latency from event to retrievable
4. Event-driven notifications
5. Temporal reasoning over streams

### 103. Personalized RAG
User-aware retrieval:
1. User interest modeling
2. Retrieval ranking by user relevance
3. Explanation style adaptation
4. Privacy-preserving personalization
5. Collaborative filtering for documents

### 104. Explainable RAG
Transparent retrieval:
1. Show why documents were retrieved
2. Highlight matching sections
3. Explain re-ranking decisions
4. Visualize embedding similarity
5. Suggest alternative queries

### 105. Interactive RAG
Collaborative retrieval:
1. Clarify ambiguous queries
2. Present retrieved docs for user selection
3. Incorporate user feedback on relevance
4. Iterative refinement
5. Explain trade-offs in strategies

### 106. Robust RAG
Handle adversarial scenarios:
1. Detect poisoned documents
2. Handle contradictory sources
3. Graceful degradation when retrieval fails
4. Conflicting evidence resolution
5. Confidence calibration

### 107. Green RAG
Sustainable retrieval:
1. Energy-efficient embedding models
2. Compressed representations
3. Selective retrieval (don't always query)
4. Caching to reduce computation
5. Carbon-aware scheduling

### 108. Edge-Cloud Hybrid RAG
Distribute intelligence:
1. Edge handles simple queries locally
2. Cloud for complex reasoning
3. Sync knowledge bases efficiently
4. Privacy-sensitive data stays edge
5. Dynamic offloading decisions

### 109. Federated RAG
Privacy-preserving collaboration:
1. Multiple organizations, shared queries
2. Retrieved docs stay local
3. Secure aggregation of answers
4. Differential privacy guarantees
5. No central data repository

### 110. Quantum RAG
Future possibilities:
1. Quantum embedding algorithms
2. Grover's algorithm for search speedup
3. Quantum machine learning for retrieval
4. Quantum-safe encryption for private RAG
5. *Still largely theoretical*

---

**Total: 110+ prompts for building trustworthy, grounded AI systems with RAG and MCP.**
