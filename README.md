# Contract Clause Checker

An automated document-review pipeline built in n8n. Upload a contract or policy document and receive a structured, risk-flagged summary by email — no manual reading required to get a first pass on what to watch for.

## The Problem

Reading through contracts, NDAs, or internal policies to catch risky or unusual clauses is tedious and easy to get wrong, especially for non-lawyers. This tool automates a first-pass review, flagging language commonly associated with one-sided or high-stakes terms so the reader knows exactly where to focus before signing anything.

## How It Works

1. **Document Intake** — A styled n8n web form collects the uploaded PDF, the user's email, and document type.
2. **Text Extraction** — The PDF's raw text is extracted.
3. **Validity Check** — The workflow verifies real text was extracted (catching scanned/image-based PDFs early) before continuing.
4. **Clause Splitting** — The document is split into individual clauses using pattern matching on numbering and section headers.
5. **Risk Classification** — Each clause is checked against curated high-risk and medium-risk keyword sets and tagged with a plain-language explanation.
6. **Summary Generation** — All classified clauses are aggregated into one structured, color-coded summary.
7. **Email Delivery** — The summary is sent as a styled HTML email directly to the uploader via Gmail (OAuth2).
8. **Error Handling** — Every processing step has a fallback: if extraction, splitting, or classification fails, the uploader still receives a clear notification instead of silence.

## Tech Stack

- **n8n** — workflow orchestration
- **JavaScript** — clause parsing and risk-classification logic
- **Gmail API (OAuth2)** — email delivery
- **n8n Form Trigger** — document intake UI with custom CSS theming

## Design Notes

This project initially explored LLM-based clause classification (Gemini API), but inconsistent free-tier availability — shifting model names, tight rate limits — made it unreliable for a dependable demo. The classification logic was rebuilt as a deterministic, rule-based keyword matcher instead: no API dependency, no rate limits, fully predictable output. This is a natural foundation to layer LLM-based contextual judgment on top of in a future version.

## Setup

1. Import `contract-clause-checker.json` into your own n8n instance
2. Configure a Gmail OAuth2 credential
3. Activate the workflow and open the form trigger URL to test
