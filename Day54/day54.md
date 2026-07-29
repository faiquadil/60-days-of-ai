# Day 54 – Question Generation Engine + New Session UI

## 🚀 Challenge
60 Days of AI – Day 54

## 🎯 Objective

Implement the complete Question Generation Engine and New Session Input UI for the AI Interview Coach by following Day 4 of the Implementation Blueprint.

---

# ✅ Features Implemented

### Data Layer
- Added session creation functionality
- Added database functions to store interview questions
- Implemented session retrieval
- Implemented question retrieval by session

### Service Layer
- Built `services.py`
- Connected AI provider with database
- Created centralized session orchestration
- Automatic provider selection (MockProvider / ClaudeProvider)

### Resume Parsing
- Implemented PDF resume parsing using **pypdf**
- Extracted text from uploaded resumes
- Added graceful handling for unreadable PDFs

### Prompt Engineering
- Created reusable Question Generation Prompt
- Created reusable Answer Evaluation Prompt
- Added helper functions for dynamic prompt construction

### User Interface
- Created a complete **New Session** page
- Role selection dropdown
- Optional Job Description input
- Optional Resume PDF upload
- Generate Questions button
- Question preview after generation

---

# 🧪 Testing Performed

## ✅ Test 1
Role only

Result:
- Session created successfully
- Five interview questions generated

---

## ✅ Test 2
Role + Job Description

Result:
- JD accepted successfully
- Questions generated without errors

---

## ✅ Test 3
Role + Resume PDF

Result:
- Resume uploaded successfully
- PDF parsed correctly
- Questions generated successfully

---

# 📂 Files Added

```
core/services.py
core/prompts.py
core/resume_parser.py
```

---

# ✏️ Files Updated

```
core/db.py
pages/new_session.py
app.py
docs/DAY4-SUMMARY.md
docs/PROJECT-STRUCTURE.md
```

---

# 🧠 Key Learnings

- Building a clean service layer for application orchestration
- Organizing project architecture for scalability
- Parsing PDF documents using pypdf
- Managing Streamlit Session State
- Designing reusable AI prompts
- Integrating SQLite with application workflows
- Separating UI, business logic, and data access

---

# 📸 Screenshots

Include:

- Milestone 1 terminal output
- New Session page
- Job Description test
- Resume Upload test
- Generated Questions screen

---

# 📝 Code Review Notes

- Current implementation is clean and modular.
- `get_ai_provider()` dynamically selects the provider, making future Claude API integration a one-line configuration change.
- No deployment required on Day 54 (scheduled for Day 9 in the Blueprint).

---

# 📌 Git Commits

### Feature Implementation

```
f6f5fd3
Day 4: Question generation engine + New Session input UI (role/JD/resume)
```

### Documentation Update

```
3600b38
Update Day 4 documentation
```

---

# 🔗 Repository

AI Interview Coach

https://github.com/faiquadil/ai-interview-coach

---

# 🎯 Day 54 Outcome

Successfully completed the Question Generation Engine and New Session UI with role selection, optional Job Description input, resume PDF parsing, database integration, and service orchestration. The application was tested across all supported input paths and is now ready for Day 55, where answer submission and AI-powered interview feedback will be implemented.
