# harness-deployment

Contains the deployment pipeline for the harness.

## Configuration

Copy `.env.example` to `.env` and set deployment-specific values. Behavioral
tuning is kept with the backend in `infrastructure/config.yaml`; Compose only
passes secrets, service addresses, writable paths, logging overrides, and the
models that it must pull.

| File | Responsibility |
| --- | --- |
| `.env` | Machine/deployment values and secrets |
| `docker-compose.yml` | CPU-compatible service topology |
| `docker-compose.gpu.yml` | Optional NVIDIA override |
| `LiteLLM_config.yaml` | LiteLLM model aliases and routing |
| Backend `infrastructure/config.yaml` | Agent, gateway, logging, and memory tuning |

Do not commit `.env`. This repository currently tracks that file, so `.gitignore`
alone will not protect future edits; untrack it separately before committing
secret changes. Extra legacy variables are harmless but no longer read by the
backend; use `.env.example` as the current supported surface.

## GPU acceleration

The base Compose stack remains compatible with machines that do not have a
GPU. On a machine with NVIDIA container support, add the GPU override:

```sh
docker compose -f docker-compose.yml -f docker-compose.gpu.yml up --detach --build
```

Tauri development detects NVIDIA GPUs automatically and uses this override
when available. Set `HARNESS_GPU=off` to force CPU mode or `HARNESS_GPU=on` to
require GPU mode. The default, `auto`, falls back to CPU if the GPU-enabled
stack cannot start.
