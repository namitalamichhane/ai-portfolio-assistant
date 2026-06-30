# Namita's Portfolio Assistant

A recruiter-facing AI chatbot that answers questions about Namita's resume, projects, and experience — sourced live from a Google Doc, with the Gemini API key kept secure server-side.

## How it works
- `index.html` — the chat UI. Auto-loads on page visit (no setup screen for visitors).
- `api/chat.js` — a Vercel serverless function that holds your Gemini API key secretly and proxies requests to Google. Your key is never exposed in the browser.

## Deploy to Vercel (free, ~5 minutes)

1. Go to **vercel.com** → sign up (free, GitHub login works)
2. Click **"Add New" → "Project"**
3. Choose **"Deploy without Git"** or drag this whole folder (containing `index.html` and the `api` folder) onto the upload area
4. Before/after deploying, go to **Project Settings → Environment Variables**
5. Add a new variable:
   - **Name:** `GEMINI_API_KEY`
   - **Value:** your Gemini key (starts with `AIza` or `AQ.`) from aistudio.google.com
6. Click **Save**, then **redeploy** (Settings → Deployments → "..." → Redeploy) so the function picks up the new variable
7. Your live URL will look like `your-project.vercel.app` — visit it to confirm the chat loads automatically

## Updating your portfolio content
Just edit your Google Doc — the chatbot re-fetches it on every message. No redeploy needed.

## Embedding on Lovable
```html
<iframe
  src="YOUR-VERCEL-URL"
  width="100%"
  height="650px"
  style="border:none; border-radius:16px;"
></iframe>
```

## Security note
The Gemini API key lives only in Vercel's environment variables and is read by `api/chat.js` on the server. It is never sent to or visible in the recruiter's browser, view-source, or network tab.
