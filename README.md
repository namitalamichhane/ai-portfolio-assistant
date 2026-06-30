# AI Portfolio Assistant

A chatbot that lets recruiters ask questions about my background, projects, experience, skills, and research instead of reading through a resume and multiple GitHub READMEs manually.

**Live demo:**

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Chat%20with%20Me-6c4ff6?style=for-the-badge&logo=vercel&logoColor=white)](https://portfolio-assistant-project.vercel.app)

---

## The idea

Recruiters spend seconds per resume. A static PDF or a pile of GitHub repos does not let them ask the specific question they actually care about — *"what ML projects has she built?"* or *"does she have SQL experience?"* This assistant answers those questions directly, in my own voice, sourced only from my real portfolio content.

```
Resume + Projects + Experience
            │
            ▼
     Google Doc (live, editable)
            │
            ▼
   Vercel serverless function
   (keeps the AI key secure)
            │
            ▼
      Gemini API (answers)
            │
            ▼
  Recruiter asks a question,
   gets a sourced, first-person
        answer in seconds
```

## What it does

- Chat interface answering only from my real portfolio content — no web browsing, no hallucinated claims
- Every answer ends with a source tag (e.g. *Source: Projects*) so claims are traceable
- Content lives in a single Google Doc — I edit the doc, the assistant reflects the change on the next message, no redeploy needed
- Speaks in first person, as if I'm personally walking the recruiter through my background
- Embedded directly on my portfolio site

## Architecture decisions (and why)

**No vector database.** My first instinct was to build a full RAG pipeline: document loaders, chunk managers, ChromaDB, embeddings. After a few rebuilds I realized that was solving a problem I didn't have. My entire portfolio fits comfortably inside a single LLM context window. So the "knowledge base" is just plain text, fetched live from a Google Doc and passed straight into the prompt. No infrastructure, no token-budget juggling, no framework to maintain — and it answers just as well.

**Google Docs as the CMS.** Instead of building an admin panel or a content pipeline, I publish a Google Doc to the web and fetch it client-side. Editing the doc *is* the update mechanism. Zero deploys for content changes.

**Serverless proxy for the API key.** The first version called the Gemini API directly from the browser, which meant the API key was visible in page source — a real security gap for anything public-facing. The fix was a small Vercel function (`/api/chat.js`) that holds the key server-side as an environment variable and proxies the request. The browser never sees the key.

**Dropped the Job Description Analyzer.** I originally planned a feature comparing job descriptions against my resume. I cut it because it didn't reflect how recruiters actually evaluate candidates — it pushed the product toward gimmicky scoring rather than genuinely useful, conversational answers.

## Stack

- Vanilla HTML/CSS/JS frontend (no framework — kept deliberately simple)
- Google Gemini API for generation
- Vercel serverless function for secure key handling
- Google Docs (published to web) as the live content source

## What I would build next

- Per-section source documents (separate docs for blog posts/research) for more granular citations
- Light analytics on what recruiters actually ask, to learn what to highlight in my portfolio
