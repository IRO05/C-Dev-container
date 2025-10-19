# Simple-C Docker Development Environment

This repository contains a **Dockerized Linux environment for C development**, including a `src` folder with your code and a ready-to-use Dockerfile. This setup allows you to work on Linux C projects from Windows or macOS without running a full VM. VSCode is used as the editor with **Dev - Containers** for seamless integration.

---

## Prerequisites

### 1. Docker Desktop
- **Windows:**  
  - Ensure **WSL 2 backend** is enabled (required for Linux containers).  
  - In Docker Desktop → **Settings → Resources → File Sharing**, make sure the drive containing this repo (e.g., `D:`) is **shared**. Sharing `D:\Containers` is sufficient.  
- **macOS:** Docker Desktop with Linux containers support.

Verify Docker is working:
```
docker version
```
You should see both `Client` and `Server` info.

---

### 2. VSCode
- Install the following extensions:
  - **Dev - Containers** (for attaching to Docker containers)
  - **C/C++** (for IntelliSense, debugging)

---

## Repository structure

C-dev-container/
  - Dockerfile
  - src/
    - dummy.c
    - Makefile

---

## Step 1: Clone the Repository

```
git clone https://github.com/IRO05/C-Dev-container.git
cd c-dev-container
```

---

## Step 2: Build the Docker Image

From the root of the repo (where `Dockerfile` is located):

```
docker build -t simple-c .
```

This builds a Docker image called `simple-c` containing Ubuntu and all necessary C development tools (`gcc`, `make`, `gdb`, etc.).

---

## Step 3: Open the Dev Container in VSCode

With the `devcontainer.json` set up, VSCode will handle building, mounting, and attaching automatically.

1. Open VSCode on your host machine.
2. Open the folder containing the repo (the root, `C-dev-container`).
3. Open Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P` on Mac) → **Dev - Containers: Reopen in Container**.
4. VSCode will:
   - Build the container if it doesn’t exist yet.
   - Mount your host `src` folder into `/workspace`.
   - Install the C/C++ extension inside the container if it isn’t already installed.
   - Open `/workspace` as the working directory.
   - Initialize IntelliSense and other C/C++ features automatically.

---

### Optional: Stop and Restart Container

- Close VSCode or detach from the container: **Ctrl + Shift + P → Dev - Containers: Close Remote Connection**.
- Reopen the container anytime:
  1. Open VSCode in the repo folder.
  2. Command Palette → **Dev - Containers: Reopen in Container**.
- Docker Desktop can also be used to **start or stop the container**, but VSCode will attach to it automatically when you reopen.

---

## Step 4: Build and Run Your Code

Inside the VSCode terminal (which is already in `/workspace`):

```
cd /workspace       # Only needed if not already there
make                # Build your project
./<executable_name> # Run your program
```

Notes:
- All changes you make are saved on your host in `src` — nothing is lost if the container is stopped.
- IntelliSense, autocomplete, and error squiggles should work automatically inside the container.

## Tips and Notes

- **Mounting:** Only the `src` folder needs to be mounted; the Dockerfile and container image exist separately.
- **Persistence:** All your code lives on the host (`src`) — nothing is lost when the container is removed.
- **Multiple devices:** You can clone this repo on any machine with Docker Desktop and VSCode. Build the image and mount the `src` folder — everything works identically.
- **Rebuilding image:** If you update the Dockerfile (e.g., install new tools), rebuild with:
```
docker build -t simple-c .
```
- **Debugging with gdb:** Install the **C/C++ extension** in VSCode. You can configure `.vscode/launch.json` to debug inside `/workspace`.

---
