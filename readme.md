# Agentic Python Debugger

Status: In development

## Overview

Agentic Python Debugger is a multi-prompt, retrieval-augmented generation (RAG) AI agent designed to help debug, understand, and generate Python code — with a focus on Django and Flask projects. The agent is intended to assist developers by combining automated static checks, targeted retrieval of project context (code, tests, docs), and large-language-model (LLM) prompts to propose diagnostics, fixes, and explanations.

## Goals

- Speed up debugging by surfacing relevant code context and suggested fixes.
- Provide concise, actionable explanations for errors and unexpected behavior.
- Support iterative developer workflows (investigate → suggest → test → refine).
- Integrate with common Python web frameworks (Django, Flask) and typical project layouts.

## Key features (planned / in progress)

- Multi-prompt orchestration to combine diagnostics, targeted code retrieval, and patch generation.
- Retrieval-augmented context: index repository code, tests, and docs for focused LLM prompts.
- Support for framework-specific analysis (Django/Flask routing, models, views).
- Test-run integration to reproduce failures and collect stack traces for analysis.
- Configurable LLM backend and vector store for embeddings/search.
- CLI and programmatic interfaces for interactive and automated workflows.

## Architecture (high level)

- Indexing layer: extracts and indexes repository files (source, tests, docs) into a vector store.
- Retriever: given an error, test failure, or developer query, returns a set of relevant source snippets.
- Orchestrator / Agent: runs multiple prompts (diagnostics, explanation, patch generation), combining retriever results and tooling (linters, test runner).
- LLM backend: any provider that supports text generation and embeddings (configurable).
- Tooling integration: test runner (pytest), static analysis (flake8, mypy), and optional runtime instrumentation.

## Supported frameworks

- Django (projects and apps)
- Flask

Support for other Python frameworks should be possible by adding framework-specific prompt/inspection modules.

## Getting started

Prerequisites
- Python 3.8+ (3.10+ recommended)
- Virtual environment tooling (venv, pipenv, or poetry)
- An LLM provider account (e.g., OpenAI) and API key if you plan to use a hosted LLM

Installation (example)
1. Clone the repository:
   git clone https://github.com/Akinfiresoye-Victor/agentic_python_debugger.git
2. Create and activate a virtual environment:
   python -m venv .venv
   source .venv/bin/activate  # macOS/Linux
   .venv\Scripts\activate     # Windows
3. Install dependencies:
   pip install -r requirements.txt

Configuration
- Keep secrets out of source control and put them in a `.env` file or your environment.
- Example `.env` variables (adjust for your implementation):
  ```
  # .env (example)
  OPENAI_API_KEY=sk-xxx
  LLM_MODEL=gpt-4o         # or another model name
  EMBEDDINGS_MODEL=text-embedding-3
  VECTOR_STORE_PATH=./vectorstore
  DATABASE_URL=sqlite:///dev.db
  LOG_LEVEL=INFO
  ```
- If you use a hosted vector DB (Pinecone, Weaviate, Milvus, etc.), configure connection details instead of `VECTOR_STORE_PATH`.

Usage (examples)
- Run the agent in interactive mode (adjust entrypoint to your project structure):
  python -m agentic_debugger.cli
  or
  python main.py --project-path /path/to/project --mode analyze --target-file path/to/file.py

- Analyze a failing test:
  1. Run tests locally to reproduce failure:
     pytest tests/my_test.py::test_something -q
  2. Provide the failing test ID or stack trace to the agent (via CLI or programmatic API) to get diagnostics and suggested fixes.

- Generate a patch (example conceptual flow):
  1. Agent examines retriever results + failure context.
  2. Agent proposes code changes as a patch file or diff.
  3. Developer reviews and applies the patch.

Running tests
- If the project includes tests, run:
  pytest -q
- Add tests for new features and components (retriever, orchestrator, prompt templates, etc.).

Development notes

- Project layout suggestions:
  - agentic_debugger/
    - cli.py                # CLI entrypoint
    - core/                 # orchestrator, prompt manager, agent logic
    - retriever/            # indexing, embedding, vector store interface
    - integrations/         # framework-specific helpers (django, flask)
    - tests/                # automated tests
    - requirements.txt
    - README.md

- Logging: use structured logs and sensible default log level; make it configurable via env or CLI flags.
- Prompt templates: keep prompt templates in separate files for easier tuning and testing.

Contributing

- Open an issue to discuss major changes or features first.
- Fork the repository and open a pull request for contributions.
- Follow existing code style and add tests for new functionality.
- Document configuration and usage for new modules.

Roadmap

- Core: robust retriever + agent orchestration
- Integrations: deep Django & Flask inspection helpers
- Testing: end-to-end workflows for reproducing and fixing errors
- UX: a simple web UI or richer CLI to guide debugging sessions

Security and configuration

- Do not commit secrets (API keys, service credentials).
- Use .env or platform secret managers for sensitive values.
- Sanitize any source code or environment content before sending to third-party LLM services if privacy is a concern.

License

- No license has been specified. Add a LICENSE file (for example, MIT or Apache-2.0) if you want to permit public use and contributions.

Contact

- Repository: https://github.com/Akinfiresoye-Victor/agentic_python_debugger
- For feature requests or issues, open an issue in the repository.

Acknowledgements / references

- Retrieval-augmented generation (RAG) patterns
- LLM prompt engineering best practices
- Standard Python testing and linting tools (pytest, flake8, mypy)
