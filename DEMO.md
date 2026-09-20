# DEMO.md — 2-Minute Walkthrough

A short script for showing the RAG Security Lab to someone else. Total
time: about 2 minutes.

## 0:00 – Open the lab, set the scene (15s)

Open `rag-security-lab.html`. Say:

> "This is a real RAG pipeline — real chunking, real TF-IDF retrieval,
> real cosine similarity, running entirely in the browser. The only mock
> part is the generation step, which is a rule-based stand-in for an LLM
> API, and it's labeled as such everywhere it shows up."

## 0:15 – Load "The Poisoned FAQ" scenario (25s)

Select **The Poisoned FAQ** from the scenario dropdown and click **Load
Scenario**. Point out:

- The knowledge base now has two documents: the real Support FAQ (trust
  1.0) and a "Community FAQ Edit" (trust 0.3) that quietly claims a
  365-day, no-questions-asked return policy.
- The **Data Poisoning** attack toggle is already on.

Click the suggested query chip, **"What is your return policy?"**, then
click **Run Query**. Point at:

- The retrieval results — the poisoned chunk ranks #1.
- The final response — it states the false 365-day policy.
- The right panel — **Data Poisoning: SUCCEEDED**, in red.

> "Nothing here was scripted to say that — the retriever really ranked
> the false document highest, and the mock LLM really built its answer
> from what it retrieved."

## 0:40 – Turn on a defense (25s)

In the left panel, enable **Chunk Trust Scoring**. Re-run the same query.
Point out:

- The poisoned chunk is now gone from retrieval results entirely — it's
  below the 0.5 trust threshold.
- The response now correctly states the 30-day policy.
- The right panel now shows **Data Poisoning: BLOCKED**, in green.
- The **Defense effectiveness** panel shows the before/after count.

## 1:05 – Show a defense that doesn't fully work (35s)

Load **The Leaky HR Bot** scenario. Run the suggested query, **"What is
the salary information for employees?"** Show the salary figures leaking
into the response, then enable **Output Filtering** and re-run. Point
out:

- The salary figures are now replaced with `[REDACTED — salary figure]`.
- Open the **Learn** tab briefly and point at the Output Filtering
  "Residual Risk" note: *pattern-based filtering can be bypassed by
  rephrasing or reformatting the same information.*

> "This is the honest part of the lab — every defense here has a stated
> limit. None of them is a silver bullet, and the lab doesn't pretend
> otherwise."

## 1:40 – The punchline: layered defense (20s)

Back in the Lab tab, load **The Hijacked Summarizer**, run its suggested
query with no defenses to show the system prompt leaking, then flip on
**Instruction Hierarchy** and re-run to show it get blocked. Close with:

> "Each defense here closes one specific gap. If you enable all four
> attacks and all four defenses at once, at least one attack still gets
> through — because a system prompt, an input filter, and a trust score
> are each solving a different problem. That's the whole thesis of this
> lab: RAG security is a layered-defense problem, not a single-toggle
> problem."

## Optional: hands-on close (if time allows)

Hand over the keyboard. Have the person add their own document with a
low trust score and a deliberately false claim, then ask a query that
should retrieve it, with and without Chunk Trust Scoring on — showing
the pipeline responds to genuinely new input, not just the pre-loaded
scenarios.
