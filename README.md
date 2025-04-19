# AgenticAI

# Environment with uv

```bash

# Global install (xài mọi project)

curl -Ls https://astral.sh/uv/install.sh | sh


# make new py project
mkpy() {
  proj="$1"
  mkdir -p "$proj" && cd "$proj" || exit
  uv venv .venv
  source .venv/bin/activate
  uv pip install requests
  echo "import requests" > main.py
  echo "requests" > requirements.txt
  uv pip install -r requirements.txt
  uv pip compile requirements.txt --output-file uv.lock

  echo "✅ Project $proj created with uv, .venv, main.py, requirements.txt and uv.lock"
}


# update new package
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

# sync uv from other project contain uv.lock file
uv_sync_from_lock() {
  if [ ! -f "uv.lock" ]; then
    echo "❌ uv.lock không tồn tại trong thư mục hiện tại."
    return 1
  fi

  if [ ! -d ".venv" ]; then
    echo "📦 Tạo virtualenv mới (.venv)"
    uv venv .venv
  fi

  source .venv/bin/activate
  echo "🔄 Cài đúng thư viện từ uv.lock ..."
  uv pip install --no-deps --frozen
  echo "✅ Đã sync môi trường Python từ uv.lock"
}


git clone https://github.com/huydxdev/uv-python-template myproj
cd myproj
source .devsetup.sh
uv_sync_from_lock
```
