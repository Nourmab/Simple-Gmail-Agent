<<<<<<< HEAD
# 📬 Email Agent

An AI-powered email assistant that connects to your Gmail inbox, detects new emails, and presents the **sender** and **subject** of each message — using the Gmail API and Groq's LLaMA model.

---

## 🧠 How it works

The agent uses a LangGraph ReAct loop powered by **Groq (LLaMA 3.3 70B)** as the LLM. It is given two Gmail tools:

- **`FixedGmailSearch`** — queries Gmail with a search string and returns a list of message IDs
- **`FixedGmailGetMessage`** — fetches only the `subject` and `from` headers for each message (no body, no attachments)

When you ask *"how many emails did I receive today?"*, the agent searches Gmail with `after:YYYY/MM/DD`, iterates over the results, and replies with the count and list of subjects — using minimal tokens because no email body is ever fetched.

---

## 🗂️ Project structure

```
email-assistant/
├── main.ipynb          # Main notebook
├── credentials.json    # Google OAuth client secrets (you provide)
├── token.json          # Auto-generated after first auth
├── .env                # API keys
└── README.md
```

---

## ⚙️ Setup

### 1. Clone the repo

```bash
git clone https://github.com/your-username/email-assistant.git
cd email-assistant
```

### 2. Create a virtual environment

```bash
python -m venv .venv
.venv\Scripts\activate        # Windows
source .venv/bin/activate     # macOS/Linux
```

### 3. Install dependencies

```bash
pip install langchain-community langchain langgraph langgraph-prebuilt \
            langchain-groq python-dotenv google-auth-oauthlib \
            google-auth-httplib2 google-api-python-client certifi
```

### 4. Set up environment variables

Create a `.env` file at the root:

```env
GROQ_API_KEY=your_groq_api_key_here
```

Get your Groq API key at [console.groq.com](https://console.groq.com).

### 5. Set up Gmail API credentials

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a project and enable the **Gmail API**
3. Create **OAuth 2.0 credentials** (Desktop app)
4. Download the file and rename it to `credentials.json`, place it at the root
5. On first run, a browser window will open for authentication — `token.json` is created automatically

---

## 🚀 Usage

Open `main.ipynb` and run all cells. Then call:

```python
run_agent("Search emails after 2026/05/08 and tell me how many I received today and list their subjects")
```

Example output:

```
🔄 Attempt 1: Search emails after 2026/05/08 ...
✅ Done: You received 3 emails today:
1. Subject: INTERNSHIP - AI SOFTWARE ENGINEER | From: recruiter@trekea.com
2. Subject: How to Become More Creative | From: write@catalyst.com
3. Subject: New jobs for you in France | From: linkedin@linkedin.com
```

---

## 🔑 Key design decisions

| Decision | Reason |
|---|---|
| `format="metadata"` in Gmail API | Never fetches email body — drastically reduces tokens |
| `fields="payload/headers"` | Only returns Subject and From headers, nothing else |
| Groq LLaMA 3.3 70B | Fast inference, free tier available |
| Patched `_parse_messages` | LangChain's default parser fetches full raw emails — overridden to use metadata only |
| No `GmailGetMessage` body tool | Not needed for list/count tasks |

---

## 📦 Dependencies

| Package | Purpose |
|---|---|
| `langchain-community` | Gmail toolkit and tools |
| `langchain-groq` | Groq LLM integration |
| `langgraph` | ReAct agent loop |
| `google-api-python-client` | Gmail API client |
| `google-auth-oauthlib` | OAuth2 authentication |
| `certifi` | SSL certificate fix |
| `python-dotenv` | Load `.env` variables |

---

## ⚠️ Notes

- `token.json` contains your Gmail access token — do not commit it to git
- Add both `token.json` and `credentials.json` to your `.gitignore`
- The agent only has **read access** to your inbox — it cannot send or delete emails

```gitignore
token.json
credentials.json
.env
```
=======
# Simple-Gmail-Agent
AI agent that detects new emails and extracts sender and subject using Gmail API and Groq LLaMA
>>>>>>> 781666b0a98f3fe5b34ae6e92b6547474212d52d
