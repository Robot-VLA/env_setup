# env_setup
Docker setup for experiments.

## Build Image
```bash
sudo docker build -t cuda-dev:12.4.1 .
```

## Start New Container
```bash
export REPOS_DIR=/path/to/your/repos
export DATASETS_DIR=/path/to/your/datasets

sudo docker run --gpus all -it --name cuda-dev \
  # --runtime=nvidia \ enable this if error on `libnvidia-ml.so.1`, see https://github.com/NVIDIA/nvidia-container-toolkit/issues/305#issuecomment-2916747627 for details
  --network=host \
  -e NVIDIA_VISIBLE_DEVICES=all \
  -e NVIDIA_DRIVER_CAPABILITIES=all \
  -v "$REPOS_DIR:/workspace/repos" \
  -v "$DATASETS_DIR:/workspace/datasets" \
  --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 \
  --restart unless-stopped \
  cuda-dev:12.4.1 /bin/bash
```

## Enter Existing Container
Use VS-Code `Remote - Containers` extension or run:
```bash
sudo docker exec -it cuda-dev /bin/bash
```

# Troubleshooting
- enable `--runtime=nvidia` in `docker run` if you encounter error related to `libnvidia-ml.so.1`.
- if VS-Code extension `Remote - Containers` has a `permission denied while trying...` error, try running
  ```bash
  sudo chmod 666 /var/run/docker.sock
  ```
  and reload VS-Code window.

  This is a security risk for the `docker` command but who cares when everyone can be sudo anyway...