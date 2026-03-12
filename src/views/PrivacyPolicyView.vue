<script setup lang="ts">
import { RouterLink } from 'vue-router'
</script>

<template>
  <div class="privacy-page">
    <nav class="navbar">
      <div class="logo">
        <img src="/logo.png" alt="ApplySure Logo" v-if="false" /> <!-- Placeholder for actual logo later -->
        <span class="logo-text">ApplySure</span>
      </div>
      <RouterLink to="/" class="back-link">Back to Home</RouterLink>
    </nav>

    <main class="content">
      <h1>Privacy Policy</h1>
      <p class="last-updated">Last updated: March 12, 2026</p>
      <hr>

      <p class="intro">
        ApplySure ("we", "our", or "the extension") is a Chrome browser extension that helps users
        analyze how well their resume matches a job description on LinkedIn and Naukri.
      </p>

      <section>
        <h2>1. INFORMATION WE COLLECT</h2>
        <div class="subsection">
          <h3>a) Resume Text</h3>
          <ul>
            <li>When you upload a PDF resume, the extension extracts its text content.</li>
            <li>This text is stored locally on your device using Chrome's built-in storage (chrome.storage.local).</li>
            <li>It is never stored on any external server by us.</li>
          </ul>
        </div>
        <div class="subsection">
          <h3>b) Job Description Text</h3>
          <ul>
            <li>The extension reads the job description text from the currently active LinkedIn or Naukri job page you are viewing.</li>
            <li>This text is only used temporarily to perform the analysis and is not stored anywhere.</li>
          </ul>
        </div>
        <div class="subsection">
          <h3>c) No Personal Account Data</h3>
          <ul>
            <li>The extension does not access, collect, or store your LinkedIn or Naukri login credentials, profile information, connections, messages, or any other account data.</li>
          </ul>
        </div>
      </section>

      <section>
        <h2>2. HOW WE USE YOUR INFORMATION</h2>
        <ul>
          <li>Your resume text and the job description text are sent together to an AI model (Google Gemini) via our secure backend proxy hosted on Vercel (https://applysure.vercel.app/api/proxy).</li>
          <li>This is done solely to generate the ATS match score, skill gap analysis, and job-ready resume suggestions.</li>
          <li>We do not log, store, or retain any resume or job description content on our Vercel backend. The proxy forwards your request to the AI API and returns the response immediately.</li>
        </ul>
      </section>

      <section>
        <h2>3. THIRD-PARTY SERVICES</h2>
        <p>The extension communicates with the following third-party AI API services to perform analysis. Their respective privacy policies apply to data processed by them:</p>
        <ul>
          <li>Mistral AI API — <a href="https://mistral.ai/privacy/" target="_blank" rel="noopener noreferrer">https://mistral.ai/privacy/</a></li>
        </ul>
        <p>Only one AI provider is used per request (determined by the backend proxy). Your data is not shared with all providers simultaneously.</p>
      </section>

      <section>
        <h2>4. DATA STORAGE</h2>
        <ul>
          <li>Resume text is stored locally on your device using chrome.storage.local.</li>
          <li>You can delete your stored resume at any time by clicking "Clear Results" or removing the extension.</li>
          <li>We have no access to data stored in your browser's local storage.</li>
        </ul>
      </section>

      <section>
        <h2>5. DATA SHARING</h2>
        <ul>
          <li>We do not sell, trade, or share your personal data with any third parties beyond what is described in Section 3 (AI APIs required for the core feature).</li>
          <li>We do not use your data for advertising or tracking purposes.</li>
        </ul>
      </section>

      <section>
        <h2>6. PERMISSIONS USED</h2>
        <p>The extension requests the following Chrome permissions:</p>
        <ul>
          <li><strong>storage</strong> : To save your resume text locally on your device.</li>
          <li><strong>tabs</strong> : To detect which tab is active and whether it is a supported job page.</li>
          <li><strong>activeTab</strong> : To read the job description from the currently active tab.</li>
          <li><strong>sidePanel</strong> : To display the ApplySure panel alongside the browser window.</li>
        </ul>
        <p>Host permissions are limited to LinkedIn, Naukri, and the AI API endpoints listed above.</p>
      </section>

      <section>
        <h2>7. CHILDREN'S PRIVACY</h2>
        <p>ApplySure is not directed at children under the age of 13. We do not knowingly collect any information from children.</p>
      </section>

      <section>
        <h2>8. CHANGES TO THIS POLICY</h2>
        <p>We may update this Privacy Policy from time to time. Any changes will be reflected with an updated "Last updated" date at the top of this document. Continued use of the extension after changes constitutes acceptance of the revised policy.</p>
      </section>

      <section>
        <h2>9. CONTACT</h2>
        <p>If you have any questions or concerns about this Privacy Policy, please contact us at:</p>
        <p>Email: labhishetttsaikalyan@gmail.com</p>
        <p>Website: <a href="https://applysure-webiste.vercel.app/" target="_blank" rel="noopener noreferrer">https://applysure-webiste.vercel.app/</a></p>
      </section>
    </main>

    <footer class="footer">
      <p>&copy; 2026 ApplySure · contact email</p>
    </footer>
  </div>
</template>

<style scoped>
.privacy-page {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  line-height: 1.6;
  color: #333;
  max-width: 800px;
  margin: 0 auto;
  padding: 0 20px;
}

.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 0;
  border-bottom: 1px solid #eaeaea;
  margin-bottom: 40px;
}

.logo-text {
  font-size: 1.5rem;
  font-weight: bold;
  color: #0066cc;
}

.back-link {
  color: #666;
  text-decoration: none;
}

.back-link:hover {
  text-decoration: underline;
}

.content h1 {
  font-size: 2.5rem;
  margin-bottom: 10px;
}

.last-updated {
  color: #666;
  font-style: italic;
  margin-bottom: 20px;
}

hr {
  border: 0;
  border-top: 1px solid #eaeaea;
  margin: 30px 0;
}

.intro {
  font-size: 1.1rem;
  margin-bottom: 40px;
}

section {
  margin-bottom: 40px;
}

h2 {
  font-size: 1.5rem;
  color: #222;
  margin-bottom: 20px;
  padding-bottom: 10px;
  border-bottom: 1px solid #f0f0f0;
}

h3 {
  font-size: 1.2rem;
  color: #444;
  margin-top: 20px;
  margin-bottom: 10px;
}

ul {
  margin-left: 20px;
  margin-bottom: 20px;
}

li {
  margin-bottom: 8px;
}

a {
  color: #0066cc;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

.footer {
  text-align: center;
  padding: 40px 0 20px;
  color: #666;
  border-top: 1px solid #eaeaea;
  margin-top: 60px;
}
</style>
