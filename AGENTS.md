# Agent Instructions for docker-socket-proxy

## Build/Lint/Test Commands
- **Install dependencies**: `poetry install`
- **Run all tests**: `pytest` (runs in parallel with `-n auto`)
- **Run single test**: `pytest tests/test_service.py::test_function_name`
- **Lint**: `flake8`
- **Format**: `black . && isort .`
- **Pre-commit**: `pre-commit run --all-files`

## Code Style Guidelines
- **Python version**: 3.8+
- **Formatting**: Black (88 char line length, 4-space indentation)
- **Imports**: isort with black profile (no unused imports)
- **Linting**: flake8 (ignores E203,E501,W503,B950)
- **Naming**: snake_case for functions/variables, PascalCase for classes
- **Error handling**: Use logging, raise appropriate exceptions
- **Testing**: pytest with descriptive test names, fixtures for setup
- **Resources**: Use context managers for file/Docker operations
- **Documentation**: No inline comments unless complex logic