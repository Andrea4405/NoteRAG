NoteRAG — Retrieval-Augmented Notes Q&A

A small RAG (Retrieval-Augmented Generation) tool: paste your own notes,
ask a question, and watch it find the relevant passages first —
then answer strictly from what it found, with inline citations back
to the source excerpts.

Built to make the RAG pipeline visible, not hidden behind a single
chat box: chunking, relevance scoring, and retrieval all show up as
distinct steps before the model ever generates an answer.

Why RAG, and why it matters

Most "AI demo" projects are a single prompt → single response. RAG is
different: instead of letting the model answer purely from memory (and
risk hallucinating), the system first retrieves the most relevant
source material and restricts the model to answering only from that
material. This is the same architectural pattern behind production
tools like codebase-aware AI assistants — find the right context first,
then reason over it.

How it works


Chunk — notes are split into independent passages (paragraph
breaks)
Score — each passage is scored for relevance against the
question's keywords
Retrieve — only the top-scoring passages are kept; everything
else is discarded
Generate — the retrieved excerpts (and only those) are sent to
Claude with instructions to answer strictly from them and cite
which excerpt supports each claim, or say "not enough information"
rather than guess


Stack


HTML / CSS / vanilla JavaScript (no framework, no build step)
Anthropic Claude API (claude-sonnet-4-6) for grounded answer generation
Custom keyword-overlap relevance scoring for retrieval


Honest note on the retrieval method

Production RAG systems retrieve passages using vector embeddings
(semantic similarity), not keyword matching. This build uses a
transparent keyword/term-overlap score instead, as a same-day,
dependency-free stand-in. The pipeline shape — chunk → retrieve →
ground → generate — is the same; only the similarity metric is
simplified. A production version would swap the scoring step for
an embeddings-based vector search (e.g. via a vector DB) without
changing the rest of the architecture.

Note on deployment

This runs as a Claude.ai artifact, which proxies the API call — it
isn't a standalone deployed app. A production version would route
the request through a backend to securely hold the API key and would
add a real embeddings-based retrieval layer.
