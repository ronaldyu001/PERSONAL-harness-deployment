# harness-deployment

Contains the deployment pipeline for the harness.

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
