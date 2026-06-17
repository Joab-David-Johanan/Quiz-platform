# Architecture Notes

## Q: Why move Python code into `src/quiz_platform/`?

A: This makes `quiz_platform` a real importable Python package. Instead of relying on the current working directory, code can import modules consistently from tests, scripts, Streamlit, FastAPI, and notebooks.

## Q: What problem does the `src/` layout solve?

A: The `src/` layout prevents accidental imports from random local folders. Python must import the installed package from the virtual environment, which is closer to how real applications run in production.

## Q: Why is this helpful for notebooks?

A: Notebooks often live in nested folders. Without an installed package, imports can break depending on where the notebook starts. With `quiz_platform` installed in the `.venv`, a notebook can import project code from anywhere:

```python
from quiz_platform.backend.services import grading_service
from quiz_platform.backend.evaluators import scoring
```

## Q: Why not put the React app inside `src/quiz_platform/`?

A: React is not Python package code. It has its own Node/Vite tooling, dependencies, build process, and deployment flow. Keeping it at `frontend/web/` avoids mixing Python packaging with JavaScript application structure.

## Q: What should live inside `src/quiz_platform/`?

A: Python application code should live there. That includes the FastAPI backend, Streamlit app, evaluators, services, repositories, schemas, code runner, and database helpers.

## Q: What should stay outside `src/quiz_platform/`?

A: Non-package assets should stay outside. That includes the React app, Docker files, Compose files, datasets, documentation, scripts, tests, and migrations.

## Q: Why keep tests outside the package?

A: Tests are not part of the application runtime. Keeping them in top-level `tests/` is a common convention and makes it clear they validate the package rather than belong inside it.

## Q: Why keep Alembic migrations outside the package?

A: Migrations are operational history for the database schema. They are important, but they are not normal importable application modules. Keeping them in `migrations/alembic/` makes their purpose clear.

## Q: What does `[tool.uv] package = true` do?

A: It tells uv this project should be treated as a package, not just a loose script project. During `uv sync`, uv installs the local project into the virtual environment.

## Q: What does Hatchling do here?

A: Hatchling is the build backend. It tells Python tooling how to build and install the package from `src/quiz_platform/`.

## Q: Why do clean imports matter?

A: Clean imports make the code easier to move, test, run, and reuse. For example, `from quiz_platform.backend.services.grading_service import grade_answer` is stable whether it is called from a test, a notebook, a Streamlit page, or a FastAPI route.

## Q: What is `.gitignore` for?

A: `.gitignore` tells Git which local or generated files should not be committed. This protects the remote repo from virtual environments, secrets, caches, logs, local databases, notebook checkpoints, frontend build outputs, and editor-specific files.

## Q: Why should `.env` be ignored?

A: `.env` often contains local secrets, API keys, passwords, database URLs, and machine-specific settings. It should stay local. The repo should track `.env.example` instead, which documents the required variables without exposing real values.

## Q: Why do we need `.env.example`?

A: `.env.example` is a safe template for local configuration. A developer can copy it to `.env` and fill in real values privately.

## Q: What is `.gitkeep` for?

A: Git does not track empty folders. A `.gitkeep` file is a harmless placeholder that lets us commit intentional empty scaffold directories before real files exist.

## Q: Should `uv.lock` be committed?

A: Yes. `uv.lock` should be committed for this application because it pins the exact dependency versions. That makes installs more reproducible across machines and future development sessions.

## Q: Why use `frontend/` instead of `apps/`?

A: `frontend/` is clearer for this project because the React app still lives outside the Python package. Since the FastAPI backend now lives at `src/quiz_platform/backend/`, `frontend/web/` clearly means the separate JavaScript frontend.

## Q: What is `src/quiz_platform/streamlit_app/` for?

A: This is the first prototype UI. Streamlit lets us build the quiz flow, coding editor, answer submission, and feedback screens quickly without spending early time on React setup. It is useful for learning, experimentation, and fast iteration.

## Q: What is `src/quiz_platform/streamlit_app/pages/` for?

A: This folder will hold Streamlit multipage screens, such as dashboard, quiz practice, review, or settings pages.

## Q: What is `src/quiz_platform/streamlit_app/components/` for?

A: This folder will hold reusable Streamlit UI pieces, such as question cards, score displays, feedback panels, code editor wrappers, and progress summaries.

## Q: What is `src/quiz_platform/streamlit_app/state/` for?

A: This folder will hold session-state helpers. Streamlit apps often need explicit state management for selected questions, submitted answers, current score, and quiz progress.

## Q: What is `src/quiz_platform/streamlit_app/utils/` for?

A: This folder will hold small frontend-specific helper functions, such as formatting scores, loading local demo data, or preparing display text.

## Q: What is `frontend/web/` for?

A: This is reserved for the future React frontend. Once the Streamlit prototype proves the product flow, React can become the polished user-facing interface.

## Q: Why keep both Streamlit and React?

A: Streamlit is best for quick experimentation. React is better for a polished production-style frontend with richer routing, reusable UI components, and more control over the user experience. We do not need to build both fully at the same time.

## Q: What is `frontend/web/src/app/` for?

A: This folder will hold app-level frontend setup, such as routing, providers, layouts, and global configuration.

## Q: What is `frontend/web/src/pages/` for?

A: This folder will hold top-level React pages, such as dashboard, quiz, question detail, review, and progress pages.

## Q: What is `frontend/web/src/features/` for?

A: This folder groups code by product feature. For example, quiz flow, code runner UI, grading display, and progress analytics can each have their own feature folder.

## Q: What is `frontend/web/src/components/` for?

A: This folder holds reusable UI components. The `ui/` folder is for generic primitives, `layout/` is for page structure, and `charts/` is for visual analytics.

## Q: What is `frontend/web/src/api/` for?

A: This folder will hold frontend API clients that call the FastAPI backend, such as question, attempt, grading, and code-run endpoints.

## Q: What is `frontend/web/src/hooks/` for?

A: This folder will hold reusable React hooks, such as hooks for loading questions, submitting attempts, running code, or reading progress data.

## Q: What is `frontend/web/src/lib/` for?

A: This folder will hold shared frontend utilities that are not React components, such as constants, formatting helpers, and small pure functions.

## Q: What is `frontend/web/src/styles/` for?

A: This folder will hold global CSS, theme tokens, and styling setup for the React frontend.

## Q: What is `src/quiz_platform/backend/` for?

A: This is where the FastAPI backend will live. It will own the API, business logic, grading pipeline, database access, code execution coordination, and background jobs.

## Q: What is `src/quiz_platform/backend/main.py` for?

A: This will be the FastAPI entrypoint. It will create the API application, register routes, configure middleware, and expose the app server.

## Q: What is `src/quiz_platform/backend/api/` for?

A: This folder holds the HTTP layer. It should define API routes and request handling, but avoid heavy business logic.

## Q: What is `src/quiz_platform/backend/api/routes/` for?

A: This folder will hold route files such as `questions.py`, `attempts.py`, `grading.py`, `code_runs.py`, and `health.py`.

## Q: What is `src/quiz_platform/backend/core/` for?

A: This folder will hold app-wide configuration and infrastructure concerns, such as environment settings, logging, security helpers, and constants.

## Q: What is `src/quiz_platform/backend/models/` for?

A: This folder will hold SQLAlchemy database models. These classes represent database tables such as users, questions, attempts, grades, and code runs.

## Q: What is `src/quiz_platform/backend/schemas/` for?

A: This folder will hold Pydantic schemas for request and response validation. Schemas define what the API accepts and returns.

## Q: What is `src/quiz_platform/backend/repositories/` for?

A: Repositories isolate database queries. This keeps SQLAlchemy code out of API routes and services, making the backend easier to test and change.

## Q: What is `src/quiz_platform/backend/services/` for?

A: Services hold business logic. For example, submitting an answer, grading an attempt, selecting the next question, or recording progress should live here.

## Q: What is `src/quiz_platform/backend/evaluators/` for?

A: Evaluators hold the answer-grading logic. This is where keyword scoring, semantic similarity, rule-based verdicts, and future LLM judge fallback will live.

## Q: What is `src/quiz_platform/backend/code_runner/` for?

A: This folder will hold code execution logic. Early on it may run local trusted code for practice, but later it should coordinate safer execution in isolated Docker containers.

## Q: What is `src/quiz_platform/backend/db/` for?

A: This folder will hold database setup, such as engine creation, sessions, base model configuration, and database dependencies.

## Q: Why do we need `src/quiz_platform/backend/workers/`?

A: Workers handle slow or risky jobs outside the main API request. Examples include semantic grading, LLM judging, code execution, report generation, and analytics updates. This keeps the API responsive.

## Q: What is Alembic?

A: Alembic is a database migration tool for SQLAlchemy. It tracks database schema changes over time, such as creating tables, adding columns, or changing indexes.

## Q: Why do we need `migrations/alembic/`?

A: This folder will store migration files. Instead of manually changing the database, we will create versioned migrations so the schema can be reproduced safely across machines and environments.

## Q: What is `tests/backend/` for?

A: This folder will hold backend tests. We will use it to test API routes, services, evaluators, repositories, and database behavior.

## Q: What is `data/questions/` for?

A: This folder will hold question-bank files, likely in JSONL format at first. Each record can define a prompt, topic, difficulty, expected answer, rubric, and starter code.

## Q: What is `data/seed/` for?

A: This folder will hold seed data used to populate the database during development, such as starter questions, topics, or demo users.

## Q: What is `data/samples/` for?

A: This folder will hold sample datasets for coding exercises, such as small CSV files or generated data used in pandas and ML questions.

## Q: What is `infra/` for?

A: This folder holds infrastructure configuration. It separates deployment and runtime setup from application code.

## Q: Why do we need Docker?

A: Docker packages an application with its runtime environment. It helps make the backend, frontend, worker, and Streamlit app run consistently across machines.

## Q: Why do we need Docker Compose?

A: Docker Compose runs multiple services together. For this project, Compose can start FastAPI, React, Streamlit, PostgreSQL, Redis, and workers with one command.

## Q: What is `infra/docker/` for?

A: This folder will hold Dockerfiles for each service, such as backend, Streamlit, React frontend, and worker.

## Q: What is `infra/compose/` for?

A: This folder will hold Docker Compose files. The development Compose file can run local hot-reload services, while the production-like Compose file can be stricter and closer to deployment.

## Q: What is `infra/nginx/` for?

A: This folder is reserved for future reverse proxy configuration. Nginx can route traffic to the frontend and backend in production-like setups.

## Q: What is `scripts/` for?

A: This folder will hold developer scripts, such as seeding the database, exporting questions, creating demo users, or running maintenance tasks.

## Q: Why separate API routes, services, and repositories?

A: This separation keeps the code easier to maintain. Routes handle HTTP, services handle business rules, and repositories handle database access. Each layer has a focused job.

## Q: What should we build first?

A: Build the Streamlit quiz prototype first. Then add a small question bank, simple grading, and local progress tracking. Once the flow feels right, move core logic into FastAPI and use the backend from Streamlit or React.
