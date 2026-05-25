---
bootstrapped_at: 2026-05-25T16:50:13Z
starter_id: django
starter_name: Django
project_name: rabbit-tracker
language_family: python
package_manager: uv
cwd_strategy: native-cwd
bootstrapper_confidence: verified
phase_3_status: ok
audit_command: pip-audit
---

## Hand-off

Verbatim copy of `context/foundation/tech-stack.md` frontmatter:

```yaml
starter_id: django
package_manager: uv
project_name: rabbit-tracker
hints:
  language_family: python
  team_size: solo
  deployment_target: fly
  ci_provider: github-actions
  ci_default_flow: auto-deploy-on-merge
  bootstrapper_confidence: verified
  path_taken: standard
  quality_override: false
  self_check_answers: null
  has_auth: true
  has_payments: false
  has_realtime: false
  has_ai: true
  has_background_jobs: true
```

### Why this stack

Solo learner building RabbitTracker on a 5-week after-hours timeline targeting medium scale (dozens to ~100 users). Django is the recommended default for `(web-app, python)` and ships auth, ORM, migrations, and a PostgreSQL-backed persistence layer from day one — covering the registration and login/logout functional requirements without additional assembly. Background post-generation (the product's core async NFR) follows the standard Django + Celery pattern, the most-documented background-job path in the Python ecosystem. Content generation (the LLM-driven decomposition of a submitted keyword into bite-sized educational posts) is a library-level integration — Anthropic or OpenAI SDK sits next to Django's views and task runners without forcing a framework switch. Bootstrapper confidence is verified, so scaffolding will be smooth. CI on GitHub Actions with auto-deploy on merge matches the solo, after-hours flow. One compensation note for agent-assisted development: Django's templates are untyped by default; adding Django REST Framework or django-ninja at HTTP boundaries and mypy in CI closes the agent-friendliness gap — bootstrapper will add this to the project's instruction file.

## Pre-scaffold verification

| Signal      | Value                                          | Severity | Notes                                                              |
| ----------- | ---------------------------------------------- | -------- | ------------------------------------------------------------------ |
| npm package | not run                                        | n/a      | non-JS starter (python); cmd_template invokes django-admin, not an npm create CLI |
| GitHub repo | not run                                        | n/a      | card docs_url is `https://docs.djangoproject.com` — not a github.com/<owner>/<repo> URL, so no recency signal available |

No recency signal available for this starter. Proceeded with no warning (WARN-AND-CONTINUE).

## Scaffold log

**Resolved invocation**: `uvx --from django django-admin startproject rabbit_tracker .`
**Strategy**: native-cwd
**Exit code**: 0
**Pre-flight files-to-touch**: manage.py, rabbit_tracker/__init__.py, rabbit_tracker/settings.py, rabbit_tracker/urls.py, rabbit_tracker/asgi.py, rabbit_tracker/wsgi.py
**Files written by CLI**: 6
**Pre-existing files preserved**: .agents/, .claude/, .git/, .gitignore, .idea/, CLAUDE.md, context/, project-idea-notes.md, README.md, skills-lock.json

Notes:
- The card's `cmd_template` is `django-admin startproject {name} .`. Under native-cwd the trailing `.` already targets the current directory, so `{name}` was substituted with the Django project module name rather than `.` (a literal `.` is rejected by django-admin as an invalid project name). The hand-off `project_name` `rabbit-tracker` contains a hyphen, which is not a valid Python module identifier, so it was normalized to `rabbit_tracker` for the module name.
- `django-admin` was not on PATH; it was provided ephemerally via `uvx --from django`, honoring the chosen `uv` package manager and the card's `pre: pip install django` intent without polluting the global environment. Django 6.0.5 was used.
- No conflicts: none of the written paths collided with pre-existing files, so no `.scaffold` siblings were created. `context/` preserved verbatim.

## Post-scaffold audit

**Tool**: pip-audit (run as `uvx pip-audit --format json`)
**Summary**: 0 CRITICAL, 0 HIGH, 0 MODERATE, 0 LOW
**Direct vs transitive**: not distinguished by this tool

No known vulnerabilities found.

**Caveat (audit scope)**: Django's `startproject` produces no dependency manifest (`requirements.txt` / `pyproject.toml`) and no project virtualenv exists yet. As a result, `pip-audit` audited the available Python environment (the ephemeral `uvx` environment containing pip-audit and its own dependencies — 28 packages), **not** the project's Django dependency. A meaningful project-dependency audit requires pinning dependencies first (e.g., `uv init` / a `pyproject.toml` declaring Django) and re-running `pip-audit` against that resolved tree. The clean result above reflects the tool's environment, not the project's eventual dependency set.

## Hints recorded but not acted on

| Hint                    | Value                |
| ----------------------- | -------------------- |
| bootstrapper_confidence | verified             |
| quality_override        | false                |
| path_taken              | standard             |
| self_check_answers      | null                 |
| team_size               | solo                 |
| deployment_target       | fly                  |
| ci_provider             | github-actions       |
| ci_default_flow         | auto-deploy-on-merge |
| has_auth                | true                 |
| has_payments            | false                |
| has_realtime            | false                |
| has_ai                  | true                 |
| has_background_jobs     | true                 |

v1 surfaces these but takes no automated action. In particular, the `has_*` feature flags (auth, ai, background-jobs) and CI/deployment hints are deliberately not scaffolded in v1 — that responsibility, along with the "untyped Django templates" compensation note from the hand-off body (DRF/django-ninja at HTTP boundaries + mypy in CI), moves to the future M1L4 ("Memory Architecture") skill.

## Next steps

Next: a future skill will set up agent context (CLAUDE.md, AGENTS.md). For now, your project is scaffolded and verified — happy hacking.

Useful manual steps in the meantime:
- `git init` is not needed — this directory already has a `.git/` repo; your existing history is untouched.
- No `.scaffold` siblings were created (no conflicts), so there is nothing to reconcile.
- Pin dependencies (`uv init` + add Django to `pyproject.toml`) so future `pip-audit` runs audit the real project tree, then address findings per your project's risk tolerance.
- Per the hand-off's compensation note, consider adding django-ninja or DRF at HTTP boundaries plus mypy in CI to close the agent-friendliness gap on Django's untyped defaults.
