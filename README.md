# Quiz Platform

Interactive AI/ML quiz platform for practicing concepts, writing code, running examples, submitting answers, and receiving layered feedback.

The long-term goal is to build a portfolio-grade learning system that combines coding practice, rubric-based grading, semantic answer evaluation, and progress tracking.

## Project Goals

- Build an interactive quiz experience for AI/ML and data science topics.
- Provide a coding interface where learners can run Python snippets and inspect output.
- Grade descriptive answers with a layered evaluation pipeline.
- Track attempts, scores, weak areas, and learning progress.
- Grow from a simple Streamlit prototype into a cleaner backend-driven application.

## Planned Evaluation Pipeline

Answers should be evaluated in layers:

1. Keyword and rubric checks for required concepts.
2. Sentence-transformer similarity for semantic understanding.
3. Rule-based scoring to decide clear correct or incorrect answers.
4. LLM judge only for borderline, long-form, or ambiguous answers.

This keeps grading cheaper, faster, and more explainable than sending every answer directly to an LLM.

## Tech Stack

- Python 3.13
- uv for project and dependency management
- Streamlit for the first UI
- streamlit-ace for the coding editor
- pandas for sample datasets and data exercises
- Pydantic for structured data validation
- SQLAlchemy for future database access
- sentence-transformers for semantic grading
- pytest for tests

## Current Status

The project is in the initial setup phase.

Completed:

- Created the uv project.
- Created a Python 3.13 virtual environment.
- Added initial dependencies.
- Generated `uv.lock`.
- Added reproducible setup notes in `resource/steps.md`.
- Added an empty `resource/iterations.md` file for future detailed iteration notes.

## Setup

Install dependencies:

```bash
uv sync
```

Run the starter script:

```bash
uv run python main.py
```

The setup history is documented in `resource/steps.md`.

## Project Structure

```text
.
|-- main.py
|-- pyproject.toml
|-- README.md
|-- uv.lock
`-- resource
    |-- iterations.md
    `-- steps.md
```

## Roadmap

This section should be updated after every major iteration.

- [x] Initialize project with uv.
- [x] Create Python 3.13 virtual environment.
- [x] Add initial dependencies.
- [x] Document reproducible setup steps.
- [ ] Create the first Streamlit app shell.
- [ ] Add a small JSONL question bank.
- [ ] Add a basic quiz flow.
- [ ] Add a code editor with `streamlit-ace`.
- [ ] Add local code execution for practice mode.
- [ ] Add basic keyword/rubric grading.
- [ ] Store attempts and scores in SQLite.
- [ ] Add a progress dashboard.
- [ ] Add semantic grading with sentence-transformers.
- [ ] Add FastAPI backend when the prototype is stable.
- [ ] Move persistence to PostgreSQL.
- [ ] Add Docker-based sandboxing for safer code execution.
- [ ] Add LLM judge fallback for borderline descriptive answers.

## Documentation

- `resource/steps.md`: short reproducible commands and setup notes.
- `resource/iterations.md`: detailed development notes for future iterations.

## Development Notes

Start small and keep each version working before adding the next layer. The first milestone should be a usable Streamlit quiz with a tiny question bank, code editor, and simple scoring.
