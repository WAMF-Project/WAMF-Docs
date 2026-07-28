# Project Structure

The WAMF repository is organised into a number of directories, each with a specific purpose.

```text
WAMF/
├── app/
├── config/
├── data/
├── docs/
├── media/
├── tests/
├── retention.py
├── run.py
├── requirements.txt
└── ...
```

| Path | Description |
|------|-------------|
| `app/` | Core WAMF application code. |
| `config/` | Configuration files. |
| `data/` | SQLite database and application data. |
| `docs/` | Project documentation. |
| `media/` | Snapshots and video clips. |
| `tests/` | Automated test suite. |
| `retention.py` | Media retention and cleanup task. |
| `run.py` | WAMF application entry point. |

## Development

WAMF follows a modular design where functionality is separated into logical components.

Code quality is maintained using:

- Ruff
- Black
- Pytest

All changes should leave the project in a working state with formatting and tests passing.