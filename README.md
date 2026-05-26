# python-dotenv — Claude Code Skill

A Claude Code skill that ensures sensitive credentials and configuration values in Python projects are always loaded from a `.env` file using `python-dotenv`, never hardcoded.

## What it does

When you invoke this skill, Claude will:

- Ask whether you need a single environment or multiple (dev / staging / production)
- Scan your Python files for hardcoded secrets, credentials, IP addresses, and network config
- Create a `.env` file, `.env.example` template, `.gitignore`, `.claudeignore`, and `requirements.txt`
- Rewrite your Python entry point to use `load_dotenv()` with startup validation that fails fast if required variables are missing

## When it triggers

This skill activates automatically when you:

- Write or review Python code involving API keys, passwords, tokens, database URLs, or secret keys
- Have hardcoded IP addresses, hostnames, ports, SSH credentials, proxy settings, or SMTP servers in Python code
- Use phrases like "connect to database", "use my API key", "authenticate", "credentials", "environment variable", "IP address", "hostname", or "server address"
- Ask how to store secrets, manage config, or set up environment variables in Python

## Example

**Before:**
```python
username = 'admin'
password = 'supersecret'

device = {
    'host': '192.168.1.1',
    'username': username,
    'password': password,
}
```

**After:**
```python
from dotenv import load_dotenv
import os

load_dotenv()

REQUIRED_VARS = ["DEVICE_USERNAME", "DEVICE_PASSWORD", "DEVICE_HOST"]
missing = [v for v in REQUIRED_VARS if not os.getenv(v)]
if missing:
    raise EnvironmentError(
        f"Missing required environment variables: {', '.join(missing)}\n"
        "Copy .env.example to .env and fill in the values."
    )

device = {
    'host': os.getenv("DEVICE_HOST"),
    'username': os.getenv("DEVICE_USERNAME"),
    'password': os.getenv("DEVICE_PASSWORD"),
}
```

## Installation

1. Download `python-dotenv.skill` from the [Releases](../../releases) page
2. In Claude Code, run:
   ```
   /install-skill path/to/python-dotenv.skill
   ```
   Or drag and drop the `.skill` file into a Claude Code session.

## Usage

Once installed, Claude will trigger this skill automatically when it detects hardcoded credentials or relevant phrases. You can also invoke it explicitly:

```
/python-dotenv
```

## Requirements

- [Claude Code](https://claude.ai/code)
- Python 3.7+
- `pip install python-dotenv` (Claude will do this for you)

## License

Apache 2.0

## Contributors
Erika Dietrick
