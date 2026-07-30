# 3️⃣ Capability Guide

This section provides **practical, end‑to‑end examples** that demonstrate how an AI assistant can be used across a wide range of domains.  Each subsection contains:
- A brief problem statement.
- The high‑level workflow (often a Mermaid diagram).
- Code snippets (Python, Java, JavaScript) that can be copied into a project.
- Tips for extending the solution.

---

## 3.1 Programming
### Example: Automated Code Refactor
```java
// Java – use the assistant to refactor a method
String original = "public int add(int a,int b){return a+b;}";
String prompt = "Refactor the above method to use Java 17 records and add Javadoc.";
String refactored = aiAssistant.ask(prompt, original);
System.out.println(refactored);
```
**Workflow**
```mermaid
flowchart LR
    A[User writes prompt] --> B[Assistant receives prompt]
    B --> C[Tool: execute_code (Java parser)]
    C --> D[Assistant returns refactored code]
    D --> E[User reviews & integrates]
```
**Key points**
- Use **function calling** to invoke a language‑specific formatter before sending back code.
- Validate the output with unit tests.

---

## 3.2 Web Development
### Example: Scaffold a FastAPI service
```python
import requests, json
prompt = "Create a minimal FastAPI app with a /hello endpoint that returns 'Hello, <name>!'" 
response = ai.ask(prompt)
open('app.py','w').write(response)
```
**Result (`app.py`)**
```python
from fastapi import FastAPI
app = FastAPI()
@app.get("/hello/{name}")
def hello(name: str):
    return {"msg": f"Hello, {name}!"}
```
**Tips**
- After generation, run `uvicorn app:app --reload` to verify.
- Add OpenAPI docs automatically; the assistant can generate Pydantic models for request bodies.

---

## 3.3 Mobile Apps
### Example: React Native component for image capture
```javascript
// Prompt to the assistant
const prompt = `Generate a React Native component that uses expo-camera to capture a photo and returns the base64 string.`;
const component = await ai.ask(prompt);
```
**Result** – paste the returned component into `CameraCapture.js`.

---

## 3.4 AI Engineering
### Example: Build a Retrieval‑Augmented Generation (RAG) pipeline
1. **Ingest documents** → embed with `sentence‑transformers`.
2. **Store** embeddings in **Qdrant**.
3. **Query** → retrieve top‑k passages → feed to LLM.
```mermaid
flowchart TD
    A[Document Corpus] --> B[Embedding Model]
    B --> C[Vector DB (Qdrant)]
    D[User Query] --> E[Embed Query]
    E --> C
    C --> F[Top‑k Retrieval]
    F --> G[LLM (RAG Prompt)]
    G --> H[Answer]
```
**Code stub** (Python)
```python
from sentence_transformers import SentenceTransformer
import qdrant_client
model = SentenceTransformer('all-MiniLM-L6-v2')
# ... ingest, query, generate ...
```

---

## 3.5 Automation & DevOps
### Example: Auto‑scale a Kubernetes deployment based on LLM‑predicted load
- Assistant predicts next‑hour request volume using historical metrics.
- Generates a `kubectl` command to adjust the replica count.
```bash
prediction=$(ai ask "Predict next hour request count for service X based on last 7 days of logs.")
replicas=$((prediction/100 + 1))
kubectl scale deployment my‑service --replicas=$replicas
```
**Safety tip:** always wrap the command in a script that validates the number before applying.

---

## 3.6 Security & Reverse Engineering
### Example: Identify a potential insecure function call in Java code
```java
String code = "..."; // user‑provided snippet
String prompt = "Find any usage of Runtime.exec or ProcessBuilder that does not sanitize input.";
String findings = ai.ask(prompt, code);
System.out.println(findings);
```
The assistant returns line numbers and suggestions for sanitisation.

---

## 3.7 Data Science & ML
### Example: Generate a Pandas data‑cleaning pipeline from a CSV description
```python
prompt = "Given a CSV with columns: date (YYYY‑MM‑DD), amount (string with $), category (nullable). Write a pandas pipeline that parses dates, converts amount to float, and fills missing categories with 'Other'."
pipeline = ai.ask(prompt)
exec(pipeline)  # runs the generated code
```
**Result** – a ready‑to‑run function `clean(df)`.

---

## 3.8 Robotics & Control
### Example: Generate ROS2 node skeleton for a lidar driver
```bash
ai ask "Create a Python ROS2 node that subscribes to /scan and publishes filtered point clouds on /filtered_scan." > lidar_node.py
```
The assistant includes imports, a callback, and a simple filter.

---

## 3.9 Prompt Engineering & Technical Writing
### Example: Turn a technical spec into a Markdown README automatically
```markdown
User prompt: "Convert this bullet list of features into a polished README with sections, badges, and a usage example."
Assistant output → `README.md`
```

---

## 3.10 Summary Table
| Domain | Typical Prompt | Example Tool(s) |
|--------|----------------|-----------------|
| Programming | "Refactor this Java method..." | `execute_code`, `patch` |
| Web Dev | "Scaffold a FastAPI app..." | `write_file`, `terminal` |
| Mobile | "Generate a React Native component..." | `write_file` |
| AI Engineering | "Build a RAG pipeline..." | `write_file`, `execute_code` |
| DevOps | "Predict load and scale K8s..." | `terminal`, `execute_code` |
| Security | "Find unsafe Runtime.exec usage..." | `read_file`, `execute_code` |
| Data Science | "Create a pandas cleaning script..." | `write_file`, `execute_code` |
| Robotics | "Python ROS2 node for lidar..." | `write_file` |
| Prompt Engineering | "Convert spec to README..." | `write_file` |

---

> **All examples are deliberately concise.** In a real project you would add unit tests, CI pipelines, and error handling. The assistant can generate those artefacts on demand.
