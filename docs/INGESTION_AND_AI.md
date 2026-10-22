# Ingestion and AI strategy

## Default behaviour

Capture and sync are not the same as AI ingestion.

- Saving an item creates a minimal record immediately.
- Obsidian sync parses Markdown, frontmatter, tags, wiki-links and headings deterministically.
- No LLM is called automatically.
- Optional non-LLM suggestions may be displayed.
- Full-text extraction, chunking, embeddings and LLM processing are initiated only by an explicit user action or an explicit batch action started by the user.

## Progressive processing

### Capture only
Store title, source URL/path, capture time, reason for saving, source type if known, and user notes. If a page is unavailable, preserve the URL and make the extraction state clear.

### Review / lightweight analysis
On request, extract readable text where permitted, produce a concise summary, identify terminology, and propose tags/topics. Show the source text available to the processor and indicate extraction limitations.

### Deep processing
On request, split text by headings and semantic sections; create versioned chunks and embeddings; extract candidate entities and relationships against a controlled schema; attach evidence chunk IDs; validate output with Pydantic; and queue ambiguous results for review.

## Invalidation and caching

- Hash raw source content.
- Cache derived output using source/chunk hash, operation name, prompt/schema version, provider and model.
- Skip processing when the cache key matches.
- When source content changes, mark derived outputs stale and regenerate only those required by a future request.
- Limit retries and record failures.
- Keep ingestion idempotent.

## Non-LLM suggestions

Use deterministic rules and spaCy where useful:
- exact tag matching and normalized aliases
- title/domain-based source classification
- keyword and noun-phrase suggestions
- named entities and candidate concepts from spaCy
- fuzzy title matching for possible duplicates
- PostgreSQL full-text search

These are suggestions, not truth. Avoid automatic creation or merging of canonical topics when ambiguity exists.

## User-triggered AI actions

Initial actions to consider:
- Summarise this source
- Extract key points / terms
- Suggest tags and topics
- Find possible connections
- Compare selected sources
- Explain a concept using selected sources
- Prepare a project resumption brief
- Propose actions from a note or project

Present generated results for review. Do not overwrite personal notes or confirmed facts.

## Retrieval and context budget

- Route straightforward lookups to SQL/full-text search without an LLM.
- Use vector retrieval for semantic discovery when available.
- Expand one or two relationship hops for connection questions.
- Use stored section summaries for broad questions when available.
- Retrieve candidate passages, then narrow to a small evidence set.
- Include provenance and source links in answers.
- Apply explicit token/context budgets; do not send the whole vault by default.
- Cache repeatable derived results and invalidate when source versions change.

## Models and observability

Use a provider abstraction with Google as the initial provider through LangChain. Keep model names and API keys in environment configuration. Separate inexpensive extraction tasks from complex synthesis tasks, but choose models based on measured quality/cost, not assumption.

LangSmith tracing is optional and controlled by environment. Document that prompts, retrieved content and model outputs may be sent to the configured tracing service; never enable tracing silently for sensitive data. Record application-level token counts, latency, cost where available, cache hits, model, prompt/schema version and outcome.

## Evaluation before optimisation

Create a small set of representative questions with expected source passages. Measure retrieval correctness, unsupported claims, latency and token use before adopting a graph framework or complex routing. Use PostgreSQL relational queries and pgvector first; add a dedicated graph database only if measured needs justify it.
