# AI-Powered Academic Notes Summarizer

A local Flask web application that turns pasted academic notes or text-based PDF documents into structured study material using the Groq API. It creates a main summary, key concepts, definitions, important points, 15 revision questions, quick revision notes, and key takeaways.

> **Important:** The application requires your own Groq API key. No fake or sample AI results are shown. If the key is missing or invalid, the app displays a clear error.

## Features

- Paste lecture notes or upload a PDF (or provide both).
- Choose Short Summary, Detailed Summary, or Exam Preparation Notes.
- Generate output in English or Urdu.
- Extract PDF text with PyMuPDF.
- Structured study guide with seven sections.
- Five short-answer, five conceptual, and five long-answer questions.
- Copy the study guide or download it as a `.txt` file.
- Responsive interface, loading state, validation, and readable error messages.
- API key stays on the Flask server and is loaded from `.env`.
- No database; submitted documents are processed in memory and not saved.

## Technology

- Python 3.10+
- Flask
- Groq Python SDK
- python-dotenv
- PyMuPDF
- HTML, CSS, JavaScript, Bootstrap 5

## Project structure

```text
academic-notes-summarizer/
├── app.py
├── requirements.txt
├── .env.example
├── .gitignore
├── README.md
├── templates/
│   └── index.html
├── static/
│   ├── style.css
│   └── script.js
└── uploads/
    └── .gitkeep
```

## Requirements

- Windows 10/11, macOS, or Linux.
- Python 3.10 or newer.
- Internet connection for Groq API calls.
- A Groq API key from https://console.groq.com/keys.

Check Python in Windows PowerShell:

```powershell
py --version
```

If `py` is not recognized, install Python from https://www.python.org/downloads/ and enable the option to add Python to PATH.

## Windows PowerShell installation

1. Extract the ZIP file.
2. Open PowerShell in the extracted `academic-notes-summarizer` folder. In File Explorer, open the folder, click the address bar, type `powershell`, and press Enter.
3. Create and activate a virtual environment:

```powershell
py -m venv venv
venv\Scripts\activate
```

If PowerShell blocks activation, run this command for the current terminal session, then activate again:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
venv\Scripts\activate
```

4. Install dependencies:

```powershell
py -m pip install --upgrade pip
py -m pip install -r requirements.txt
```

5. Create your environment file. Copy the example:

```powershell
Copy-Item .env.example .env
```

6. Open `.env` in Notepad:

```powershell
notepad .env
```

Replace `your_groq_api_key_here` with your real Groq API key:

```env
GROQ_API_KEY=gsk_your_real_key_here
GROQ_MODEL=llama-3.3-70b-versatile
```

Save and close Notepad. Do not share `.env`, commit it to Git, or put the key in JavaScript/HTML. The `.gitignore` file excludes it from version control. The ZIP intentionally contains only `.env.example`, never a real key.

## Run the application

Make sure the virtual environment is active and you are in the project folder:

```powershell
py app.py
```

Open this address in your browser:

**http://127.0.0.1:5000**

Keep the PowerShell window open while using the app. Press `Ctrl+C` in that window to stop the server.

## How to use

1. Paste notes in the text box, upload a PDF, or do both.
2. Choose a summary style and output language.
3. Select **Generate Summary**.
4. Review the seven generated sections.
5. Use **Copy Summary** or **Download .txt** to keep the material.
6. **Generate Again** reruns the same input; **Clear All** resets the page.

## Input and privacy limits

- PDF uploads are limited to 10 MB.
- Extracted source text is limited to 120,000 characters.
- Only `.pdf` files are accepted.
- This version extracts selectable PDF text. Scanned/image-only PDFs need OCR and will show a helpful error.
- Input content is processed in memory and is not stored in an application database or uploads folder.
- Content is sent to Groq to generate the requested output; review Groq's current privacy and data-use terms before submitting sensitive material.
- AI output can contain mistakes. Compare important study facts with your original course material.

## Common errors and solutions

| Error | What to do |
|---|---|
| `GROQ_API_KEY is missing` | Create `.env` from `.env.example`, add your key, save, and restart `py app.py`. |
| Groq rejected the API key / HTTP 401 | Check that the key is copied correctly, has no surrounding quotes/spaces, and is active. |
| Rate limit reached / HTTP 429 | Wait and retry, or check your Groq account limits. |
| Could not connect to Groq | Check your internet connection, proxy, or firewall. |
| No readable text found | The PDF may be scanned. Use a text-based PDF or add OCR in a future version. |
| Invalid PDF | Re-export the document as a valid PDF and upload again. |
| File too large | Use a PDF under 10 MB or split it into smaller files. |
| `py` is not recognized | Reinstall Python and enable PATH, or use `python` in place of `py`. |
| Port 5000 is already in use | Close the other app using port 5000, then restart this app. |
| Module not found | Activate `venv` and run `py -m pip install -r requirements.txt` again. |

## Configuration

The default model is `llama-3.3-70b-versatile`. You can set `GROQ_MODEL` in `.env` to another model available to your Groq account. Model availability and API limits can change.

## Testing checklist

- Empty form: frontend and backend reject empty input.
- Unsupported upload: backend rejects non-PDF extensions.
- Empty/unreadable PDF: backend returns a validation error.
- Oversized upload or extracted text: backend returns HTTP 413.
- Missing API key: backend returns a clear setup message.
- Invalid key, rate limit, network/API errors: handled with user-facing messages.
- Successful live summarization: requires a valid Groq key and internet access.

The project can be syntax-checked without an API key:

```powershell
py -m py_compile app.py
```

PDF extraction can be tested locally with a readable PDF. A real Groq integration test requires a valid API key; the app does not substitute fabricated results when the API is unavailable.
