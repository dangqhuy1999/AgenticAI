# AgenticAI

# Environment with uv

```
mkpy() {
  proj="$1"
  mkdir -p "$proj" && cd "$proj" || exit
  uv venv .venv
  source .venv/bin/activate
  echo "import requests\nprint('Hello from $proj')" > main.py
  echo "requests" > requirements.txt
  uv pip install -r requirements.txt
  uv pip compile
  echo "✅ Project $proj created with uv, .venv, main.py, requirements.txt and uv.lock"
}
uv_update_lock() {
  if [ ! -f .venv/bin/activate ]; then
    echo "❌ .venv not found. Run inside your Python project with uv venv"
    return 1
  fi

  source .venv/bin/activate
  echo "🔄 Updating requirements.txt and uv.lock ..."
  uv pip freeze > requirements.txt
  uv pip compile
  echo "✅ Updated uv.lock and requirements.txt"
}
```
