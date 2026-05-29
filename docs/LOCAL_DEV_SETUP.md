# Local Development Setup — VS Code + Python

<span style="color:#0056A3">**Click-by-click guide to running Bendora School System locally for development.**</span>

> ✅ Optimized for: Windows 10/11, macOS 12+, Ubuntu 22.04+ | VS Code 1.85+ | Python 3.11+ | Docker Desktop

## 📦 Step 1: Install Prerequisites

| Tool | Download Link | Verify Installation |
|------|--------------|---------------------|
| **VS Code** | [code.visualstudio.com](https://code.visualstudio.com) | `code --version` |
| **Python 3.11+** | [python.org/downloads](https://python.org/downloads) | `python3 --version` |
| **Git** | [git-scm.com](https://git-scm.com) | `git --version` |
| **Docker Desktop** | [docker.com/products/docker](https://docker.com/products/docker) | `docker --version` & `docker compose version` |

> 💡 *Liberia Dev Tip:* If Docker is too heavy for your machine, you can run PostgreSQL & Redis natively. Skip Step 4 and follow the "Native Fallback" note.

## 🗂️ Step 2: Open Project in VS Code

1. Launch VS Code
2. Press `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (Mac)
3. Type `Git: Clone` and press Enter
4. Paste repository URL: `https://github.com/raccazahn/Bendora-School-System.git`
5. Choose a local folder (e.g., `C:\Projects\` or `~/Projects/`)
6. When prompted, click **Open** to open the cloned repo

## ⚙️ Step 3: Install Recommended Extensions

Press `Ctrl+Shift+X` → Search & install:
- `ms-python.python` (Official Python support)
- `ms-python.vscode-pylance` (IntelliSense & type checking)
- `ms-azuretools.vscode-docker` (Container management)
- `charliermarsh.ruff` (Fast Python linter/formatter)
- `bradlc.vscode-tailwindcss` (Tailwind autocomplete)
- `GitHub.copilot` *(Optional AI pair programmer)*

## 🔐 Step 4: Configure Environment Variables

1. In the VS Code Explorer, right-click `.env.example`
2. Select **Copy**
3. Right-click in the same folder → **Paste**
4. Rename the new file to `.env.local`
5. Open `.env.local` and verify/update:
 
   APP_ENV=development
   DATABASE_URL=postgresql://postgres:postgres@localhost:5432/bendora_school
   REDIS_URL=redis://localhost:6379/0
   LOW_BANDWIDTH_MODE=true
   DEFAULT_TIMEZONE=Africa/Monrovia


## 🐘 Step 5: Start Database & Cache (Docker)

1. Open VS Code Terminal: ``Ctrl+` ``
2. Run:
   docker compose up -d postgres redis
   ```
3. Wait ~15 seconds, then verify:
   docker compose ps
   # Expected output: postgres and redis show as "healthy" or "running"
   ```

> 📡 *Native Fallback (No Docker):*
> - Install PostgreSQL locally → `CREATE DATABASE bendora_school;`
> - Install Redis → `redis-server`
> - Update `DATABASE_URL` and `REDIS_URL` in `.env.local` if using custom ports/passwords.

## 🧪 Step 6: Setup Python Environment & Dependencies

In the VS Code Terminal:

# Create virtual environment
python3 -m venv venv

# Activate:
# Windows:
venv\Scripts\activate
# macOS/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

✅ VS Code should auto-detect the interpreter. If not:
- Press `Ctrl+Shift+P` → `Python: Select Interpreter` → Choose `./venv/bin/python` (or `.\venv\Scripts\python.exe`)

## 🗄️ Step 7: Initialize Database & Seed Demo Data

# Apply schema migrations (creates tables, RLS policies, indexes)
alembic upgrade head

# Seed a demo tenant with sample users (admin, teacher, student)
python scripts/seed_demo.py --tenant bendora-demo


🔐 **Demo Credentials (after seeding):**
- Admin: `admin@bendora.demo` / `BendoraAdmin2026!`
- Teacher: `teacher@bendora.demo` / `BendoraTeacher2026!`
- Student: `student@bendora.demo` / `BendoraStudent2026!`

## 🚀 Step 8: Launch Development Servers

### Backend (FastAPI/Flask)

uvicorn src.main:app --reload --host 0.0.0.0 --port 8000
# OR if using Flask:
flask --app src.main run --reload --host 0.0.0.0 --port 8000

### Frontend (React + Vite/Next.js)
Open a **new terminal** (`Ctrl+Shift+`` `), navigate to `src/web/` (or root if monorepo):

npm install
npm run dev

✅ Open `http://localhost:5173` (or `3000`) in your browser  
✅ Log in with demo credentials  
✅ Verify role-based routing works (Admin vs Student views)

## 🐛 Troubleshooting Common Issues

| Symptom | Solution |
|---------|----------|
| `psycopg2.OperationalError: connection refused` | Ensure Docker postgres is running. Check `docker compose logs postgres` |
| `ModuleNotFoundError: No module named 'src'` | Activate venv + run `pip install -e .` or adjust `PYTHONPATH` |
| `CORS error in browser console` | Add `http://localhost:5173` to `ALLOWED_ORIGINS` in `.env.local` |
| `RLS policy denied query` | You're missing `tenant_id` in session/context. Run `python scripts/seed_demo.py` again |
| `Slow page loads` | Enable `LOW_BANDWIDTH_MODE=true` in `.env.local` |

## 🧹 Cleanup (When Done Coding)

# Stop Docker containers
docker compose down

# Deactivate virtual environment
deactivate   # or just close the terminal

# Clear cache (optional)
rm -rf venv/ __pycache__/ .pytest_cache/ .ruff_cache/

<span style="color:#0A2540">*Code locally. Ship confidently. Built for developers in Liberia and beyond. — Bendora Engineering*</span>
