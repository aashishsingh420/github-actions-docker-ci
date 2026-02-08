
# GitHub Actions + Docker CI (FastAPI Example)

This repository demonstrates a **complete, real-world CI workflow** using:

- Python (FastAPI)
- Docker
- GitHub Actions

The goal is not to build a complex application, but to **prove the full lifecycle**:
code → container → CI validation → clean execution on a fresh machine.

---

## What this project does

This project exposes a **small HTTP API** with one endpoint:

GET /add?a=&b=

Example:

```bash
curl "http://localhost:8000/add?a=2&b=3"

Response:

{
  "a": 2,
  "b": 3,
  "result": 5
}

The simplicity is intentional.
It allows the focus to stay on CI/CD mechanics, not application complexity.

⸻

Why this project exists

This repo exists to answer these questions confidently:
	•	How does GitHub Actions work end-to-end?
	•	What is a runner, really?
	•	How does Docker get source code during CI?
	•	How is CI different from running Docker locally?
	•	How do you validate a running container automatically?

Everything here is designed to be explainable in an interview.

⸻

Repository structure

.
├── app/
│   ├── __init__.py
│   └── main.py
├── Dockerfile
├── requirements.txt
├── .dockerignore
└── .github/
    └── workflows/
        └── ci.yml

What each part does
	•	app/
Application source code (FastAPI service)
	•	Dockerfile
Defines how the application is packaged into a container
	•	requirements.txt
Declares Python dependencies explicitly
	•	.dockerignore
Prevents local artifacts from being copied into the image
	•	.github/workflows/ci.yml
GitHub Actions workflow that builds, runs, and validates the container

⸻

Running the app locally (without Docker)

This step exists to prove the app works before containerization.

python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --host 0.0.0.0 --port 8000

Test it:

curl "http://localhost:8000/add?a=5&b=7"


⸻

Running the app with Docker (locally)

Build the image:

docker build -t github-actions-docker-ci:local .

Run the container:

docker run --rm -p 8000:8000 github-actions-docker-ci:local

Test it:

curl "http://localhost:8000/add?a=10&b=20"

This proves the container behaves identically to the local app.

⸻

CI pipeline overview (GitHub Actions)

The CI pipeline runs automatically on:
	•	every push to main
	•	every pull request targeting main

What CI does, step by step
	1.	Spins up a fresh Linux runner
	2.	Checks out the repository
	3.	Builds the Docker image
	4.	Starts the container
	5.	Calls the /add endpoint
	6.	Fails if the response is incorrect
	7.	Cleans up the container

No local state is reused.
If CI passes, the build is reproducible.

⸻

CI workflow (high level)

Checkout code
↓
Docker build
↓
Docker run
↓
HTTP validation (curl)
↓
Cleanup

This mirrors real production CI, not a mocked example.

⸻

Why Docker is used here

Docker ensures:
	•	the same runtime everywhere
	•	no reliance on developer machines
	•	reproducible builds in CI
	•	clear separation between build and execution

CI runs the same Docker commands you would run locally.

⸻

What this project intentionally avoids
	•	Kubernetes
	•	cloud deployment
	•	secret management
	•	complex application logic

Those are separate concerns and would hide the fundamentals.

⸻

Key takeaways

After understanding this repo, you should be able to explain:
	•	how GitHub Actions executes jobs
	•	what a runner is
	•	how Docker build context works
	•	how COPY pulls files into images
	•	how CI validates running containers
	•	how to debug failing pipelines
	•	how to debug failing pipeline- add 1

⸻

Status

✅ CI passing
✅ Dockerized
✅ Validated via HTTP
✅ Reproducible on clean runners

⸻
⸻
