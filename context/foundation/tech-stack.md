---
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
---

## Why this stack

Solo learner building RabbitTracker on a 5-week after-hours timeline targeting medium scale (dozens to ~100 users). Django is the recommended default for `(web-app, python)` and ships auth, ORM, migrations, and a PostgreSQL-backed persistence layer from day one — covering the registration and login/logout functional requirements without additional assembly. Background post-generation (the product's core async NFR) follows the standard Django + Celery pattern, the most-documented background-job path in the Python ecosystem. Content generation (the LLM-driven decomposition of a submitted keyword into bite-sized educational posts) is a library-level integration — Anthropic or OpenAI SDK sits next to Django's views and task runners without forcing a framework switch. Bootstrapper confidence is verified, so scaffolding will be smooth. CI on GitHub Actions with auto-deploy on merge matches the solo, after-hours flow. One compensation note for agent-assisted development: Django's templates are untyped by default; adding Django REST Framework or django-ninja at HTTP boundaries and mypy in CI closes the agent-friendliness gap — bootstrapper will add this to the project's instruction file.
