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
'''
docker version
'''
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

'''
git clone https://github.com/IRO05/C-Dev-container.git
cd c-dev-container
'''

---

## Step 2: Build the Docker Image

From the root of the repo (where `Dockerfile` is located):

'''
docker build -t simple-c .
'''

This builds a Docker image called `simple-c` containing Ubuntu and all necessary C development tools (`gcc`, `make`, `gdb`, etc.).

---

## Step 3: Run the Docker Container

Run a container from the image and mount the `src` folder:

'''
docker run -it --name simple-c-dev -v "<full-path-to-repo>/src:/workspace" simple-c
'''

**Notes:**
- Replace `<full-path-to-repo>` with the absolute path on your machine.
- On Windows, enclose the path in quotes if it contains spaces, e.g.:  
  '''
  -v "D:\Containers\simple-c\src:/workspace"
  '''
- `simple-c-dev` is the name of the container — you can reuse it later.

Inside the container, your working directory `/workspace` will contain all files from the `src` folder.

---

### Optional: Stop and Restart Container

You can stop the container inside the terminal:

'''
exit
'''

Or detach without stopping:

**Ctrl + P, then Ctrl + Q**

To restart later from the command line:

'''
docker start -ai simple-c-dev
'''

Alternatively, you can **use Docker Desktop**:
- Open Docker Desktop → Containers → simple-c-dev → Start or Restart
- Click **Terminal** in the GUI to open a shell inside the container

---

## Step 4: Connect VSCode to the Container

1. Open VSCode.
2. Open the folder containing the repo (the root, `simple-c`).
3. Open Command Palette (`Ctrl+Shift+P`) → **Dev - Containers: Attach to Running Container...**  
4. Select `simple-c-dev`.

VSCode will now:
- Open inside the container.
- Use `/workspace` as the working directory.
- Allow editing, building, and debugging C programs using the container’s Linux environment.

---

## Step 5: Build and Run Your Code

Inside VSCode's terminal (or the container terminal):

'''
cd /workspace          # If not already here
make                   # Builds your project
./<executable_name>    # Run your program
'''

Example for a dummy file in `src`:

'''
gcc dummy.c -o dummy
./dummy
'''

---

## Tips and Notes

- **Mounting:** Only the `src` folder needs to be mounted; the Dockerfile and container image exist separately.
- **Persistence:** All your code lives on the host (`src`) — nothing is lost when the container is removed.
- **Multiple devices:** You can clone this repo on any machine with Docker Desktop and VSCode. Build the image and mount the `src` folder — everything works identically.
- **Rebuilding image:** If you update the Dockerfile (e.g., install new tools), rebuild with:
'''
docker build -t simple-c .
'''
- **Debugging with gdb:** Install the **C/C++ extension** in VSCode. You can configure `.vscode/launch.json` to debug inside `/workspace`.

---
