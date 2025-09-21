# PyCLI: A Web-Based Terminal 

PyCLI is a web-based terminal emulator with a retro aesthetic, created for the CodeMate Hackathon. It provides a sandboxed, Linux-like shell environment directly in your browser. The backend is powered by Python and Flask, featuring a custom command executor that safely re-implements common shell commands without executing arbitrary code on the host machine.

The project includes both a web interface and a standalone local command-line client.

## Features

-   **Interactive Web Terminal:** A responsive "RetroShell" UI built with vanilla JavaScript and CSS.
-   **Sandboxed Environment:** All file system operations are sandboxed and start within the `/tmp` directory for safety.
-   **Custom Command Executor:** Core logic is handled by a Python class that parses and safely executes a whitelisted set of commands.
-   **System Monitoring:** Includes commands to check CPU usage, memory, and running processes using `psutil`.
-   **Standalone CLI:** A local terminal client (`cli.py`) that uses the same command execution engine.
-   **Deployment Ready:** Configured with a `Procfile` and `gunicorn` for easy deployment to platforms like Heroku.

## Architecture

The application is composed of three main parts:

1.  **Frontend (`templates/index.html`):** A single-page application that provides the terminal interface. It captures user input, sends it to the backend API via `fetch`, and renders the standard output (`stdout`) and standard error (`stderr`) in the terminal window.

2.  **Backend (`webapp.py`):** A lightweight Flask application that serves the frontend and exposes a REST API endpoint at `/api/exec`. This endpoint receives JSON payloads containing the command string and the current working directory (`cwd`).

3.  **Command Executor (`executor.py`):** This is the core engine of the terminal. The `CommandExecutor` class contains pure Python implementations of various shell commands. It parses the raw command string, identifies the command and its arguments, and executes the corresponding sandboxed logic. This design avoids dangerous operations like `os.system` or `subprocess`, ensuring that only the predefined, safe commands can be run.

## Supported Commands

The following commands are implemented and available in the shell:

```
  File System & Navigation:
  - pwd
  - ls [-l] [path]
  - cd <path>
  - mkdir <name>
  - rmdir <name>
  - rm [-r] <path>
  - cat <file>
  - touch <file>
  - mv <src> <dest>
  - cp [-r] <src> <dest>
  - echo [text] [> file] [>> file]
  - head <file>
  - tail <file>

  System & Info:
  - cpu
  - mem
  - ps
  - whoami
  - date
  - uptime

  Utility:
  - clear
  - help
```

## Getting Started

### Prerequisites

-   Python 3.x
-   `pip`

### Installation

1.  Clone the repository:
    ```sh
    git clone https://github.com/nikhilsharma2707/codemate_hackathon.git
    ```

2.  Navigate to the project directory:
    ```sh
    cd codemate_hackathon
    ```

3.  Install the required Python packages:
    ```sh
    pip install -r requirements.txt
    ```

### Running the Web Application

To run the web-based terminal, start the Flask development server:

```sh
python webapp.py
```

Open your web browser and navigate to `http://127.0.0.1:5000` to use the terminal.

### Running the Local CLI

The project also includes a standalone command-line version that uses the same executor engine.

```sh
python cli.py
```

## Deployment

This application is ready for deployment on PaaS providers like render. The `Procfile` is configured to use `gunicorn` as the production web server:

```
web: gunicorn webapp:app
