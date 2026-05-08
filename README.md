#AI-Generated GitHub Actions CI/CD Pipeline

**Domain:** Healthcare Claims Processing

## Objective

Use AI to generate a GitHub Actions YAML workflow for automating healthcare ETL pipeline deployments with linting, testing, and coverage reporting.

## Architecture

```
Hospital DB → Python ETL → Azure SQL
```

## Tools Used

- **GitHub Actions** - CI/CD automation platform
- **Python** - ETL scripting language
- **flake8** - Python linting tool
- **pytest** - Python testing framework
- **pytest-cov** - Coverage reporting plugin

## Prerequisites

- GitHub account
- Git installed locally
- Python 3.11+ installed

## Setup Instructions

### Step 1: Create GitHub Repository

```bash
git init
git remote add origin https://github.com/<username>/<repo-name>.git
```

### Step 2: Create Workflow Folder

```bash
mkdir -p .github/workflows
```

### Step 3: Create CI Workflow File

Create `.github/workflows/ci.yml` with the following content:

```yaml
name: Healthcare CI

on:
  pull_request:
    branches: [main, master]

jobs:
  lint:
    name: Lint with flake8
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      - name: Install flake8
        run: pip install flake8

      - name: Run flake8
        run: flake8 . --count --max-line-length=100 --show-source --statistics

  test:
    name: Test with pytest
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
          pip install pytest pytest-cov flake8

      - name: Run lint
        run: flake8 . --count --max-line-length=100 --show-source --statistics

      - name: Run tests with coverage
        run: pytest --cov=. --cov-report=xml --cov-report=term-missing -v

      - name: Upload coverage report
        uses: actions/upload-artifact@v4
        with:
          name: coverage-report
          path: coverage.xml
          retention-days: 7
```

### Step 4: Create Requirements File

Create `requirements.txt`:

```
pandas
pytest
pytest-cov
flake8
```

### Step 5: Push Code to GitHub

```bash
git add .
git commit -m "Added CI pipeline"
git push origin master
```

**Note:** If your local branch is `master` but GitHub expects `main`:
```bash
git branch -m main
git push origin main
```

### Step 6: Validate Workflow

1. Open your GitHub repository in a browser
2. Navigate to **Actions** tab
3. Create a pull request to trigger the workflow
4. Verify both `lint` and `test` jobs execute successfully

## Workflow Features

| Feature | Description |
|---------|-------------|
| **PR Trigger** | Workflow runs on pull requests to `main` or `master` |
| **Parallel Jobs** | Lint and test jobs run concurrently for faster feedback |
| **Python 3.11** | Explicit Python version ensures consistency |
| **flake8 Linting** | Enforces code style with 100-character line limit |
| **pytest** | Runs unit tests with verbose output |
| **Coverage Reporting** | Generates XML and terminal coverage reports |
| **Artifact Upload** | Stores coverage report for 7 days |

## Expected Output

- Automated CI/CD pipeline executing on every PR
- PR validation with status checks
- Coverage reporting with downloadable artifacts
- Failed checks block PR merge (if branch protection enabled)

## Learning Outcomes

Upon completion of this lab, you will understand:

- How to structure GitHub Actions workflow files
- YAML syntax for CI/CD pipelines
- Integrating Python linting tools (flake8)
- Configuring pytest with coverage reporting
- Using GitHub Actions artifacts for report storage
- Branch protection and PR validation workflows

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `src refspec main does not match any` | Use `git push origin master` or rename branch to `main` |
| Workflow not triggering | Ensure file is in `.github/workflows/` directory |
| `flake8: command not found` | Add `pip install flake8` to dependencies step |
| `pytest: command not found` | Add `pytest` to `requirements.txt` |
| Coverage report not generated | Ensure `pytest-cov` is installed |

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [flake8 Documentation](https://flake8.pycqa.org/)
- [pytest Documentation](https://docs.pytest.org/)
