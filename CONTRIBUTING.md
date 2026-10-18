# Contributing to GM/GN Bot

Thanks for taking the time to improve the project. Prefer focused pull requests with a clear problem statement and testable changes.

## Before opening a pull request

1. Explain what changed and why.
2. Update both `README.md` and `README.en.md` when user-facing commands, configuration, or behavior changes.
3. Keep documentation images in sync with the steps or options they illustrate.
4. Never include real account tokens, cookies, credentials, personal data, production channel IDs, or private logs.
5. For new third-party code or assets, add upstream links and license/attribution notes to [`docs/SOURCES.md`](docs/SOURCES.md).

## Local quality checks

Use Python 3.10 or newer. From the repository root:

```bash
pip install -r requirements-dev.txt
ruff check .
ruff format --check .
python -m compileall -q src
python scripts/check_docs.py
```

The repository currently does not contain an automated test suite. Do not describe linting or compilation as behavior tests. Add focused regression tests when contributing testable business logic, and ensure they can run without real Discord credentials or production services.

## Pull request checklist

- [ ] The change has a clear purpose and summary.
- [ ] Secrets and production identifiers are not included.
- [ ] Indonesian and English user-facing documentation stays consistent.
- [ ] `ruff check .` passes.
- [ ] `ruff format --check .` passes.
- [ ] `python -m compileall -q src` passes.
- [ ] `python scripts/check_docs.py` passes.
- [ ] New sources, assets, and dependency/license considerations are documented.

## Important platform note

The current implementation uses a user-account client (self-bot), which Discord says is not permitted. The repository documents this existing limitation for transparency; contributors should not add mechanisms intended to evade platform enforcement. Prefer a migration to an official Discord bot application for supported deployments.
