```
```bash
services:
  comfyui:
    container_name: comfyui
    environment:
      - NVIDIA_VISIBLE_DEVICES=all
      - NVIDIA_DRIVER_CAPABILITIES=all,compute,utility
    image: hikage/comfyui-docker:latest
    ports:
      - '8188:8188'
    restart: unless-stopped
    runtime: nvidia
    stdin_open: True
    tty: True
    volumes:
      # persist your models:
      - /mnt/mnt/comfyui/models:/opt/comfyui/models

      # persist your input content:
      - /mnt/mnt/comfyui/input:/input

      # persist your output content:
      - /mnt/mnt/comfyui/output:/output

      # persist your custom_nodes content:
      - /mnt/mnt/comfyui/custom_nodes:/custom_nodes

      # persist user settings:
      - /mnt/mnt/comfyui/settings:/settings

x-notes: >
  # comfyui-docker

  **Source** :
  [https://github.com/Elenedeath/ComfyUI-Docker](https://github.com/Elenedeath/ComfyUI-Docker)

  **Image Docker** : hikage/comfyui-docker:latest

  ## Security


  **Read the following security precautions to ensure that you wish to continue
  using this application.**

  ---

  ## Bug Reports and Feature Requests


  If you find a bug in this app or have an idea for a new feature, please file
  an issue at

  https://github.com/Elenedeath/ComfyUI-Docker
x-portals:
  - host: 0.0.0.0
    name: Web UI
    path: /
    port: 8188
    scheme: http
    ```
    
    
    
services:
  comfyui:
    container_name: comfyui
    image: hikage/comfyui-docker:latest
    environment:
      - NVIDIA_VISIBLE_DEVICES=all
      - NVIDIA_DRIVER_CAPABILITIES=all,compute,utility
    ports:
      - '8188:8188'
    restart: unless-stopped
    runtime: nvidia
    stdin_open: true
    tty: true
    volumes:
      - /mnt/mnt/comfyui/models:/opt/comfyui/models
      - /mnt/mnt/comfyui/input:/opt/comfyui/input
      - /mnt/mnt/comfyui/output:/opt/comfyui/output
      - /mnt/mnt/comfyui/user:/opt/comfyui/user
x-portals:
  - host: 0.0.0.0
    name: Web UI
    path: /
    port: 8188
    scheme: http
    
      

# FAILED

[EFAULT] Failed 'up' action for 'cui' app. Please check /var/log/app_lifecycle.log for more details



services:
  comfyui:
    container_name: comfyui
    image: hikage/comfyui-docker:latest
    ports:
      - '8188:8188'
    restart: unless-stopped
    stdin_open: true
    tty: true
    volumes:
      - /mnt/mnt/comfyui/models:/opt/comfyui/models
      - /mnt/mnt/comfyui/input:/opt/comfyui/input
      - /mnt/mnt/comfyui/output:/opt/comfyui/output
      - /mnt/mnt/comfyui/user:/opt/comfyui/user
x-portals:
  - host: 0.0.0.0
    name: Web UI
    path: /
    port: 8188
    scheme: http