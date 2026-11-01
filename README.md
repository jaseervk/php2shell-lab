# PHP2Shell Exploitation Lab

A PHP2Shell exploitation lab containing a Docker-based target environment and a Python exploit script.

This repository is intended for running and demonstrating the PHP2Shell exploitation process in a local lab environment.

## Project Structure

```text
php2shell-lab/
│
├── exploit.py
│
└── Docker/
    ├── Dockerfile
    ├── docker-compose.yml
    ├── zz-poc.ini
    │
    └── webroot/
        └── index.php
```

## Requirements

- Docker
- Docker Compose
- Python 3

## Setup & Run

### 1. Start the Docker Environment

Navigate to the Docker directory:

```bash
cd Docker
```

Build and start the target:

```bash
docker compose up -d --build
```

The lab will be available at:

```text
http://127.0.0.1:8088
```

### 2. Run the Exploit

Open a new terminal in the repository's root directory.

**Method 1: Command Execution with Fetch Verification**

```bash
python3 exploit.py --host 127.0.0.1 --port 8088 \
    --cmd 'id>/var/www/html/pwn.txt' --fetch /pwn.txt
```

**Command explanation:**

| Parameter | Explanation |
|---|---|
| `python3` | Runs the script using Python 3. |
| `exploit.py` | The PHP2Shell exploitation script. |
| `--host 127.0.0.1` | Specifies the target IP address. Here, the target is running locally. |
| `--port 8088` | Specifies the port where the Docker target is running. |
| `--cmd` | Specifies the command to execute on the target. |
| `id` | Displays the identity and privileges of the user executing the command. |
| `>` | Redirects the command output into a file instead of displaying it directly. |
| `/var/www/html/pwn.txt` | File path where the command output is saved on the target. |
| `--fetch /pwn.txt` | Retrieves the generated file through the target's web server to verify command execution. |

**How does it work?**

1. The exploit script connects to the local PHP target at `127.0.0.1:8088`.
2. It performs the PHP2Shell exploitation process.
3. The supplied `id` command executes on the target system.
4. The command output is saved in `pwn.txt`.
5. The `--fetch` option retrieves the file contents.
6. A successful retrieval provides evidence of command execution.

**Method 2: Sleep-Based Verification**

```bash
python3 exploit.py --host 127.0.0.1 --port 8088 --sleep-oracle
```

This uses a time-based verification method.

Instead of retrieving a file, the script uses a `sleep` command to check whether the supplied command was executed. It measures the response time to identify a successful execution.

## Stop the Lab

Navigate to the Docker directory and run:

```bash
docker compose down
```

## Disclaimer

This project is intended for controlled local lab testing and security research. Use it only in environments you own or have explicit authorization to test.
