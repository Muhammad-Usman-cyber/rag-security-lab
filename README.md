# RAG Security Lab

An interactive, browser-based lab for learning how Retrieval-Augmented
Generation (RAG) systems can be attacked — and defended — through hands-on
experimentation with a real retrieval pipeline.

## How to run it

No installation, build step, or API key required.

1. Download `rag-security-lab.html`.
2. Open it in any modern browser (Chrome, Firefox, Edge, Safari), or push it
   to a GitHub repository and enable **GitHub Pages** — the whole app is a
   single self-contained HTML file with embedded CSS and JavaScript.

That's it. Everything — document storage, chunking, embedding, retrieval,
and generation — runs entirely client-side in your browser.

## What this teaches

This lab is built around one core idea: **a RAG system's retrieval step
creates a new trust boundary.** Anything in the knowledge base — including
content an attacker managed to get ingested — is treated as legitimate
context by the generation step, unless something explicitly stops it.

You'll learn this by actually doing it, not just reading about it:

- Toggle an **attack** on and watch a real (not scripted) retrieval
  pipeline pull a malicious chunk into the top results.
- Toggle a **defense** on and watch the same attack get blocked, partially
  mitigated, or — sometimes — still get through, with an honest explanation
  of why.
- Add your own documents, change the chunk size, or write your own queries
  to see how retrieval behavior actually changes.

### What's real, and what's simulated

- **Retrieval is real.** The lab implements actual TF-IDF vectorization and
  cosine similarity search, computed live in your browser against whatever
  documents are currently in the knowledge base. This is a genuine (if
  simple, embedding-model-free) information retrieval pipeline — not a
  lookup table of pre-written answers.
- **Generation is a labeled mock.** Per the "no paid APIs required"
  constraint, the "LLM" is a deterministic, rule-based generator that
  builds an extractive answer from whatever chunks were actually
  retrieved, and applies simple pattern-matching to decide whether to
  "obey" an embedded instruction. Every response is labeled **"Mock LLM
  Response — rule-based, not a real model"** so this is never ambiguous.

## Using the lab

- **Lab tab:** the main interactive environment.
  - **Left panel:** manage the knowledge base (add/remove documents, set a
    trust score for new ones), configure chunking (size/overlap) and
    top-k, and toggle the four attacks and four defenses.
  - **Center panel:** enter a query (or click a suggested one), see the
    ranked retrieval results with real similarity scores, and see the
    final generated response. A "Before / Current" toggle lets you compare
    the response with attacks off vs. as currently toggled.
  - **Right panel:** live attack status (blocked/succeeded, computed from
    the actual pipeline run — never hardcoded), a defense-effectiveness
    metric comparing your current defenses against no defenses at all, and
    a running activity log of everything the pipeline did.
- **Learn tab:** background on RAG, how each attack and defense works and
  its limitations, and how this maps to the OWASP LLM Top 10 (below).
- **Load Sample Scenario:** three pre-built walkthroughs — The Poisoned
  FAQ, The Leaky HR Bot, and The Hijacked Summarizer — each loading the
  relevant documents, attack toggle, and a suggested query.
- **Reset Lab:** clears all documents, toggles, and the activity log back
  to the default baseline knowledge base.

## Mapping to the OWASP LLM Top 10

| OWASP Category | Demonstrated by |
|---|---|
| **LLM01: Prompt Injection** | The Indirect Prompt Injection attack — a hidden instruction embedded in a retrieved document. |
| **LLM02: Insecure Output Handling** | The Output Filtering defense (and what happens without it) — whether a response is validated before being returned. |
| **LLM06: Sensitive Information Disclosure** | The Sensitive Information Disclosure attack — PII surfacing in a response because it was retrievable. |
| **LLM03-adjacent: Poisoning** | The Data Poisoning attack and the Chunk Trust Scoring defense, adapted to a RAG corpus rather than training data. |

See the in-app **Learn** tab for a fuller explanation of each of these,
including real-world context for indirect prompt injection research.

## Design principles behind this lab

- **Educational first.** Every toggle is meant to teach something specific,
  and every result is computed from the real pipeline state, not
  pre-written per scenario.
- **Honest about limitations.** Every defense includes an explicit
  "Residual Risk" note. None of the four defenses is presented as a
  complete solution, and combining all four still doesn't guarantee every
  attack fails — see `THREAT_MODEL.md`.
- **Hands-on.** You can add your own documents, change chunking
  parameters, write arbitrary queries, and see the retrieval and
  generation results change immediately.
- **Self-contained.** No paid APIs, no server, no build step, no
  dependencies beyond a browser.

## Related files

- `THREAT_MODEL.md` — a table of each threat, its attack vector, the
  defense implemented here, and its residual risk.
- `DEMO.md` — a short, scripted 2-minute walkthrough for showing this lab
  to someone else.
