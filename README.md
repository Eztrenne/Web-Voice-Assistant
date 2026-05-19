<div align="center">

# 🎙️ Web Voice Assistant
### AI Conversational Assistant · Voice · Screen Vision

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![Cloudflare Workers](https://img.shields.io/badge/Cloudflare%20Workers-F38020?style=for-the-badge&logo=cloudflare&logoColor=white)
![Status](https://img.shields.io/badge/Status-Live-00FF87?style=for-the-badge)

*A browser-based voice assistant: speak naturally, get spoken answers, and — on desktop — let it use your screen as context to better understand what you're asking.*

---

**[🧩 Overview](#-overview) · [🏗️ Architecture](#️-architecture) · [⚙️ Configuration](#️-configuration) · [🛰️ The Proxy](#️-the-serverless-proxy) · [🚀 Deployment](#-deployment) · [⚠️ Limitations](#️-known-limitations) · [📬 Contact](#-contact)**

</div>

---

## 🧩 Overview

A **single-page application** (one HTML file, no framework, no build step) that delivers a voice-driven conversational assistant powered by an AI model.

The user speaks, the system transcribes the voice, sends the query to a language model through a secure *proxy*, and returns the answer as speech. On desktop, it can optionally capture a single screen frame to give the model visual context.

| Capability | Description |
|---|---|
| 🗣️ Voice conversation | Browser-native speech recognition and synthesis |
| 🤖 AI responses | Requests routed to a language model via proxy |
| 🖥️ Visual context | One-shot screen capture as support (desktop only) |
| 🔐 Restricted access | Login validated against an external user list |
| 🪶 Zero dependencies | HTML + CSS + JS in a single file, no compilation |
| 🌗 Light/dark theme | Dark mode and compact view included |

---

## 🏗️ Architecture
<img width="1286" height="718" alt="image" src="https://github.com/user-attachments/assets/2af1d285-3cd6-475b-82c7-18727764be9b" />

```
   Browser
      │  voice, UI, one-shot screen capture
      ▼
   Serverless proxy  ──►  AI model API
      │  injects the secret key · validates origin & secret
      │
      └── (in parallel) Published spreadsheet (CSV) → user list
```

- The **browser** serves the UI and handles microphone, voice, and a single screen-frame capture at the moment of asking.
- A **serverless proxy** receives requests, checks the origin and a shared secret, injects the API key (stored **server-side only**), and forwards to the AI provider. The key **never reaches the client**.
- The **user list** is read from a spreadsheet published as CSV: access control is managed without touching the code or redeploying.

> 💡 **Security note:** this pattern protects the API key and adds an access barrier. It is not equivalent to real backend authentication — being a client-side app, the downloaded user list is inspectable from the browser. Suitable for controlled environments, not for sensitive data.

---

## ⚙️ Configuration

In the configuration block at the top of the `<script>` there are three constants to fill in:

```js
// Public URL of the serverless proxy
const WORKER_URL = 'https://YOUR-PROXY.example.workers.dev';

// Shared secret client ↔ proxy (must match on both sides)
const APP_SECRET = 'CHANGE-THIS-TO-A-LONG-RANDOM-VALUE';

// URL of the published CSV with the user list
const USERS_CSV_URL = 'https://docs.google.com/.../pub?output=csv';
```

### 📄 User sheet format

| usuarios | contraseñas |
|----------|-------------|
| user1    | password1   |
| user2    | password2   |

- Two columns: username and password.
- The first row may be a header (auto-detected).
- Publish as CSV (*File → Share → Publish to web → CSV*).

---

## 🛰️ The Serverless Proxy

The proxy is a lightweight function that:

1. Accepts only `POST` requests from allowed origins (configurable CORS list).
2. Verifies a shared secret in a header; if it doesn't match, responds `401`.
3. Injects the API key from a secret environment variable (**never in the code**).
4. Forwards the request to the AI provider and returns the response.

| Environment variable | Description |
|---|---|
| `API_KEY` | AI provider key (secret) |
| `APP_SECRET` | Same value as the client's `APP_SECRET` constant |

---

## 🚀 Deployment

| Step | Action |
|---|---|
| 1️⃣ | Deploy the serverless proxy and set `API_KEY` and `APP_SECRET` |
| 2️⃣ | Add the frontend domain to the proxy's allowed origins |
| 3️⃣ | Fill `WORKER_URL`, `APP_SECRET` and `USERS_CSV_URL` in the HTML |
| 4️⃣ | Publish the user sheet as CSV |
| 5️⃣ | Upload the HTML to static hosting and (optional) attach a domain |

---

## ⚠️ Known Limitations

| Limitation | Detail |
|---|---|
| 🖥️ Screen capture desktop-only | `getDisplayMedia` / `ImageCapture` are unreliable on mobile browsers |
| 🎤 Browser-dependent voice | `SpeechRecognition` quality varies by browser and OS |
| ⏳ No streaming | The response is shown when complete; latency may be noticeable |
| 🔓 Client-side security | The user list is inspectable; do not use with sensitive data |

---

## 🎛️ Customization

- **Language and voice** — adjustable in `SpeechRecognition` and `speechSynthesis`.
- **Model and length** — configurable in the request body (`model`, `max_tokens`).
- **System prompt** — editable to change behavior and style.
- **Styles** — all CSS lives in the `<head>`, with variables for theming.

---

## 🛠️ Tech Stack

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![Web Speech API](https://img.shields.io/badge/Web%20Speech%20API-4285F4?style=for-the-badge&logo=googlechrome&logoColor=white)
![Cloudflare Workers](https://img.shields.io/badge/Cloudflare%20Workers-F38020?style=for-the-badge&logo=cloudflare&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)

---

## 📬 Contact

<div align="center">

**Luis Choque** · Telecommunications Engineer

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/luis-f-choque-paca-0b6324240/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Eztrenne)

*Feel free to reach out for questions, feedback or collaboration.*

---

*Built with 💙 by Eztrenne · Telecommunications Engineer*

</div>

---

<div align="center">

<sub>© 2026 <strong>Luis Fernando Choque Paca</strong></sub>

</div>
