# 🤖 AI Resume Analyzer

An AI-powered Resume Analyzer built using **Streamlit and Google Gemini AI** that evaluates resumes and provides detailed feedback to improve job readiness.

The system processes uploaded resumes, extracts information, evaluates resume quality, and generates actionable suggestions using large language models.

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

# 🚀 Features

* Upload and analyze resumes in **PDF format**
* Automatic **text extraction from resumes**
* **AI-based resume evaluation**
* Identification of **missing skills and weak sections**
* Suggestions to improve resume quality
* Detection of **strong and weak action verbs**
* Automatic **language detection and translation support**
* Simple **Streamlit web interface**

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
git clone https://github.com/your-username/AI-Resume-Analyzer.git
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
3. Add the API key inside `app.py`

Example:

```python
genai.configure(api_key="YOUR_API_KEY")
```

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

The uploaded PDF file is processed using **PyMuPDF** to extract text.

```
import fitz
```

This library reads the PDF and extracts the resume content.

---

## 3. Language Detection

The system detects the language of the resume using the **langdetect** library.

If the resume is not in English, it is translated before analysis.

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

## 5. Resume Evaluation

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

## 6. Action Words Analysis

The file:

```
prompts/action_words.txt
```

contains a list of strong action verbs commonly used in professional resumes.

The system checks if the resume uses strong action verbs.

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

---

# 🧪 Example Workflow

1. User uploads a resume
2. The system extracts text from the PDF
3. Language detection is performed
4. The AI analyzes resume content
5. Suggestions and improvements are generated

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

# ⭐ Support

If you find this project useful, consider:

* Starring the repository
* Sharing it with others
* Contributing improvements
