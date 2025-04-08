Python Odoo Debug Images
This repository includes a Dockerfile to create your own image for debugging Odoo.

- It has been tested on VSCode for both macOS and Linux, but it should also function on Windows.

- Each folder contains an image specific to the corresponding Odoo version.

- You can create images for different Odoo versions by using the existing ones as a reference.

- Feel free to share your updates and improvements by submitting a pull request!

Copyright © Hawary  m@havari.me  2025-present. By using this repository, you acknowledge that you understand the implications and assume all associated risks.

Below is the updated step-by-step guide for debugging Odoo using Docker, with **Step 6: Clone Tutorial Files** modified to use the new repository URL: `https://github.com/Pro-Tech777/Docker-Odoo-Debug-Developer-Environment.git`. All other steps remain unchanged, and the checklist for Step 6 has been updated to reflect the new repository.

---

### Step-by-Step Guide: Debug Odoo Using Docker

#### Step 1: Install Prerequisites (Docker, Docker Compose, Git, and Docker CLI Tools)
- **Instructions**:  
  - **Install Git**:  
    - Follow the installation guide for your operating system (e.g., MacOS, Ubuntu, Windows) from the official Git website.  
  - **Install Docker**:  
    - Download and install Docker Desktop (MacOS/Windows) or Docker Engine (Linux) from the official Docker installation page: [Docker Install](https://docs.docker.com/get-docker/).  
    - Verify installation:  
      ```bash
      docker --version
      ```
  - **Install Docker Compose**:  
    - Docker Compose is included with Docker Desktop on MacOS/Windows. For Linux, install it separately:  
      ```bash
      sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
      sudo chmod +x /usr/local/bin/docker-compose
      ```
    - Verify installation:  
      ```bash
      docker-compose --version
      ```
  - **Install Docker CLI Tools**:  
    - The core Docker CLI (`docker`) is installed with Docker. Additional tools like `docker-buildx` and `docker-scan` may need manual setup:  
      - **Buildx**: Included in Docker Desktop; for Linux, enable it with:  
        ```bash
        docker buildx version
        ```
        If not installed, follow [Buildx setup](https://docs.docker.com/buildx/working-with-buildx/).  
      - **Docker Scan**: Install via:  
        ```bash
        docker scan --version
        ```
        If unavailable, follow [Docker Scan setup](https://docs.docker.com/engine/scan/).  
  - **Linux Configuration**: Add your user to the "docker" group to avoid using `sudo`:  
    ```bash
    sudo usermod -a -G docker <your_username>
    ```
    Log out and back in to apply changes.  
- **Checklist**:  
  - [ ] Git is installed (`git --version` works).  
  - [ ] Docker is installed (`docker --version` works).  
  - [ ] Docker Compose is installed (`docker-compose --version` works).  
  - [ ] Docker Buildx is available (`docker buildx version` works).  
  - [ ] Docker Scan is available (`docker scan --version` works).  
  - [ ] (Linux only) User added to "docker" group (`docker info` runs without `sudo`).

---

#### Step 2: Set Up Directory Structure
- **Instructions**:  
  - Create the root directory `$HOME/odoo` for all Odoo-related files.  
  - Create version-specific subdirectories (e.g., `$HOME/odoo/14.0`).  
  - Inside each version directory, create:  
    - `$HOME/odoo/<odoo_version>/src` for Odoo source code.  
    - `$HOME/odoo/varlib` for the Odoo filestore (ensure the container has write access).  
    - `$HOME/odoo/shared_postgres_data` for Postgres data.  
    - `$HOME/odoo/docker-odoo-debug` for tutorial configuration files.  
  - Example for Odoo 14.0:  
    ```bash
    mkdir -p $HOME/odoo/14.0/src $HOME/odoo/varlib $HOME/odoo/shared_postgres_data $HOME/odoo/docker-odoo-debug
    ```
- **Checklist**:  
  - [ ] Root directory `$HOME/odoo` created.  
  - [ ] Version-specific directory (e.g., `$HOME/odoo/14.0`) created.  
  - [ ] Source code directory `$HOME/odoo/<odoo_version>/src` created.  
  - [ ] Filestore directory `$HOME/odoo/varlib` created with write access for the container.  
  - [ ] Postgres data directory `$HOME/odoo/shared_postgres_data` created.  
  - [ ] Tutorial directory `$HOME/odoo/docker-odoo-debug` created.

---

#### Step 3: Set Up Postgres Database Server
- **Instructions**:  
  - Run a Postgres container with the following command:  
    ```bash
    mkdir -p $HOME/odoo/shared_postgres_data && \
    docker run -d -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo \
       -e POSTGRES_DB=postgres \
       -e PGDATA=/var/lib/postgresql/data \
       --shm-size=1g \
       --restart always \
       -v $HOME/odoo/shared_postgres_data:/var/lib/postgresql/data \
       -p 5432:5432 \
       --name db-shared postgres:14
    ```
  - Verify the Postgres container is running:  
    ```bash
    docker ps
    ```
- **Checklist**:  
  - [ ] Postgres container is running.  
  - [ ] Postgres data directory `$HOME/odoo/shared_postgres_data` is mounted.  
  - [ ] Port 5432 is exposed and accessible.

---

#### Step 4: Clone Odoo Source Code
- **Instructions**:  
  - Navigate to the source directory and clone the Odoo repository (e.g., for version 14.0):  
    ```bash
    cd $HOME/odoo/14.0/src && git clone -b 14.0 --single-branch --depth=1 https://github.com/odoo/odoo.git
    ```
  - (Optional) For Odoo Enterprise, clone the enterprise repo:  
    ```bash
    cd $HOME/odoo/14.0/src && git clone <enterprise_repo_url>
    ```
  - To update the source code later:  
    ```bash
    cd $HOME/odoo/14.0/src/odoo && git pull
    ```
- **Checklist**:  
  - [ ] Odoo Community source code cloned to `$HOME/odoo/<odoo_version>/src/odoo`.  
  - [ ] (Optional) Odoo Enterprise source code cloned to `$HOME/odoo/<odoo_version>/src/enterprise`.

---

#### Step 5: Install VSCode Extensions
- **Instructions**:  
  - Open Visual Studio Code (VSCode).  
  - Install the following extensions:  
    - Python  
    - Pylint  
    - Docker  
- **Checklist**:  
  - [ ] Python extension installed in VSCode.  
  - [ ] Pylint extension installed in VSCode.  
  - [ ] Docker extension installed in VSCode.

---

#### Step 6: Clone Tutorial Files
- **Instructions**:  
  - Clone the tutorial repository into `$HOME/odoo/docker-odoo-debug` using the updated repository URL:  
    ```bash
    cd $HOME/odoo && git clone https://github.com/Pro-Tech777/Docker-Odoo-Debug-Developer-Environment.git docker-odoo-debug
    ```
  - Alternatively, download the files manually from the repository (`https://github.com/Pro-Tech777/Docker-Odoo-Debug-Developer-Environment`) and place them in `$HOME/odoo/docker-odoo-debug`.  
- **Checklist**:  
  - [ ] Tutorial repository cloned or downloaded to `$HOME/odoo/docker-odoo-debug` from `https://github.com/Pro-Tech777/Docker-Odoo-Debug-Developer-Environment.git`.

---

#### Step 7: Configure Odoo Config File
- **Instructions**:  
  - Locate the `odoo.conf` file in `$HOME/odoo/docker-odoo-debug`.  
  - Edit `odoo.conf` to include:  
    - Path to Odoo source code (e.g., `$HOME/odoo/14.0/src/odoo`).  
    - Filestore path (e.g., `$HOME/odoo/varlib`).  
    - (Optional) Custom add-ons paths if needed.  
- **Checklist**:  
  - [ ] `odoo.conf` filestore path points to `$HOME/odoo/varlib`.  
  - [ ] `odoo.conf` Odoo source code path points to `$HOME/odoo/<odoo_version>/src/odoo`.  
  - [ ] (Optional) Custom add-ons paths added to `odoo.conf`.

---

#### Step 8: Configure VSCode
- **Instructions**:  
  - Copy the `.vscode` directory from `$HOME/odoo/docker-odoo-debug` to your project root (e.g., `$HOME/odoo/14.0`).  
    ```bash
    cp -r $HOME/odoo/docker-odoo-debug/.vscode $HOME/odoo/14.0/
    ```
  - Alternatively, if `.vscode` already exists:  
    - From `settings.json`, copy all "odoo" variables (under "Cetmix Docker Debug Variables").  
    - From `tasks.json`, copy all tasks.  
    - From `launch.json`, copy the launch configurations.  
  - In `settings.json`, set the `odooVersion` variable to your project version (e.g., "14.0").  
  - Update paths in `settings.json`, `tasks.json`, and `launch.json` to match your directory structure if different.  
- **Checklist**:  
  - [ ] `.vscode` directory copied to project root or configurations merged.  
  - [ ] `settings.json` "odooVersion" set to project version (e.g., "14.0").  
  - [ ] Paths in `settings.json`, `tasks.json`, and `launch.json` updated if necessary.

---

#### Step 9: (Optional) Build Custom Docker Image for Optimization
- **Instructions**:  
  - Build a custom Docker image for your Odoo version (e.g., 14.0) using `docker build`:  
    ```bash
    docker build -t my-odoo:14.0 $HOME/odoo/docker-odoo-debug/14.0
    ```
  - Edit `tasks.json` and comment out the `"dependsOn": ["build-image"]` line to skip the build step during debugging.  
- **Checklist**:  
  - [ ] Docker image built (e.g., `my-odoo:14.0`).  
  - [ ] `"dependsOn": ["build-image"]` commented out in `tasks.json`.

---

#### Step 10: Pre-Flight Checklist
- **Instructions**:  
  - Verify all components are set up correctly before debugging.  
- **Checklist**:  
  - [ ] Git installed (`git --version`).  
  - [ ] Docker installed (`docker --version`).  
  - [ ] Docker Compose installed (`docker-compose --version`).  
  - [ ] Docker Buildx available (`docker buildx version`).  
  - [ ] Docker Scan available (`docker scan --version`).  
  - [ ] Odoo source code cloned.  
  - [ ] Filestore directory created with write access for the container.  
  - [ ] VSCode plugins (Python, Pylint, Docker) installed.  
  - [ ] Postgres DB server running.  
  - [ ] Tutorial files cloned/downloaded to `$HOME/odoo/docker-odoo-debug` from `https://github.com/Pro-Tech777/Docker-Odoo-Debug-Developer-Environment.git`.  
  - [ ] Configuration files copied or merged into project `.vscode` directory.  
  - [ ] `odoo.conf` paths (filestore, source code, add-ons) correctly set.  
  - [ ] `settings.json` "odooVersion" set.  
  - [ ] Paths in `tasks.json` point to correct locations (Odoo source, Dockerfile, `odoo.conf`).  
  - [ ] (Optional) Docker image built and `"dependsOn": ["build-image"]` disabled.

---

#### Step 11: Start Debugging
- **Instructions**:  
  - Open VSCode in your project directory (e.g., `$HOME/odoo/14.0`).  
  - Press the "Debug" button or run a launch configuration from `launch.json`.  
  - Ensure Odoo starts in the Docker container and connects to the Postgres database.  
- **Checklist**:  
  - [ ] VSCode opened in project directory.  
  - [ ] Debugging started successfully.

---
