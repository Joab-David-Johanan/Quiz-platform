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
