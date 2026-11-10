# Using the Commons with the local API

Full guide: [../RUN.md](../RUN.md)

Nothing here runs on its own. The API reads this checkout through a bundle it builds.

## Tests

```bash
.venv/bin/python -m pytest -q
```

## Load your changes into the local API

```bash
cd ../opendiving-api
ENVIRONMENT=local SECRET_KEY=x .venv/bin/python scripts/build_commons_bundle.py ../opendiving-commons
pkill -f "uvicorn app.main:app"
cd src && nohup ../.venv/bin/uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload > ../.logs/api.log 2>&1 &
```

The API copies the bundle into its database on startup. **With `COMMONS_PRIMARY=true` (the local setting) only new
sites get in, and edits to existing sites are ignored.** Remove that line from `../opendiving-api/src/.env` while you edit,
then check in the app that the change landed. Details: [../STACKS.md](../STACKS.md).
