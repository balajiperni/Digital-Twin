# 🤖 Balaji's AI Digital Twin

An AI-powered **Digital Twin and Professional Portfolio Assistant** that represents Balaji Perni and interacts with website visitors such as recruiters, hiring managers, collaborators, and other professionals.

Instead of simply displaying a static resume, this project allows visitors to **have a conversation with an AI representation of Balaji**, ask questions about his education, skills, projects, experience, career interests, and technical background, and potentially leave their contact information for follow-up.

The system uses **Google Gemini through its OpenAI-compatible API**, a custom professional knowledge base, LinkedIn profile information, tool calling, and Pushover notifications to create an interactive and agentic portfolio experience.

---

## ✨ Features

### 🧑‍💻 Professional Digital Twin

The AI is instructed to represent Balaji and answer questions about:

* Education
* Technical skills
* Programming languages
* Data Science
* Machine Learning
* Artificial Intelligence
* Computer Vision
* Generative AI
* AI Agents
* Software development
* Projects
* Certifications
* Career interests
* Professional goals

The digital twin is designed to communicate professionally and naturally with recruiters and other visitors.

---

### 📄 Personal Knowledge Base

The digital twin uses a custom `summary.txt` file containing detailed information about Balaji's:

* Academic background
* Technical skills
* Projects
* Career direction
* Learning experience
* Professional interests

This allows the AI to answer questions specifically about Balaji rather than behaving like a generic chatbot.

---

### 💼 LinkedIn Integration

The project also reads information from a LinkedIn PDF:

```text
linkedin.pdf
```

The PDF is processed using `pypdf` and its text is provided to the AI as additional context.

This gives the digital twin two important sources of information:

```text
summary.txt
      +
linkedin.pdf
      ↓
Digital Twin Context
```

---

### 🧠 Gemini-Powered AI

The application uses Google's Gemini API through Google's OpenAI-compatible endpoint.

The OpenAI Python SDK is used as the client:

```python
gemini = OpenAI(
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/",
    api_key=google_api_key,
)
```

This allows the application to use the familiar:

```python
openai.chat.completions.create(...)
```

interface while sending requests to Gemini.

---

### 🛠️ Tool Calling

The digital twin is not limited to generating text.

Gemini can decide when it needs to use a tool.

The application supports a tool-calling workflow:

```text
Visitor
   ↓
Gemini
   ↓
Tool required?
   ├── No → Generate answer
   │
   └── Yes
        ↓
     Tool Call
        ↓
 handle_tool_calls()
        ↓
   Tool Result
        ↓
     Gemini
        ↓
   Final Answer
```

This gives the digital twin the ability to perform actions instead of only responding to questions.

---

### 📩 Contact / Lead Capture

If a visitor is interested in contacting Balaji, discussing an opportunity, hiring him, or collaborating with him, the digital twin can request their email address.

The email can then be processed through the application's tool system for follow-up.

This turns the portfolio into a simple **AI-powered lead-generation system**.

---

### 🔔 Pushover Notifications

The project uses **Pushover** as a notification mechanism.

For example, if the digital twin encounters a question it cannot confidently answer, the question can be recorded and a notification can be sent.

Example workflow:

```text
Recruiter
    ↓
"As Balaji worked with AWS?"
    ↓
Digital Twin
    ↓
Information unavailable
    ↓
Record question
    ↓
Pushover API
    ↓
📱 Balaji receives notification
```

This allows Balaji to discover questions that visitors are asking and continuously improve the digital twin's knowledge base.

---

## 🏗️ System Architecture

The overall architecture looks like this:

```text
                         ┌──────────────────┐
                         │ Website Visitor  │
                         │ / Recruiter      │
                         └────────┬─────────┘
                                  │
                                  ▼
                         ┌──────────────────┐
                         │     Gradio       │
                         │  Chat Interface  │
                         └────────┬─────────┘
                                  │
                                  ▼
                         ┌──────────────────┐
                         │     app.py       │
                         │ Chat Controller  │
                         └────────┬─────────┘
                                  │
                                  ▼
                    ┌──────────────────────────┐
                    │       Google Gemini      │
                    │      LLM / Reasoning     │
                    └────────────┬─────────────┘
                                 │
                   ┌─────────────┴─────────────┐
                   │                           │
                   ▼                           ▼
            Normal Question              Tool Required
                   │                           │
                   ▼                           ▼
              AI Response               Tool Calling
                                               │
                                               ▼
                                      handle_tool_calls()
                                               │
                              ┌────────────────┴──────────────┐
                              │                               │
                              ▼                               ▼
                       Contact Capture                Unknown Question
                              │                               │
                              └───────────────┬───────────────┘
                                              ▼
                                         Pushover
                                              │
                                              ▼
                                       📱 Notification
```

---

## 📁 Project Structure

```text
digital-twin/
│
├── app.py
│
├── context.py
│
├── tools.py
│
├── styles.py
│
├── summary.txt
│
├── linkedin.pdf
│
├── .env
│
├── requirements.txt
│
└── README.md
```

### `app.py`

The main application entry point.

It:

* Loads environment variables
* Initializes the Gemini client
* Loads the digital-twin system prompt
* Maintains conversation history
* Sends requests to Gemini
* Handles tool calls
* Starts the Gradio interface

---

### `context.py`

Contains the digital twin's system prompt.

The system prompt defines:

* Who the AI represents
* What information it can discuss
* How it should communicate
* How it should behave with recruiters
* How it should handle unknown information
* How it should use tools
* How it should avoid hallucinating information

---

### `tools.py`

Contains the tools available to the digital twin.

The tools can be used for actions such as:

* Recording visitor information
* Recording unanswered questions
* Sending Pushover notifications
* Handling follow-up information

The exact tools depend on the implementation.

---

### `styles.py`

Contains the Gradio interface styling and example questions.

This controls the visual presentation and predefined example prompts.

---

### `summary.txt`

Contains the detailed professional profile of Balaji.

This acts as the main knowledge source for the digital twin.

---

### `linkedin.pdf`

Contains the exported LinkedIn profile.

The application extracts the text using:

```python
from pypdf import PdfReader
```

and provides the extracted information to Gemini.

---

## ⚙️ Tech Stack

| Technology        | Purpose                 |
| ----------------- | ----------------------- |
| Python            | Core application        |
| Google Gemini     | LLM                     |
| OpenAI Python SDK | API client              |
| Gradio            | Chat interface          |
| Pypdf             | LinkedIn PDF extraction |
| python-dotenv     | Environment variables   |
| Pushover          | Notifications           |
| Tool Calling      | Agent actions           |
| Markdown          | Response formatting     |

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd digital-twin
```

---

### 2. Create a virtual environment

Windows:

```bash
python -m venv venv
```

Activate it:

```bash
venv\Scripts\activate
```

Linux/macOS:

```bash
python3 -m venv venv
source venv/bin/activate
```

---

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

---

### 4. Create your `.env`

Create a `.env` file in the project root:

```env
GOOGLE_API_KEY=your_google_gemini_api_key

PUSHOVER_TOKEN=your_pushover_application_token
PUSHOVER_USER=your_pushover_user_key
```

Never commit your `.env` file to GitHub.

Add this to `.gitignore`:

```text
.env
venv/
__pycache__/
*.pyc
```

---

### 5. Add your professional information

Create or update:

```text
summary.txt
```

with your professional profile.

Also place your LinkedIn PDF in the project directory:

```text
linkedin.pdf
```

---

### 6. Configure the Gemini model

In `app.py`, configure the model:

```python
MODEL_NAME = "gemini-2.5-flash"
```

The Gemini client is initialized through the OpenAI-compatible endpoint:

```python
gemini = OpenAI(
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/",
    api_key=google_api_key,
)
```

---

### 7. Run the application

```bash
python app.py
```

Gradio will start the application locally.

You can then open the local URL displayed in the terminal.

---

## 💬 Example Questions

Visitors can ask questions such as:

```text
Tell me about Balaji.
```

```text
What technologies does Balaji know?
```

```text
What is Balaji's strongest programming language?
```

```text
Tell me about his Machine Learning experience.
```

```text
What projects has Balaji worked on?
```

```text
Explain Balaji's Task Manager project.
```

```text
What experience does Balaji have with Computer Vision?
```

```text
Has Balaji worked with PostgreSQL?
```

```text
What are Balaji's career interests?
```

```text
What AI technologies has Balaji explored?
```

```text
How can I contact Balaji?
```

---

## 🔄 Example Agent Workflow

### Normal Question

```text
Visitor:
"What programming languages does Balaji know?"

        ↓

Gemini receives:
- System prompt
- Professional summary
- LinkedIn context
- Conversation history
- User question

        ↓

Gemini generates answer

        ↓

Visitor receives response
```

---

### Tool-Based Question

```text
Visitor:
"I'd like to contact Balaji."

        ↓

Gemini

        ↓

Digital twin asks:
"Sure. Please provide your email address."

        ↓

Visitor:
"recruiter@example.com"

        ↓

Gemini generates tool call

        ↓

handle_tool_calls()

        ↓

Contact information recorded

        ↓

Follow-up notification/action
```

---

### Unknown Question

```text
Visitor:
"Has Balaji worked with AWS Lambda in production?"

        ↓

Gemini checks available context

        ↓

Information not available

        ↓

Tool records the question

        ↓

Pushover notification

        ↓

📱 Balaji receives notification

        ↓

Visitor receives an honest response
```

The system intentionally avoids making up information.

---

## 🔐 Security

Do not expose API keys in source code.

Use environment variables:

```env
GOOGLE_API_KEY=...
PUSHOVER_TOKEN=...
PUSHOVER_USER=...
```

Make sure `.env` is included in `.gitignore`.

Never commit:

```text
.env
```

to a public GitHub repository.

If an API key is accidentally exposed, revoke it and generate a new one.

---

## 🧠 Design Philosophy

The goal of this project is not to create a chatbot that simply answers questions.

The goal is to create an **AI representation of a professional portfolio** that can:

* Understand a person's professional background
* Answer questions conversationally
* Represent their skills and projects
* Interact with recruiters
* Capture potential professional leads
* Identify knowledge gaps
* Notify the person about unanswered questions
* Continuously improve through new information

The system follows an important principle:

> **If the digital twin does not know something, it should say that it doesn't know rather than inventing an answer.**

This is especially important when the AI is representing a real person professionally.

---

## 🔮 Future Improvements

Potential future improvements include:

* Deploying the digital twin publicly
* Adding a custom frontend instead of Gradio
* Adding persistent conversation storage
* Adding a vector database for better retrieval
* Implementing RAG
* Adding semantic search across projects and documents
* Adding GitHub profile integration
* Adding automatic resume synchronization
* Adding LinkedIn data synchronization
* Adding recruiter lead analytics
* Adding email notifications
* Adding a recruiter dashboard
* Adding conversation analytics
* Adding multilingual support
* Adding voice interaction
* Adding a more advanced memory system
* Integrating additional professional documents
* Adding authentication for the owner dashboard

---

## 🎯 Project Goal

The long-term goal of this project is to transform a traditional personal portfolio from:

```text
Resume
+
Projects
+
Contact Form
```

into:

```text
AI Digital Twin
       +
Professional Knowledge
       +
Conversational Interface
       +
Tool Calling
       +
Lead Capture
       +
Notifications
       +
Continuous Improvement
```

This allows visitors to interact with a professional profile conversationally instead of simply reading a static resume.

---

## 👨‍💻 About

**Balaji Perni**

B.Tech — Artificial Intelligence & Data Science
SRKR Engineering College
Expected Graduation: 2027

Areas of interest:

**Artificial Intelligence · Data Science · Machine Learning · Computer Vision · Generative AI · AI Agents · Data Analysis · Software Development**

---

## ⭐ Why This Project?

Traditional portfolios tell visitors:

> "Here is my resume."

This project aims to let visitors ask:

> "What has Balaji actually built?"

> "What technologies does he know?"

> "What is his experience with Machine Learning?"

> "Can I contact him?"

and receive an interactive, AI-powered response.

**This is a portfolio that can talk about itself.**
