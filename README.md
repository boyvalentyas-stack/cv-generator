# CraftCV — Free ATS-Friendly CV Generator

> **Live site:** [boyvalentyas-stack.github.io/cv-generator](https://boyvalentyas-stack.github.io/cv-generator)

---

## Why I Built This

I built CraftCV because I was struggling to find a job, and every time I tried to update my CV, every tool I found online either required a subscription, asked for my credit card, locked the PDF export behind a paywall, or made me create an account just to download my own resume.

I figured — this is just a form and a PDF. Why should it cost money?

So I built my own. It's free, it doesn't store anything, and it works.

---

## What Is CraftCV

CraftCV is a fully client-side, browser-based CV generator that lets anyone build a professional, ATS-optimised resume in minutes — without signing up, without paying, and without trusting a third party with their personal information.

Everything runs in the browser. No backend. No database. No tracking.

---

## Features

### Core
- **ATS-Optimised PDF output** — Generated using jsPDF as pure vector text (not a screenshot), ensuring every word is machine-readable by Applicant Tracking Systems
- **Live CV preview** — See your resume rendered in real time before generating the PDF, with the option to go back and edit
- **Dynamic multi-entry forms** — Add unlimited work experience and education entries, each individually collapsible and deletable
- **Email a copy to yourself** — Optionally receive a formatted HTML summary of your CV inputs via EmailJS, sent directly from your browser
- **Zero data retention** — All form data lives exclusively in browser `sessionStorage` and is automatically cleared when the tab is closed. No server ever receives your CV

### User Experience
- **Bilingual interface** — Full English and Indonesian (Bahasa Indonesia) support with instant one-click switching. All labels, placeholders, error messages, and CV section headings switch language without losing any entered data
- **Auto-save within session** — Form progress is automatically saved to `sessionStorage` on every field change, so navigating between pages does not lose your work
- **Step-by-step navigation** — A clear multi-step flow (Fill → Preview → Generate) with a sticky sidebar and progress bar on the form page, and a breadcrumb step indicator on preview and generate pages
- **Per-step form validation** — Each section validates before advancing, with inline field errors and toast notifications
- **Chip-input skills** — Technical skills, soft skills, languages, and certifications are entered as interactive chips (press Enter or comma to add, backspace or × to remove, paste a comma-separated list to bulk-add)
- **Responsive design** — Fully usable on desktop and mobile. The form sidebar collapses to a scrollable tab strip on narrow screens

### Privacy
- No cookies
- No analytics
- No accounts
- No third-party data collection beyond EmailJS (used only when the user explicitly triggers an email send)

---

## How It Works

```
Landing Page → Fill CV Form → Preview CV → Generate PDF → Download
                                                       ↘ Email Copy (optional)
```

| Step | Page | What happens |
|---|---|---|
| 1 | `index.html` | Landing page — explains the tool and links to the form |
| 2 | `form.html` | 6-section form: Personal Info, Summary, Experience, Education, Skills, Extras & Email |
| 3 | `preview.html` | Live CV render with ATS checklist, word count, and score panel |
| 4 | `generate.html` | PDF generation + optional email send |

---

## Form Sections

The CV form is divided into six sections, navigable via sidebar (desktop) or tab strip (mobile):

| # | Section | Fields |
|---|---|---|
| 1 | **Personal Info** | First name, last name, professional title, email, phone, city, country, LinkedIn, website, GitHub, nationality |
| 2 | **Professional Summary** | Free-text summary (600 character limit with live counter) |
| 3 | **Work Experience** | Job title, company, location, employment type, start/end dates, "currently working here" toggle, description (800 char limit) — unlimited entries |
| 4 | **Education** | Degree, field of study, institution, location, GPA, start/end years, "currently studying" toggle, honours/achievements — unlimited entries |
| 5 | **Skills** | Technical skills, soft skills, languages, certifications — all chip-input |
| 6 | **Extras & Email** | Key achievements, references, optional email copy opt-in |

---

## Technology Stack

| Component | Technology |
|---|---|
| Frontend | Vanilla HTML, CSS, JavaScript — no frameworks |
| Fonts (UI) | DM Serif Display, DM Sans — via Google Fonts |
| Fonts (CV / PDF) | EB Garamond (body, name, role), DM Sans (labels, meta) |
| PDF generation | [jsPDF 2.5.1](https://github.com/parallax/jsPDF) — vector text, no canvas/screenshot |
| Email delivery | [EmailJS Browser SDK v4](https://www.emailjs.com) — client-side only |
| Hosting | GitHub Pages |
| Data storage | Browser `sessionStorage` only — cleared on tab close |

---

## Project Structure

```
cv-generator/
├── index.html            # Landing page
├── form.html             # 6-step CV form
├── preview.html          # Live CV preview + ATS panel
├── generate.html         # PDF generation + email
├── emailjs_template.html # HTML email template (paste into EmailJS dashboard)
├── cv-generator-favicon.ico
└── README.md
```

---

## EmailJS Setup (for the Email Feature)

The email feature sends a formatted copy of the user's CV directly from their browser using [EmailJS](https://www.emailjs.com). No server is involved. To enable it:

### 1. Create a free EmailJS account
Go to [emailjs.com](https://www.emailjs.com) — the free plan allows 200 emails/month, no credit card required.

### 2. Connect a Gmail service
**Email Services → Add New Service → Gmail** — connect the Gmail account you want to send from. Copy the **Service ID** (e.g. `service_xxxxxxx`).

### 3. Create an email template
**Email Templates → Create New Template** — configure the following:

| Field | Value |
|---|---|
| Subject | `Your CV Summary — {{to_name}}` |
| To Email | `{{to_email}}` |
| Reply To | `{{reply_to}}` |

Switch to the **HTML editor** and paste the full contents of `emailjs_template.html` into the content area.

The template uses these four variables sent from the app:

| Variable | Content |
|---|---|
| `{{to_name}}` | Recipient's full name |
| `{{to_email}}` | Recipient's email address |
| `{{cv_role}}` | Job title / desired role |
| `{{cv_full_text}}` | Complete plain-text CV summary |
| `{{personal_note}}` | Optional personal note from the user |
| `{{reply_to}}` | CV owner's email address |

Copy the **Template ID** (e.g. `template_xxxxxxx`).

### 4. Get your Public Key
**Account → General → API Keys** — copy the **Public Key**.

### 5. Done
The credentials are already configured in `generate.html`. If you fork this project and want to use your own EmailJS account, replace the three constants near the top of the `sendEmail()` function in `generate.html`:

```javascript
const EJS_SERVICE  = 'your_service_id';
const EJS_TEMPLATE = 'your_template_id';
const EJS_KEY      = 'your_public_key';
```

---

## PDF Output Specification

| Property | Value |
|---|---|
| Format | A4 (210 × 297 mm) |
| Margins | 20 mm all sides |
| Generation method | jsPDF vector text — not a screenshot |
| Body font | EB Garamond Regular / Bold / Italic |
| Label font | DM Sans Regular / Bold |
| ATS compatibility | High — single column, standard headings, no tables, no text boxes, machine-readable fonts |

The PDF is generated entirely in the browser. The file is downloaded directly to the user's device and is never uploaded to any server.

### Typography hierarchy in the generated PDF

| Element | Size | Style | Color |
|---|---|---|---|
| Name | 26 pt | EB Garamond Bold | `#111111` |
| Job title | 12 pt | EB Garamond Italic | `#444444` |
| Contact line | 9 pt | DM Sans Regular | `#333333` |
| Section title | 8.5 pt | DM Sans Bold, ALL-CAPS | `#111111` |
| Item title (job/degree) | 10.5 pt | DM Sans Bold | `#111111` |
| Date | 8.5 pt | DM Sans Regular | `#999999` |
| Company / institution | 10 pt | EB Garamond Italic | `#444444` |
| Body / bullet text | 10–10.5 pt | EB Garamond Regular | `#222222` |
| Skill group labels | 8.5 pt | DM Sans Bold | `#333333` |

---

## Local Development

This project has no build step and no dependencies to install. Clone and open:

```bash
git clone https://github.com/boyvalentyas-stack/cv-generator.git
cd cv-generator
```

Then open `index.html` in any modern browser. All four pages link to each other via relative paths.

> **Note:** The email feature requires a live EmailJS connection. It will not work when opened as a local file (`file://`) in some browsers due to CORS restrictions on the EmailJS SDK. Use a local dev server (e.g. VS Code Live Server, `npx serve`, or `python3 -m http.server`) if you need to test the email feature locally.

---

## Deployment

The site is deployed via **GitHub Pages** with no additional configuration. The `main` branch root serves directly as the site root.

To deploy your own fork:
1. Fork the repository
2. Go to **Settings → Pages**
3. Set source to **Deploy from a branch → main → / (root)**
4. The site will be live at `https://your-username.github.io/cv-generator`

---

## Privacy & Data Handling

| What | How it's handled |
|---|---|
| Form data | Stored only in browser `sessionStorage`. Never sent to any server. Cleared when the tab is closed. |
| PDF generation | Runs 100% in the browser via jsPDF. The file is never uploaded anywhere. |
| Email sending | Uses the EmailJS client-side SDK. Data is sent directly from the browser to the EmailJS API — not through any server this project controls. |
| Analytics | None. No Google Analytics, no tracking pixels, no cookies. |
| Accounts | None required. No login, no sign-up. |

This project was built with the explicit intention that no visitor should ever need to trust it with their personal data. The browser `sessionStorage` guarantee is the technical enforcement of that promise.

---

## Known Limitations

- **No PDF attachment in email** — EmailJS free tier does not support file attachments. The email sends a plain-text + HTML summary of your CV inputs, not the PDF file itself. Download the PDF separately.
- **Session data only** — If you close the tab or the browser crashes before downloading, your entered data is lost. There is no persistent storage by design.
- **Font loading** — The PDF generator fetches EB Garamond and DM Sans from Google Fonts CDN at generation time. If you are offline or Google Fonts is unreachable, the PDF will fall back to Times New Roman and Helvetica.
- **Email feature requires EmailJS credentials** — The email toggle will not work without a valid EmailJS Service ID, Template ID, and Public Key configured in `generate.html`.

---

## License

This project is open source and free to use. Built as a personal portfolio project.

---

## Author

**Boy Valentyas**
- GitHub: [github.com/boyvalentyas-stack](https://github.com/boyvalentyas-stack)
- Portfolio: [boyvalentyas-stack.github.io](https://boyvalentyas-stack.github.io)
- LinkedIn: [linkedin.com/in/boy-valentyas-indra-gunara-86a9ba181](https://linkedin.com/in/boy-valentyas-indra-gunara-86a9ba181/)
