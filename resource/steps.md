# Project Setup Steps

1. Initialize the project.

   ```bash
   uv init
   ```

   Sets up the project structure with basic files such as `pyproject.toml`, `main.py`, `README.md`, `.python-version`, and `.gitignore`.

2. Create the virtual environment.

   ```bash
   uv venv --python 3.13
   ```

   Creates a local virtual environment in `.venv` using Python 3.13.

3. Update `pyproject.toml`.

   ```toml
   [project]
   name = "quiz-platform"
   version = "0.1.0"
   description = "Interactive AI/ML quiz platform with coding practice, rubric grading, semantic evaluation, and progress tracking."
   readme = "README.md"
   requires-python = ">=3.13"
   dependencies = [
       "pandas>=2.3.0",
       "pydantic>=2.11.0",
       "sentence-transformers>=4.1.0",
       "sqlalchemy>=2.0.0",
       "streamlit>=1.45.0",
       "streamlit-ace>=0.1.1",
   ]

   [dependency-groups]
   dev = [
       "pytest>=8.4.0",
   ]
   ```

   Adds the quiz platform description, sets the Python requirement to `>=3.13`, and defines the initial dependencies for Streamlit UI, code editor support, data handling, validation, database access, semantic grading, and testing.

4. Update `.python-version`.

   ```text
   3.13
   ```

   Pins the project to Python `3.13` so `uv sync` uses a Python version compatible with `pyproject.toml`.

5. Create the documentation folder.

   ```bash
   mkdir -p resource
   ```

   Creates the `resource` folder for project documentation.

6. Create `resource/steps.md`.

   ```bash
   touch resource/steps.md
   ```

   Documents the reproducible setup commands with brief descriptions.

7. Create `resource/iterations.md`.

   ```bash
   touch resource/iterations.md
   ```

   Creates a placeholder file for detailed development notes in future iterations.

8. Install dependencies.

   ```bash
   uv sync
   ```

   Installs the dependencies declared in `pyproject.toml`, creates or updates `uv.lock`, and synchronizes the `.venv` environment.

9. Create the industry-grade project scaffold.

   ```bash
   mkdir -p \
     frontend/streamlit_app/{pages,components,state,utils} \
     frontend/web/src/{app,pages,api,hooks,lib,styles} \
     frontend/web/src/components/{ui,layout,charts} \
     frontend/web/src/features/{quiz,code_runner,grading,progress} \
     backend/app/{core,models,schemas,services,repositories,evaluators,code_runner,db,workers} \
     backend/app/api/routes \
     backend/tests \
     backend/alembic \
     data/{questions,seed,samples} \
     infra/{docker,compose,nginx} \
     scripts
   ```

   Creates separate areas for the Streamlit prototype, future React frontend, FastAPI backend, database migrations, question data, Docker/Compose infrastructure, and helper scripts.

   Note: this project uses `frontend/` instead of `apps/` because it reads more clearly beside `backend/`.

10. Create Docker and Docker Compose placeholders.

   ```bash
   touch \
     infra/docker/backend.Dockerfile \
     infra/docker/streamlit.Dockerfile \
     infra/docker/web.Dockerfile \
     infra/docker/worker.Dockerfile \
     infra/compose/docker-compose.dev.yml \
     infra/compose/docker-compose.prod.yml
   ```

   Prepares separate Dockerfiles for backend, Streamlit, React frontend, and worker services, plus separate Compose files for development and production-like environments.

11. Move Python application code into `src/quiz_platform/`.

   ```bash
   mkdir -p src/quiz_platform tests migrations
   mv frontend/streamlit_app src/quiz_platform/streamlit_app
   mv backend/app src/quiz_platform/backend
   mv backend/tests tests/backend
   mv backend/alembic migrations/alembic
   rmdir backend
   ```

   Makes backend and Streamlit code part of the importable `quiz_platform` Python package while keeping React, data, infrastructure, scripts, and documentation outside the package.

12. Make the project installable as a Python package.

   ```toml
   [build-system]
   requires = ["hatchling"]
   build-backend = "hatchling.build"

   [tool.uv]
   package = true

   [tool.hatch.build.targets.wheel]
   packages = ["src/quiz_platform"]
   ```

   Configures the project so `uv sync` installs `quiz_platform` into the virtual environment, giving clean imports from tests, scripts, and notebooks.

13. Re-sync and verify package imports.

   ```bash
   uv sync
   uv run python -c "print(__import__('quiz_platform').__name__)"
   uv run python -c "print(__import__('quiz_platform.backend').backend.__name__)"
   uv run python -c "print(__import__('quiz_platform.streamlit_app').streamlit_app.__name__)"
   ```

   Rebuilds the local package and confirms that `quiz_platform`, `quiz_platform.backend`, and `quiz_platform.streamlit_app` can be imported cleanly.

14. Add repository hygiene files.

   ```bash
   touch .env.example
   find . -type d -empty -not -path "./.git/*" -not -path "./.venv/*" -exec touch "{}/.gitkeep" \;
   ```

   Keeps empty scaffold folders trackable with `.gitkeep`, tracks a safe `.env.example` template, and ignores local files that should not be pushed, such as `.env`, `.venv`, caches, logs, local databases, notebook checkpoints, frontend build outputs, and editor files.
