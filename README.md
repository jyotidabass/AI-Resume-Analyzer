# 🤖 AI Resume Analyzer

An AI-powered Resume Analyzer built using **Streamlit and Google Gemini AI** that evaluates resumes and provides detailed feedback to improve job readiness.

The system processes uploaded resumes, extracts information, evaluates resume quality, and generates actionable suggestions using large language models.

> 📖 **Full step-by-step blog walkthrough available on Medium:**
> [Build Your Own AI Resume Analyzer with Python and Google Gemini](https://medium.com/tech-ai-made-easy/build-your-own-ai-resume-analyzer-with-python-and-google-gemini-c31a330dfec8?sk=2afabc147523e44d8ade0e323efeb2b3)
> The blog explains every single function, every design decision, and every line of code in the simplest possible words — perfect if you want to understand the project deeply before building it yourself.

---

# 📌 Overview

This project helps job seekers automatically analyze their resumes and identify areas of improvement such as:

* Resume structure
* Skills presentation
* Action verbs usage
* Professional tone
* Missing information
* Overall resume effectiveness

The application provides instant feedback through an interactive web interface.

---

# 📰 Blog Walkthrough

A detailed blog covering this entire project has been published on Medium:

**🔗 [Build Your Own AI Resume Analyzer with Python and Google Gemini](https://medium.com/tech-ai-made-easy/build-your-own-ai-resume-analyzer-with-python-and-google-gemini-c31a330dfec8?sk=2afabc147523e44d8ade0e323efeb2b3)**

The blog covers:

* What the app does and how it works end to end
* How to set up Google Gemini AI and BigQuery
* Step-by-step explanation of every function in `app.py`
* How PDF text extraction and image conversion work
* How language detection and translation are handled
* How asyncio runs 3 AI checks in parallel to make the app fast
* How the prompts folder controls the AI's behaviour
* How session management and feedback collection work
* A complete guide on how to run the app locally from scratch

Whether you are a beginner or an experienced developer, the blog is written in simple, plain English so anyone can follow along and build their own version.

---

# 🚀 Features

* Upload and analyze resumes in **PDF format**
* Automatic **text extraction from resumes**
* **AI-based resume evaluation**
* Identification of **missing skills and weak sections**
* Suggestions to improve resume quality
* Detection of **strong and weak action verbs**
* Automatic **language detection and translation support** (133 languages)
* Full review delivered in the **user's original language**
* Simple **Streamlit web interface**
* Session tracking and feedback saved to **Google BigQuery**

---

# 🏗️ Project Architecture

The application follows a simple pipeline:

```
User Upload Resume
        │
        ▼
PDF Processing
(PyMuPDF / PDF2Image)
        │
        ▼
Text Extraction
        │
        ▼
Language Detection
        │
        ▼
Translation (if required)
        │
        ▼
AI Resume Evaluation
(Google Gemini)
        │
        ▼
Feedback & Suggestions
```

---

# 📂 Project Structure

```
AI-Resume-Analyzer
│
├── app.py
│   Main Streamlit application
│
├── requirements.txt
│   List of required Python libraries
│
├── prompts/
│   ├── extract_information.txt
│   ├── final_review.txt
│   ├── action_words.txt
│   ├── guidelines0.txt
│
└── README.md
```

---

# ⚙️ Installation

## 1. Clone the Repository

```
git clone https://github.com/jyotidabass/AI-Resume-Analyzer.git
cd AI-Resume-Analyzer
```

---

## 2. Create a Virtual Environment

### Windows

```
python -m venv venv
venv\Scripts\activate
```

### Mac / Linux

```
python3 -m venv venv
source venv/bin/activate
```

---

## 3. Install Dependencies

```
pip install -r requirements.txt
```

---

# 🔑 API Setup

This project requires a **Google Gemini API key**.

1. Visit the Google AI Studio website
2. Generate an API key
3. Save it in a file called `credentials.json` in the project folder:

```json
{
  "token": "YOUR_GEMINI_API_KEY"
}
```

Also set up a Google Cloud service account and save the key as `service-account-key.json` in the project folder. This is required for BigQuery session and feedback storage.

> 💡 If you want to skip BigQuery entirely, remove the `upload_to_bq` calls and the BigQuery client setup from `app.py`. The resume analysis will still work fully.

> 📖 See the [Medium blog](https://medium.com/tech-ai-made-easy/build-your-own-ai-resume-analyzer-with-python-and-google-gemini-c31a330dfec8?sk=2afabc147523e44d8ade0e323efeb2b3) for a complete step-by-step setup guide.

---

# ▶️ Running the Application

Start the Streamlit application using:

```
streamlit run app.py
```

After running the command, the application will open in your browser.

Default URL:

```
http://localhost:8501
```

---

# 📄 Supported File Format

Currently supported resume format:

* PDF (.pdf)

---

# 🧠 How the Code Works

## 1. Resume Upload

The Streamlit interface allows users to upload their resume.

```
st.file_uploader()
```

---

## 2. PDF Processing

The uploaded PDF file is processed using **PyMuPDF** to extract text, font names, font sizes, and hyperlinks.

```
import fitz
```

This library reads the PDF and extracts the resume content. If the resume is more than 3 pages, the analysis stops and a warning is shown.

---

## 3. Language Detection

The system detects the language of the resume using the **langdetect** library.

If the resume is not in English, it is automatically translated to English before analysis using **deep-translator**. The original language code is saved so the final review is returned to the user in their own language.

> Supports 133 languages.

---

## 4. Resume Information Extraction

The file:

```
prompts/extract_information.txt
```

contains instructions used by the AI to extract structured information from the resume.

Example information extracted:

* Name
* Skills
* Work experience
* Education
* Certifications

---

## 5. Parallel AI Checks with asyncio

The app uses Python's **asyncio** to run 3 AI checks at the same time instead of one after another. This is the main reason the app finishes in about 15 seconds.

The three parallel tasks are:
* Extract personal info as a JSON object
* Check experience level vs resume page count (guidelines0.txt)
* Check overall content quality (guidelines1.txt)

A fourth check sends both text and resume page images to Gemini together — this is called a **multimodal check** and lets the AI inspect the visual layout of the resume.

> 📖 The [Medium blog](https://medium.com/tech-ai-made-easy/build-your-own-ai-resume-analyzer-with-python-and-google-gemini-c31a330dfec8?sk=2afabc147523e44d8ade0e323efeb2b3) explains asyncio and the parallel architecture in simple words.

---

## 6. Resume Evaluation

The AI evaluates the resume using prompts defined in:

```
prompts/final_review.txt
```

The evaluation includes:

* Resume strengths
* Weaknesses
* Missing information
* Improvement suggestions

---

## 7. Action Words Analysis

The file:

```
prompts/action_words.txt
```

contains a list of strong action verbs commonly used in professional resumes.

The system checks if the resume uses strong action verbs like Led, Built, Designed, Optimized, and Achieved.

---

## 8. Font Check

The app checks whether the resume uses ATS-safe fonts from the recommended list: Times New Roman, Arial, Helvetica, Verdana, and Calibri. It also checks that font sizes stay in the 10–12pt range.

ATS stands for Applicant Tracking System — the software companies use to scan resumes automatically before a human sees them. Wrong fonts can cause ATS to misread your resume.

---

# 📦 Dependencies

Main libraries used in this project:

```
streamlit
pandas
pymupdf
pdf2image
Pillow
google-generativeai
deep-translator
langdetect
google-cloud-bigquery
streamlit-feedback
requests
```

All dependencies are listed in `requirements.txt`.

---

# 🔒 Data Privacy

The application processes the resume only for analysis purposes.
Sensitive information should not be stored permanently unless explicitly configured.
Session tracking uses an anonymized hash of the user's IP address and current time — no personal data is stored.

---

# 🧪 Example Workflow

1. User uploads a resume
2. The system extracts text, fonts, and links from the PDF
3. Language detection is performed and translation applied if needed
4. PDF pages are converted to images for visual analysis
5. Three AI checks run in parallel using asyncio
6. Action words and font checks run instantly without any AI
7. A final combined review is generated in the user's original language
8. Suggestions and improvements are displayed grouped by resume section
9. User feedback is saved to BigQuery

---

# 🤝 Contributing

Contributions are welcome.

Steps to contribute:

1. Fork the repository
2. Create a new branch
3. Commit your changes
4. Submit a pull request

---

# 📜 License

This project is released under the MIT License.

---

# 📖 Read the Full Blog

If you want a complete, plain-English walkthrough of every function and design decision in this project, read the full blog on Medium:

**🔗 [Build Your Own AI Resume Analyzer with Python and Google Gemini](https://medium.com/tech-ai-made-easy/build-your-own-ai-resume-analyzer-with-python-and-google-gemini-c31a330dfec8?sk=2afabc147523e44d8ade0e323efeb2b3)**

---

# ⭐ Support

If you find this project useful, consider:

* Starring the repository
* Sharing it with others
* Contributing improvements
* Reading and clapping for the [Medium blog](https://medium.com/tech-ai-made-easy/build-your-own-ai-resume-analyzer-with-python-and-google-gemini-c31a330dfec8?sk=2afabc147523e44d8ade0e323efeb2b3)
