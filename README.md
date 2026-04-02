# 🎓 AI Teaching Assistant for Faculty

### Agentic RAG-based Auto Planning & Auto Assessment System

---

## 📚 Documentation for Jury Evaluation

**For Hackathon Presentation:**
- 📖 **[JURY_EVALUATION_GUIDE.md](JURY_EVALUATION_GUIDE.md)** - Comprehensive guide covering Innovation, Uniqueness, Frontend/Backend, Integration, and Agent Communication
- 🎤 **[PRESENTATION_QUICK_REFERENCE.md](PRESENTATION_QUICK_REFERENCE.md)** - Quick reference card for easy access during presentation
- 🏗️ **[ARCHITECTURE_DIAGRAM.md](ARCHITECTURE_DIAGRAM.md)** - Visual architecture diagrams and system flow

**For Development:**
- 📐 **[ARCHITECTURE_EXPLANATION.md](ARCHITECTURE_EXPLANATION.md)** - Detailed technical architecture documentation
- 🚀 **[QUICK_START.md](QUICK_START.md)** - Quick setup guide
- 📋 **[EXECUTION_GUIDE.md](EXECUTION_GUIDE.md)** - Execution and usage guide

---

## 📌 Problem Context (Why This Solution?)

In real academic environments, faculty members are responsible for multiple **manual, repetitive, and regulation-heavy tasks**, such as:

* Analyzing large syllabus PDFs
* Planning weekly lesson schedules
* Designing question papers aligned with Bloom’s Taxonomy
* Evaluating student answers using rubrics

Existing AI tools behave like **generic chatbots**:

* ❌ They hallucinate content
* ❌ They ignore syllabus boundaries
* ❌ They do not follow Bloom’s taxonomy or exam rules
* ❌ They provide no academic compliance guarantees

👉 **Hence, a chatbot-style AI is unsafe for academic workflows.**

---

## 💡 Proposed Solution (What We Built)

We built an **Agentic AI Teaching Assistant** that behaves like a **digital academic assistant**, not a chatbot.

The system:

* Works strictly within **syllabus boundaries**
* Enforces **Bloom’s Taxonomy**
* Follows **exam patterns & rubrics**
* Uses **multi-agent reasoning + guardrails** to ensure correctness

This makes the solution **safe, explainable, and suitable for universities**.

---

## 🧠 How the Solution Works (End-to-End Flow)

The solution follows a **controlled, step-by-step academic pipeline** instead of a single AI prompt.

```
Faculty Input
   ↓
Intent Understanding Agent
   ↓
Orchestrator (Decision Maker)
   ↓
RAG (Syllabus Retrieval)
   ↓
Academic Content Agents
   ↓
Compliance & Validation Agent
   ↓
Final Output to Faculty
```

Each step is **independently verified**, making the system reliable.

---

## 🏗️ System Architecture Overview

The architecture is designed around **agent separation of concerns**.

### Core Principles

* One agent = one academic responsibility
* Agents do not directly talk to each other
* Orchestrator controls execution flow
* All outputs are validated before display

![Architecture Diagram](https://github.com/Jasmikapeddada/TEAM-60/blob/main/ChatGPT%20Image%20Dec%2013%2C%202025%2C%2004_40_33%20PM.png)

---

## 🧠 Updated Agent Design (Modified from Initial Version)

To improve **clarity, academic safety, and jury explainability**, we refined the originally used agents into a **cleaner, responsibility-driven design**. Each agent now has:

* A **single academic responsibility**
* **Structured JSON input/output**
* No direct inter-agent coupling

---

### 1️⃣ Intent Understanding Agent (NEW – Added)

**Why modified?**
Earlier, user input was passed directly to generation agents, which caused ambiguity.

**Updated Role:**

* Converts faculty natural language into structured intent
* Decides *what* needs to be generated (lesson plan, questions, evaluation)

**Example Output:**

```json
{
  "tasks": ["lesson_plan", "question_paper"],
  "units": ["Unit-2"],
  "weeks": 6,
  "bloom_levels": ["Understand", "Apply"]
}
```

👉 This agent enables **dynamic orchestration**.

---

### 2️⃣ Orchestrator Agent (ENHANCED)

**Earlier:** Static execution flow

**Now:**

* Dynamically decides which agents to invoke
* Controls async execution
* Maintains execution timeline

**Responsibility:**

* No content generation
* Only routing, coordination, and logging

---

### 3️⃣ Syllabus Understanding Agent (REFINED)

**Earlier:** Raw text extraction

**Now:**

* Converts syllabus PDF into structured JSON
* Extracts:

  * Units
  * Topics
  * Learning outcomes

**Why modified?**
This structured syllabus becomes the **single source of truth** for all agents.

---

### 4️⃣ Lesson Planner Agent (SPLIT FROM CONTENT AGENT)

**Earlier:** Lesson planning + generation combined

**Now (Separated):**

* Allocates topics week-wise
* Ensures syllabus coverage balance
* Prevents topic overload

👉 Separation improves explainability and debugging.

---

### 5️⃣ Assessment Generator Agent (REFINED)

**Earlier:** Generic question generation

**Now:**

* Generates questions per Bloom level
* Enforces exam pattern rules
* Uses Bloom verbs from constants

**Guarantees:**

* No repetition
* Balanced difficulty
* Bloom compliance

---

### 6️⃣ Evaluation Agent (ENHANCED)

**Earlier:** Simple scoring

**Now:**

* Rubric-based scoring
* Criterion-wise feedback
* Missing concept detection

**Output Includes:**

* Marks
* Strengths
* Weak areas
* Suggestions

---

### 7️⃣ Compliance Agent (NEW – Non-Generative)

**Why added?**
To ensure academic safety before final output.

**Checks:**

* Out-of-syllabus content
* Bloom imbalance
* Exam rule violations

**Behavior:**

* PASS → Output displayed
* FAIL → Regeneration triggered

👉 This agent provides **institution-level trust**.

---

## 📚 RAG Pipeline (Why Answers Are Trustworthy)

### Step 1: Document Ingestion

* Syllabus PDF → text extraction
* Chunked into 500–700 tokens
* Embedded using SentenceTransformers

### Step 2: Vector Storage

* Stored in FAISS Vector DB

### Step 3: Context Retrieval

* Retrieves only relevant syllabus chunks
* Filters by unit and topic

### Step 4: Grounded Generation

* LLM generates output using retrieved context only

👉 **No retrieved context = no generation**

---

## 🛡️ Academic Guardrails

### Hard Guardrails (Non-negotiable)

* Syllabus-only generation
* Bloom taxonomy enforcement
* Exam pattern adherence

### Soft Guardrails (Quality Enhancements)

* Difficulty balancing
* Concept diversity
* Verb variation

---

## 📊 Evaluation Metrics (How Quality Is Measured)

| Metric                | Description                                       |
| --------------------- | ------------------------------------------------- |
| Bloom Alignment Score | Match between expected and generated Bloom levels |
| Coverage Completeness | % of syllabus topics covered                      |
| Difficulty Balance    | Variance across question difficulty               |
| Explainability Score  | Quality of evaluation feedback                    |

---

## 🖥️ Streamlit UI Walkthrough

### 1️⃣ Upload Page

* Upload syllabus PDF
* Select subject & output type

### 2️⃣ Processing Page

* Live agent execution status
* Intermediate outputs shown

### 3️⃣ Results Page

* Lesson plan table
* Question paper
* Bloom taxonomy distribution
* Answer evaluation feedback

---

## 🛠️ Technology Stack

| Layer      | Technology           |
| ---------- | -------------------- |
| UI         | Streamlit            |
| Agents     | LangChain / CrewAI   |
| LLM        | Groq / OpenAI        |
| Embeddings | SentenceTransformers |
| Vector DB  | FAISS                |
| Backend    | Python               |

---

## 🎯 Key Differentiators

✔ Not a chatbot
✔ Fully syllabus-grounded (RAG)
✔ Multi-agent academic reasoning
✔ Deterministic compliance checks
✔ University-ready design

---

## 🎤 Jury-Friendly One-Line Pitch

> "An agentic, syllabus-grounded AI Teaching Assistant that automates academic planning and assessment while enforcing Bloom’s taxonomy, exam rules, and compliance — something a normal chatbot cannot do."

---

## 🔮 Future Scope

* LMS integration
* Multilingual syllabus support
* Department-level analytics
* Adaptive learning recommendations
