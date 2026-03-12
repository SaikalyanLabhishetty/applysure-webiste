# ApplySure — Landing Page Content Guide

This document contains all the content you need to build the ApplySure landing page.
Use the sections below as copy for your hero, features, how-it-works, FAQ, footer, and privacy policy route.

---

## 1. BRAND

- **Name:** ApplySure
- **Tagline:** Know Before You Apply.
- **Sub-tagline:** Instant ATS match score, skill gap analysis, and a job-ready resume — right inside your browser.
- **Accent color suggestion:** -- prefer blue already used in extension.
- **Target platforms:** LinkedIn · Naukri
- **Use Logo:** get logo from public/logo.png
- **Price:** Free

---

## 2. HERO SECTION

### Headline
> **Stop Guessing. Start Knowing.**

### Sub-headline
> ApplySure analyzes your resume against any job description on LinkedIn or Naukri in seconds.
> Get your ATS match score, see exactly what skills you're missing, and generate a tailored
> job-ready resume — all without leaving the page.

### CTA Button
> **Add to Chrome — It's Free**
> _(link to Chrome Web Store listing)_

### Trust line below CTA
> Works on LinkedIn & Naukri · No sign-up required · Your data never leaves your browser

---

## 3. HOW IT WORKS (3-step section)

### Step 1 — Upload Your Resume
Upload your resume as a PDF once. ApplySure reads the text and saves it securely on your
device. No account, no cloud upload.

### Step 2 — Open Any Job Page
Navigate to any job listing on LinkedIn or Naukri. ApplySure automatically reads the job
description in the background.

### Step 3 — Click "Analyze Match"
Hit the Analyze Match button in the side panel. Within seconds you get:
- Your ATS match score (0–100%)
- A clear Hire / Consider / Reject decision
- A list of missing skills you need to add
- A fully rewritten Job-Ready Resume tailored to that role

---

## 4. FEATURES SECTION

### Feature 1 — ATS Match Score
Get a precise percentage score showing how well your resume matches the job description.
Our AI uses strict domain-matching rules — a backend engineer applying for a frontend role
won't get an inflated score.

### Feature 2 — Skill Gap Analysis
See exactly which skills the job requires that your resume is missing. Stop wondering why
you're not getting callbacks.

### Feature 3 — Job-Ready Resume Generator
ApplySure rewrites your resume to align with the specific job you're applying to —
highlighting the right keywords, skills, and experience the recruiter is looking for.

### Feature 4 — Easy Apply Detection
On LinkedIn, ApplySure detects if the job has Easy Apply enabled and shows you that
information instantly so you can act faster.

### Feature 5 — Works in Your Sidebar
No switching tabs. The extension lives in Chrome's side panel — analyze a job while
viewing it, side by side.

### Feature 6 — 100% Private
Your resume text is stored only on your own device using Chrome's local storage. It is
never saved on any server. Job descriptions are processed in real time and discarded
immediately after analysis.

---

## 5. SUPPORTED PLATFORMS

| Platform  | Job Page Analysis | Easy Apply Detection |
|-----------|:-----------------:|:--------------------:|
| LinkedIn  |        ✅         |          ✅          |
| Naukri    |        ✅         |          —           |

---

## 6. FAQ SECTION

**Q: Is ApplySure free?**
Yes. ApplySure is completely free to use with no subscription or sign-up required.

**Q: Does ApplySure store my resume on a server?**
No. Your resume is stored only on your own device using Chrome's built-in local storage.
It is never uploaded to any external server by us.

**Q: Which job sites does it work on?**
Currently LinkedIn and Naukri. More platforms may be added in future updates.

**Q: What AI model does ApplySure use?**
ApplySure uses Google Gemini 2.0 Flash via a secure backend proxy. The AI processes
your resume and job description only to generate the analysis and immediately discards
the data.

**Q: Does it work on job search listing pages?**
No. Navigate to a specific individual job posting page (not a search results list) and
then click Analyze Match.

**Q: How do I delete my saved resume?**
Click "Replace Resume" or "Clear Results" inside the extension panel at any time.

---

## 7. FOOTER LINKS

- Privacy Policy → `/privacy`
- Chrome Web Store → _(your store link)_
- Contact → _(your email)_
- GitHub → _(optional, your repo link)_

---

## 8. PRIVACY POLICY ROUTE

Add a `/privacy` route to your Vercel app (`applysure.vercel.app/privacy`).

### Page title
`ApplySure — Privacy Policy`

### Content to render on the page
Copy the full text from `privacy_policy.txt` in the extension repository and render it
as a clean, readable HTML page. You can use a simple layout:

```
Navbar: ApplySure logo + "Back to Home" link

Body:
  <h1>Privacy Policy</h1>
  <p>Last updated: March 12, 2026</p>
  <hr>
  ... (paste sections from privacy_policy.txt as <h2> and <p> tags)

Footer: © 2026 ApplySure · contact email
```

### Vercel route setup (api/privacy or pages/privacy)
If using Next.js, create `pages/privacy.jsx` or `app/privacy/page.jsx`.
If using plain HTML on Vercel, create `public/privacy.html` — Vercel will serve it at `/privacy`.

The URL to submit to the Chrome Web Store:
```
https://applysure.vercel.app/privacy
```

---

## 9. SEO META TAGS (for the landing page `<head>`)

```html
<title>ApplySure — ATS Resume Analyzer for LinkedIn & Naukri</title>
<meta name="description" content="ApplySure is a free Chrome extension that instantly analyzes your resume against any LinkedIn or Naukri job. Get your ATS score, skill gaps, and a tailored job-ready resume in one click." />
<meta property="og:title" content="ApplySure — Know Before You Apply" />
<meta property="og:description" content="Free Chrome extension. Instant ATS match score, skill gap analysis, and job-ready resume generator for LinkedIn and Naukri." />
<meta property="og:image" content="/og-image.png" />
<meta property="og:url" content="https://applysure.vercel.app" />
<meta name="twitter:card" content="summary_large_image" />
```

---

## 10. CHROME WEB STORE SHORT DESCRIPTION (132 chars max)

```
Instantly analyze how well your resume matches any LinkedIn or Naukri job — get ATS score, skill gaps, and a tailored job-ready resume in one click.
```

---
