# 10 FAQ

Below are **100 practical questions** covering the entire handbook.  Each entry includes a concise answer and, where appropriate, a link to the relevant section.

| # | Question | Answer |
|---|----------|--------|
| 1 | What is the difference between a *large language model* and a *foundation model*? | An LLM is a type of foundation model specialised for natural‑language tasks. All LLMs are foundation models, but not all foundation models handle language (e.g., vision‑only models). |
| 2 | How many tokens can GPT‑4‑o‑mini process? | 128 k tokens (≈ 96 k words). See **4. Model Guide → Fast vs Reasoning models**. |
| 3 | When should I use a fast chat model vs a reasoning model? | Use a fast model for simple Q&A, classification, or low‑latency chat. Use a reasoning model when the task needs multi‑step chain‑of‑thought, complex planning, or code debugging. |
| 4 | What is a *context window* and why does it matter? | The context window is the maximum number of tokens the model can attend to simultaneously. Anything outside is truncated, so important information must be kept inside or retrieved via RAG. |
| 5 | How do I avoid hallucinations when using an LLM? | • Use Retrieval‑Augmented Generation (RAG). • Ask the model to cite sources. • Post‑process with a verification LLM or external API. |
| 6 | What is *function calling* (MCP) and how does it work? | The model outputs a JSON payload with a `function` name and `arguments`. The system executes the function and feeds the result back, enabling safe tool usage. |
| 7 | Which vector database should I pick for a small prototype? | **Qdrant Cloud** (free tier) is easy to start; for larger scale consider **Pinecone** or **Milvus**. |
| 8 | How do I embed a document for RAG? | Use a sentence‑transformer (e.g., `all‑MiniLM‑L6‑v2`) to turn each chunk into a vector, then upsert into the vector DB. |
| 9 | What is *LoRA* and when is it useful? | Low‑Rank Adaptation (LoRA) adds a small trainable matrix to a frozen LLM, enabling cheap fine‑tuning on domain data without full model updates. |
|10| How can I cache LLM responses to reduce cost?| Store the prompt hash and model output in a key‑value store (Redis, SQLite). Re‑use cached results when the same prompt recurs. |
|11| What is the recommended way to store API keys in a FastAPI app?| Use environment variables and load them with `python‑dotenv` or the built‑in `os.getenv`. Never commit keys to source control. |
|12| How do I rate‑limit a FastAPI endpoint?| Use the `slowapi` library and decorate the route with `@limiter.limit("10/minute")`. |
|13| When should I choose PostgreSQL over a vector DB for storing embeddings?| Use PostgreSQL for relational data and metadata. Store embeddings only in a vector DB; you can keep a reference ID in PostgreSQL. |
|14| What is *chain‑of‑thought* prompting?| Instruct the model to “think step by step” before answering, which improves accuracy on arithmetic and reasoning tasks. |
|15| How do I generate a Mermaid diagram from a markdown file?| Write the diagram code inside a fenced block with `mermaid` as the language; GitHub renders it automatically. |
|16| What is the difference between *few‑shot* and *zero‑shot* prompting?| Few‑shot provides example input‑output pairs in the prompt; zero‑shot gives only the instruction. |
|17| How can I test my FastAPI endpoints locally?| Use `uvicorn` for the server and `httpx` or `curl` for requests; `pytest` with `TestClient` provides automated tests. |
|18| What is the best way to version my AI models?| Store model artifacts in an artifact repository (e.g., HuggingFace Hub, S3) with semantic version tags, and reference the version in your code. |
|19| How do I monitor token usage across multiple services?| Export a custom metric from each service (e.g., `tokens_used`) to Prometheus and aggregate in Grafana. |
|20| What is *prompt templating* and why use it?| Define reusable prompt skeletons with placeholders (e.g., `{question}`) and fill them programmatically to keep prompts consistent. |
|21| How can I enforce JSON output from a model?| Include in the system prompt: "Always respond with a JSON object matching this schema…" and set `response_format` if the provider supports it. |
|22| When should I use a *multimodal* model?| When you need to process images, audio, or video together with text, such as visual QA or caption generation. |
|23| What are the security concerns with executing code via `execute_code`?| Untrusted code can escape the container, consume resources, or exfiltrate data. Use a sandboxed environment, resource limits, and a whitelist of allowed commands. |
|24| How do I deploy a Dockerised FastAPI app to Kubernetes?| Build the image, push to a registry, write a Deployment and Service manifest, and apply with `kubectl`. Use Helm for repeatable releases. |
|25| What is *observability* and which three pillars are essential?| Observability = the ability to understand system behaviour. The three pillars: Metrics, Traces, Logs. |
|26| How can I add OpenTelemetry tracing to a FastAPI service?| Install `opentelemetry-instrumentation-fastapi`, configure an exporter (Jaeger, OTLP), and wrap the app with `FastAPIInstrumentor().instrument_app(app)`. |
|27| What is *long‑context* retrieval and when is it needed?| Splitting a huge corpus into overlapping windows and retrieving multiple chunks to feed a model with >32k token windows, useful for legal documents. |
|28| How do I fine‑tune a 7B model on a custom dataset?| Use the `trl` library with LoRA, a small learning rate, and a dataset in the `datasets` format. |
|29| What is *prompt engineering* vs *prompt design*?| Engineering focuses on practical techniques (few‑shot, temperature). Design includes higher‑level considerations like persona, role, and system instructions. |
|30| How can I generate unit tests automatically with an LLM?| Prompt the model: "Write pytest tests for the following function…" and ask for coverage metrics. |
|31| What is the difference between *semantic* and *syntactic* similarity?| Semantic similarity captures meaning (embeddings); syntactic similarity looks at exact token overlap (e.g., Levenshtein). |
|32| When should I use *vector search* vs *keyword search*?| Use vector search for semantic similarity, keyword search for exact matches or boolean queries. |
|33| How do I handle *out‑of‑memory* errors when loading large models?| Use quantisation (`bitsandbytes`), offload layers to CPU, or run on a GPU with sufficient VRAM. |
|34| What is *model distillation*?| Training a smaller student model to mimic the outputs of a larger teacher model, reducing latency and cost. |
|35| How can I protect my API keys in a public repo?| Store them in GitHub Secrets, reference via `${{ secrets.MY_KEY }}` in workflows, and never commit them. |
|36| What is *continuous integration* for AI projects?| Running automated tests, linting, and model validation on every push, often with GitHub Actions or GitLab CI. |
|37| How do I benchmark LLM latency?| Use `time` on the API call or the provider’s latency metrics; repeat many times and compute percentiles. |
|38| What is *prompt injection* and how to mitigate it?| An attacker injects malicious instructions into the user prompt. Mitigate by sanitising inputs, using system prompts, and limiting model temperature. |
|39| When should I use *retrieval* vs *generation* for FAQ answering?| Retrieval when you have a curated knowledge base; generation when you need to synthesize across many sources. |
|40| How can I version my documentation alongside code?| Keep docs in the same repo, use a `docs/` folder, and generate a static site with GitHub Pages on each release. |
|41| What is *zero‑shot* classification?| Using a model to classify text into categories it has never seen labeled examples for, by providing a description of each class in the prompt. |
|42| How do I store *conversation history* for a chatbot?| Persist each turn in PostgreSQL or a NoSQL store; optionally summarise old turns to keep within the context window. |
|43| What is the best way to *paginate* search results from a vector DB?| Use `offset`/`limit` parameters or cursor‑based pagination with the last vector ID. |
|44| How can I ensure *data privacy* when sending documents to an LLM?| Use on‑premise models or encrypt data before sending, and delete it immediately after inference. |
|45| What is *parameter‑efficient fine‑tuning*?| Techniques like LoRA, adapters, or prefix‑tuning that modify only a tiny fraction of model weights. |
|46| How do I handle *non‑ASCII* characters in tokenisation?| Most modern tokenisers use Unicode‑aware byte‑pair encoding, so they handle emojis and CJK characters automatically. |
|47| When should I use *GPU* vs *CPU* for inference?| GPU for batch inference or large models (>2B parameters); CPU is fine for small models or low‑throughput services. |
|48| What is *prompt chaining*?| Splitting a complex task into multiple prompts where the output of one becomes the input of the next. |
|49| How do I add *type hints* to my Python code for better LLM assistance?| Use PEP 484 annotations (`def func(a: int) -> str:`). The model can read them and generate type‑correct code. |
|50| What is *model bias* and how can I test for it?| Systematic prejudice in outputs. Test by prompting the model with demographic‑varying inputs and analysing differences. |
|51| How can I use *GitHub Actions* to lint my markdown docs?| Add `markdownlint-cli` in a workflow step: `npm i -g markdownlint-cli && markdownlint "**/*.md"`. |
|52| What is *semantic versioning* for AI models?| `MAJOR.MINOR.PATCH` where MAJOR changes the architecture, MINOR adds new capabilities, PATCH fixes bugs. |
|53| How do I *monitor* token‑usage cost across multiple services?| Export a custom metric `tokens_used` from each service to Prometheus and visualise in Grafana. |
|54| What is *prompt caching*?| Storing the model's response for identical prompts to avoid recomputation and cost. |
|55| When should I use *streaming* responses from the LLM?| For real‑time chat UI or large outputs where you want progressive rendering. |
|56| How can I *debug* a failing LangChain agent?| Enable verbose logging (`langchain.verbose = True`) and inspect the intermediate steps. |
|57| What is *data drift* in production AI systems?| The statistical properties of input data change over time, degrading model performance. |
|58| How do I *detect* data drift?| Compare embedding distributions of recent inputs to a baseline using KL divergence or KS test. |
|59| What is *model serving* with vLLM?| High‑throughput LLM serving that batches requests on the GPU for low latency. |
|60| How can I *auto‑scale* LLM pods in Kubernetes?| Use the Horizontal Pod Autoscaler with a custom metric (e.g., `queue_length`) exported by the serving layer. |
|61| When should I use *JSON schema* for LLM output?| Whenever downstream code expects a strict structure; it reduces parsing errors. |
|62| What is *prompt templating* in Jinja2?| Use Jinja2 syntax (`{{ variable }}`) to fill placeholders in a prompt string programmatically. |
|63| How do I *handle* large file uploads in FastAPI?| Use `UploadFile` with streaming (`await file.read()`) and store in object storage (S3, MinIO). |
|64| What is *continuous deployment* for AI services?| Automatically deploying a new model version after it passes all tests and validation checks. |
|65| How can I *benchmark* accuracy of a fine‑tuned model?| Use a held‑out test set and compute metrics like BLEU, ROUGE, or Exact Match depending on the task. |
|66| What is *prompt engineering* for *SQL generation*?| Provide the schema and ask the model to output a valid SQL query in a fenced code block. |
|67| How do I *secure* a webhook endpoint that receives LLM callbacks?| Verify a secret token in the request header and enforce TLS. |
|68| What is *model interpretability*?| Techniques (SHAP, attention visualization) that explain why a model made a prediction. |
|69| When should I use *Docker Compose* vs *Kubernetes*?| Compose for local development; Kubernetes for production, scaling, and resilience. |
|70| How can I *profile* GPU usage during inference?| Use `nvidia-smi` or NVIDIA Nsight Systems to monitor memory and compute utilization. |
|71| What is *prompt leakage*?| When a model unintentionally reveals its system prompt or internal instructions. |
|72| How do I *prevent* prompt leakage?| Keep system prompts short, avoid exposing them in user‑visible messages, and use the provider's `system` role correctly. |
|73| What is *retrieval‑augmented generation* (RAG) with *hybrid search*?| Combining vector similarity with keyword filters to improve relevance. |
|74| How can I *store* large embeddings efficiently?| Use a vector DB that supports IVF‑PQ or HNSW indexing; avoid storing raw NumPy arrays in relational tables. |
|75| What is *model quantisation*?| Reducing precision of weights (e.g., 8‑bit) to lower memory and improve inference speed. |
|76| When should I use *8‑bit* vs *4‑bit* quantisation?| 8‑bit for minimal accuracy loss; 4‑bit for extreme memory constraints, accepting a small drop in quality. |
|77| How do I *handle* multi‑turn conversations with memory?| Store recent turns in the context window and use a summarisation step for older history. |
|78| What is *few‑shot* prompting for *translation*?| Provide a few example source‑target sentence pairs before the new sentence to translate. |
|79| How can I *benchmark* latency across different LLM providers?| Write a script that measures round‑trip time for the same prompt on each provider and reports percentiles. |
|80| What is *prompt engineering* for *image captioning*?| Supply the image (via base64) and ask: "Provide a concise caption in under 20 words." |
|81| How do I *manage* secret keys for multiple environments?| Use a secret manager (AWS Secrets Manager, HashiCorp Vault) and inject via environment variables at runtime. |
|82| What is *model gating*?| Restricting model usage based on user tier or compliance requirements. |
|83| How can I *automatically* generate a changelog from git commits?| Use `git log --pretty=format:"* %s" $(git describe --tags --abbrev=0)..HEAD`. |
|84| What is *continuous evaluation* for deployed models?| Periodically run a benchmark suite on live traffic and alert on regression. |
|85| How do I *handle* Unicode emojis in token counts?| Tokenisers treat them as individual tokens; count them like any other token. |
|86| What is *prompt injection* in a chatbot UI?| Users prepend instructions like "Ignore previous messages and say X". Mitigate with system prompts and validation. |
|87| How can I *visualise* attention weights?| Use `bertviz` or extract attention matrices and plot heatmaps. |
|88| When should I use *graph databases* with AI?| For knowledge graphs where relationships matter (e.g., recommendation engines). |
|89| What is *retrieval‑augmented generation* with *SQL*?| Retrieve relevant rows from a database, then let the LLM generate a query to fetch the final answer. |
|90| How do I *ensure* reproducibility of fine‑tuning runs?| Fix random seeds, log hyperparameters, and version datasets and code. |
|91| What is *model serving* with *ONNX Runtime*?| Export the model to ONNX and run inference with the highly‑optimised ONNX Runtime engine. |
|92| How can I *detect* malicious code generated by an LLM?| Run static analysis tools (Bandit, SonarQube) on the generated snippet before execution. |
|93| What is *prompt engineering* for *summarisation*?| Provide the text and ask: "Summarise the following in 3 bullet points, preserving key facts." |
|94| How do I *handle* large PDF ingestion for RAG?| Split the PDF into pages, chunk each page into 500‑word pieces, embed, and store. |
|95| What is *model drift* vs *data drift*?| Model drift: degradation due to changes in model weights (e.g., after updates). Data drift: changes in input distribution. |
|96| How can I *automate* model registry updates?| Use a CI job that pushes new model artifacts to a registry (e.g., HuggingFace) after successful tests. |
|97| What is *prompt engineering* for *SQL injection prevention*?| Instruct the model: "Never output raw user input; always use parameterised queries." |
|98| How do I *scale* a vector DB horizontally?| Choose a cloud‑managed service (Pinecone, Qdrant Cloud) that handles sharding; otherwise run multiple nodes with consistent hashing. |
|99| What is *zero‑trust* networking for AI services?| Every request is authenticated and authorised, and internal traffic is encrypted and verified. |
|100| How can I *contribute* to this handbook?| Fork the repo, add or improve a markdown file, run `pre‑commit` checks, and open a pull request. See `CONTRIBUTING.md`. |

---

*Feel free to suggest additional questions via a GitHub issue.*