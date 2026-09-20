# RAG Security Lab

An interactive, browser-based lab for learning how Retrieval-Augmented Generation (RAG) systems can be attacked — and defended — through hands-on experimentation with a real retrieval pipeline.

RAG Security Lab teaches RAG security by doing: toggle real attacks, run them through a real TF-IDF retrieval pipeline, watch a malicious chunk actually surface in the results, then toggle defenses and see what gets blocked, partially mitigated, or still gets through.

- **Live Demo:** https://muhammad-usman-cyber.github.io/rag-security-lab/

## Overview

RAG Security Lab is a browser-based interactive learning environment designed to help users understand RAG-specific attack surface through hands-on exercises.

Instead of only reading about prompt injection or data poisoning in the abstract, users toggle real attacks on and watch a live retrieval pipeline actually pull manipulated content into its results — then toggle defenses on and see, honestly, what each one does and doesn't fix.

The lab is built around one core idea: **a RAG system's retrieval step creates a new trust boundary.** Anything in the knowledge base — including content an attacker managed to get ingested — is treated as legitimate context by the generation step, unless something explicitly stops it.

## Key Features

- 4 implemented attacks: Data Poisoning, Retrieval Manipulation, Indirect Prompt Injection, Sensitive Information Disclosure
- 4 implemented defenses: Chunk Trust Scoring, Instruction Hierarchy, Output Filtering, Input Sanitization
- Real, live TF-IDF vectorization and cosine similarity retrieval — not scripted or pre-written results
- Live attack status (blocked/succeeded) computed from actual pipeline execution
- Defense-effectiveness metric comparing current defenses against none
- Editable knowledge base — add documents, set trust scores, adjust chunk size/overlap and top-k
- Before/Current comparison toggle on retrieval results
- 3 pre-built scenarios: The Poisoned FAQ, The Leaky HR Bot, The Hijacked Summarizer
- In-app Learn tab mapping each attack/defense to the OWASP LLM Top 10
- Running activity log of everything the pipeline does
- No account or signup required
- No external AI API required
- Fully browser-based static application

## How It Works

The learning process follows a simple cycle:
```text
Toggle an Attack
       ↓
  Run a Query
       ↓
See the Real Pipeline Respond
       ↓
 Toggle a Defense
       ↓
Re-run the Same Query
       ↓
Compare Before/After
       ↓
Read the Residual Risk
```


This makes the application focused on experimentation rather than passive learning.

## Attacks & Defenses

The lab implements four attack classes and four corresponding defenses.

| Attack | What It Does |
|---|---|
| Data Poisoning | A false-information document gets into the knowledge base and ranks as high as, or higher than, the correct one |
| Retrieval Manipulation | A document stuffed with query-relevant keywords artificially inflates its similarity score |
| Indirect Prompt Injection | A retrieved document contains an embedded instruction the generation step may treat as a command |
| Sensitive Information Disclosure | PII or secrets in the knowledge base get retrieved and surfaced in a response |

| Defense | What It Does |
|---|---|
| Chunk Trust Scoring | Excludes chunks below a trust threshold from retrieval before ranking happens |
| Instruction Hierarchy | Separates system instructions from retrieved content at generation time |
| Output Filtering | Scans generated responses for sensitive-looking patterns and redacts matches |
| Input Sanitization | Strips known injection-trigger phrases from the user's query before retrieval |

## Residual Risk

Every defense includes an explicit, honest limitation rather than being presented as a complete fix. A full breakdown of attack vector, defense, and residual risk for each threat is in [`THREAT_MODEL.md`](./THREAT_MODEL.md) — every claim in it is reproducible in the app via the live "Defense effectiveness" metric.

**Key takeaway:** enable all four attacks with no defenses and each succeeds against a relevant query. Enable all four defenses and re-run the same queries — in most configurations, at least one attack still gets through, because no single defense here addresses every attack class. That's the central lesson: RAG security requires layered, complementary defenses, not any single control.

## What's Real, and What's Simulated

- **Retrieval is real.** The lab implements actual TF-IDF vectorization and cosine similarity search, computed live against whatever documents are currently in the knowledge base — a genuine information retrieval pipeline, not a lookup table of pre-written answers.
- **Generation is a labeled mock.** The "LLM" is a deterministic, rule-based generator that builds an extractive answer from whatever chunks were actually retrieved. Every response is labeled **"Mock LLM Response — rule-based, not a real model"** so this is never ambiguous.

## Mapping to the OWASP LLM Top 10

| OWASP Category | Demonstrated by |
|---|---|
| LLM01: Prompt Injection | The Indirect Prompt Injection attack |
| LLM02: Insecure Output Handling | The Output Filtering defense |
| LLM06: Sensitive Information Disclosure | PII surfacing in a response because it was retrievable |
| LLM03-adjacent: Poisoning | The Data Poisoning attack and Chunk Trust Scoring defense |

## Technology

RAG Security Lab is implemented as a standalone browser application using:

- HTML
- CSS
- JavaScript
- Client-side TF-IDF retrieval and cosine similarity

The application does not require a backend server or external AI API for its core functionality.

## Development Approach

This project was built through iterative AI-assisted development, using prompt engineering as the primary way of communicating requirements, refining the interface, implementing the retrieval pipeline, and debugging issues.

### The Development Loop

```text
Idea
  ↓
Prompt
  ↓
AI-generated implementation
  ↓
Test
  ↓
Identify problems
  ↓
Refine the prompt
  ↓
Improve the implementation
  ↓
Repeat
```


## Why I Built This

RAG is one of the most common ways LLMs get deployed in real applications, and its retrieval step introduces a trust boundary that's easy to overlook. I wanted to move past reading about that risk and build a pipeline where I could trigger it, observe it, and then implement — and honestly evaluate — defenses against it.

This project connects my growing AI Security work with core cybersecurity concepts like trust boundaries, least privilege, and layered defense, and follows directly from what I learned building Prompt Lab and Prompt Injection Defense Lab.

## AI Security Relevance

This project provides practical exposure to:

- RAG-specific attack surface
- Data poisoning and retrieval manipulation
- Indirect prompt injection via retrieved documents
- Instruction hierarchy and instruction/data separation
- Output filtering and sensitive data handling
- Defense-in-depth for AI-powered applications

## Project Limitations

- The scoring and detection logic is intentionally simplified for teaching, not a production RAG security scanner
- No real embedding model or LLM — retrieval is TF-IDF, generation is rule-based
- Limited attack coverage — does not model embedding-specific attacks, vector-store access control, multi-turn context manipulation, or tool-use chaining
- All documents and scenarios are fictional

## Future Improvements

- Additional attack and defense types
- Embedding-based retrieval as an alternative to TF-IDF
- More granular defense-effectiveness analytics
- Additional pre-built scenarios
- Exportable session results

## What I Learned

- **Retrieval introduces a new trust boundary.** Anything in the knowledge base is treated as legitimate context unless something explicitly stops it.
- **No single defense is sufficient.** Each defense here closes one specific gap; layered, complementary controls are what actually reduce risk.
- **Trust scoring has limits.** Trust assigned once, at ingestion, doesn't catch a compromised source that was already marked trusted.
- **Pattern-based defenses can be bypassed.** Output filtering and instruction hierarchy are pattern-based and can miss rephrased or unusually-worded attacks.
- **AI-generated output still requires validation.** Every claim the lab makes about attack success or defense effectiveness is computed live from the pipeline, not hardcoded.

## Project Structure
```text
rag-security-lab/
├── index.html
├── README.md
├── THREAT_MODEL.md
└── DEMO.md
```


The application is contained in a standalone HTML file, making it simple to run and deploy.

## Running Locally

No build system or package installation is required.

Clone the repository:
```bash
git clone https://github.com/Muhammad-Usman-cyber/rag-security-lab.git
```
Enter the project directory:

```bash
cd rag-security-lab
```


Open `index.html` in a modern web browser. The application runs entirely on the client side.

## Live Demo

RAG Security Lab:
https://muhammad-usman-cyber.github.io/rag-security-lab/

## Project Repository

GitHub:
https://github.com/Muhammad-Usman-cyber/rag-security-lab

## Part of My AI Security Journey

RAG Security Lab is an independent project within my broader AI Security learning journey, following Prompt Lab and Prompt Injection Defense Lab.

AI Security Journey:
https://github.com/Muhammad-Usman-cyber/ai-security-journey

## Author

Muhammad Usman
Cybersecurity student building a foundation in:

- Security Operations
- Penetration Testing
- Web Application Security
- Vulnerability Research
- AI Security

My current direction is to build on my cybersecurity foundation and develop practical expertise in securing AI-powered systems.
