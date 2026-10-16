<img src="docs/banner.svg" alt="AgentPGO backend — trace profiling and evaluation" width="100%">

# AgentPGO

Related to **[AgentPGO, built in collaboration with Aurorium Nexus](https://github.com/Auro-rium/agent-PGO)**. This repository holds my backend development work.
### Profile-guided optimization for AI agents.

A backend project for collecting agent traces, profiling cost and latency, evaluating runs, and comparing optimization candidates before exporting recommendations.

**Python · FastAPI · SQLAlchemy · OpenTelemetry · TypeScript SDK**

**Status:** development in progress on the default `backend-v1` branch. The repository contains backend services, SDKs, infrastructure, tests, and a separate landing page. A public backend deployment has not been verified.

## Inside the system

| Area | Source |
| --- | --- |
| API, authentication, and trace ingestion | [apps/api/](apps/api/) |
| Python and TypeScript connectors | [packages/](packages/) |
| Profiling, evaluation, and optimization | [services/](services/) |
| Command-line interface | [cli/](cli/) |
| Database migrations | [migrations/](migrations/) |
| Infrastructure definitions | [infra/terraform/](infra/terraform/) |
| Architecture notes | [docs/backend-architecture.md](docs/backend-architecture.md) |

The API accepts OTLP JSON at `/v1/traces` and `/v1/otlp/v1/traces`. Protected operations require tenant-scoped credentials. Connector instrumentation is metadata-only by default and fails open if export is unavailable.

## Try the backend locally

1. Clone the `backend-v1` branch and create a Python 3.11+ virtual environment using the commands below.
2. Install the development extras and run the test suite before starting the API.
3. Open the API docs at `http://127.0.0.1:8000/docs` after Uvicorn starts. Configure the environment and database using the project architecture notes before sending trace data.
4. Instrument a development agent with the Python or TypeScript connector. Send OTLP JSON to `/v1/traces` or `/v1/otlp/v1/traces`, then inspect the resulting trace and profiling records.
5. Compare recorded runs and export recommendations only after reviewing the underlying reports and measurements.

The README commands start a development API only; they do not provision a production database, tenant credentials, or a hosted service. Historical benchmark replay is a proxy, not a live provider benchmark.

## Local development

Use Python 3.11+:

```sh
git clone --branch backend-v1 https://github.com/AyushKJha/PGO_work.git
cd PGO_work
python -m venv .venv
```

Activate the environment (`.venv\Scripts\Activate.ps1` on Windows or `source .venv/bin/activate` on macOS/Linux), then install:

```sh
python -m pip install -e ".[dev]"
python -m pytest -q
uvicorn apps.api.main:app --reload
```

Read the [backend architecture](docs/backend-architecture.md), [migration notes](migrations/README.md), and [Python SDK guide](packages/sdk-py/README.md) for configuration and integration. Starting the server alone does not provision a hosted service or production database.

## Evaluation boundaries

Benchmark replay uses historical reports and is a proxy measurement. It should not be presented as a new live provider benchmark or a measured production improvement. Live provider runs require separate credentials and recorded usage/latency evidence.

The codebase includes billing and deployment components; their presence does not establish that a production service or payment integration is live.

[Development and benchmark notes](DEVELOPMENT.md) · [Local AWS validation](docs/aws/local-validation.md)
