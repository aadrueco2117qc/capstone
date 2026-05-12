# EyeGuard — How to Run Locally

This guide gets the full app running on your machine.
You will end up with:
- Frontend at **http://localhost:3000**
- Flask backend at **http://localhost:5000**
- Supabase handling auth and the database (already set up)

---

## What You Need Installed

| Tool | Check if installed | Download |
|---|---|---|
| Node.js 18+ | `node -v` | https://nodejs.org |
| pnpm | `pnpm -v` | `npm install -g pnpm` |
| Python 3.8+ | `python --version` | https://python.org |
| Git | `git --version` | https://git-scm.com |

---

## Step 1 — Get Your Supabase Keys

You need two values from your Supabase project.

1. Go to **https://supabase.com** and open your project
2. Click **Project Settings** (gear icon, bottom left)
3. Click **API** in the left sidebar
4. Copy these two values:

```
Project URL        →  looks like: https://xxxxxxxxxxxx.supabase.co
anon / public key  →  long string starting with eyJ...
```

Keep these open — you'll paste them in Step 3.

---

## Step 2 — Open the Project Folder

Open a terminal (Command Prompt or PowerShell) and go to the project:

```cmd
cd C:\Users\Mars\Documents\v0-eyestrain-main
```

---

## Step 3 — Create the Environment File

The frontend needs to know your Supabase credentials.
Create a file called `.env.local` in the root of the project:

```cmd
copy NUL .env.local
```

Then open it in any text editor (Notepad is fine) and paste this,
replacing the values with your actual keys from Step 1:

```
NEXT_PUBLIC_SUPABASE_URL=https://xxxxxxxxxxxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

Save and close the file.

> ⚠️ Never share this file or commit it to GitHub. It's already in `.gitignore`.

---

## Step 4 — Install Frontend Dependencies

In the same terminal (project root):

```cmd
pnpm install
```

This downloads all the Node.js packages. Takes about 1–2 minutes the first time.

---

## Step 5 — Start the Frontend

```cmd
pnpm dev
```

You should see:

```
▲ Next.js 16.2.4
- Local:   http://localhost:3000
- Ready in 2.1s
```

Open **http://localhost:3000** in your browser. The app is running.

> The frontend connects directly to Supabase for auth and data —
> you don't need the Flask backend just to see the UI and log in.

---

## Step 6 — Set Up the Flask Backend (for ML predictions)

The machine learning runs in the Flask backend. Open a **second terminal window**
(keep the first one running pnpm dev).

### 6a — Go to the backend folder

```cmd
cd C:\Users\Mars\Documents\v0-eyestrain-main\backend
```

### 6b — Create a Python virtual environment

```cmd
python -m venv venv
```

### 6c — Activate the virtual environment

```cmd
venv\Scripts\activate
```

Your terminal prompt should now show `(venv)` at the start.

### 6d — Install Python dependencies

```cmd
pip install -r requirements.txt
```

This installs Flask, scikit-learn, SQLAlchemy, and everything else.
Takes 2–3 minutes the first time.

### 6e — Create the backend environment file

In the `backend` folder, create a `.env` file:

```cmd
copy NUL .env
```

Open it and add:

```
FLASK_ENV=development
SECRET_KEY=eyeguard-dev-secret-key-change-in-production
DATABASE_URL=sqlite:///eyeguard.db
```

### 6f — Start Flask

```cmd
python app.py
```

**The first time you run this**, the ML training pipeline starts automatically.
Watch the terminal — you will see the full training process printed out:

```
============================================================
  EYEGUARD ML TRAINING PIPELINE — STARTING
============================================================

[STEP 1/4] Generating synthetic training data...
  Simulating 300 users × 30 days = 9,000 daily log samples
  ...
  ✓ Generated 9000 training samples

[STEP 2/4] Feature engineering...
[STEP 3/4] Training 3 models with 5-fold cross-validation...

  [Model 1/3] Risk Level Classifier (Random Forest)
    Mean accuracy : 0.851 ± 0.003
    Top 5 most important features:
      screen_time_minutes   0.3412  █████████████
      ...

  [Model 2/3] Risk Percentage Regressor (Gradient Boosting)
    Mean R² : 0.821 ± 0.011

  [Model 3/3] Fatigue Predictor (Linear Regression)
    Mean R² : 0.754 ± 0.018

[STEP 4/4] Training complete
============================================================
  EYEGUARD ML TRAINING PIPELINE — COMPLETE
============================================================

 * Running on http://0.0.0.0:5000
```

**On every run after that**, the saved models load instantly:

```
============================================================
  ML MODELS LOADED FROM DISK
  Location : backend/ml/models
  classifier           : 45.2 KB
  regressor            : 18.7 KB
  fatigue              : 2.1 KB
============================================================
 * Running on http://0.0.0.0:5000
```

---

## Step 7 — Use the App

With both terminals running:

| What | URL |
|---|---|
| The app | http://localhost:3000 |
| Flask API | http://localhost:5000 |

### First time using it:

1. Go to **http://localhost:3000**
2. Click **Sign Up** — create an account with your email and password
3. You'll be taken to the dashboard
4. Click **Daily Log** in the sidebar
5. Fill in your screen time, breaks, symptoms, sleep, etc.
6. Submit the form
7. Go back to **Dashboard** and click **Refresh**
8. Watch the Flask terminal — you'll see the ML prediction process printed live
9. The dashboard updates with your risk level, risk %, and fatigue score

---

## Both Terminals at a Glance

```
Terminal 1 (Frontend)          Terminal 2 (Backend)
─────────────────────          ────────────────────
cd v0-eyestrain-main           cd v0-eyestrain-main\backend
pnpm dev                       venv\Scripts\activate
                               python app.py

→ http://localhost:3000        → http://localhost:5000
```

---

## Troubleshooting

### "pnpm: command not found"
```cmd
npm install -g pnpm
```

### "Module not found" errors on pnpm dev
```cmd
pnpm install
```

### Frontend loads but shows blank / auth errors
- Check your `.env.local` file has the correct Supabase URL and key
- Make sure there are no spaces around the `=` sign
- Restart with `pnpm dev` after editing `.env.local`

### Flask won't start — "No module named flask"
- Make sure you activated the virtual environment first: `venv\Scripts\activate`
- Then: `pip install -r requirements.txt`

### ML training takes too long or crashes
- Needs about 500MB of free RAM
- If it crashes, delete `backend/ml/models/` folder and restart Flask

### "sqlite3.OperationalError" on Flask start
- Delete `backend/instance/eyeguard.db` and restart Flask
- It will recreate the database automatically

### Dashboard shows predictions but Flask terminal shows nothing
- The frontend (Supabase route) handles predictions separately from Flask
- To see the Flask ML output, make sure the frontend's `NEXT_PUBLIC_API_URL` points to `http://localhost:5000/api`
- Add this line to your `.env.local`:
  ```
  NEXT_PUBLIC_API_URL=http://localhost:5000/api
  ```

---

## Stopping the App

- **Frontend**: Press `Ctrl + C` in Terminal 1
- **Backend**: Press `Ctrl + C` in Terminal 2
- To deactivate the Python virtual environment: type `deactivate`

---

## Quick Reference — All Commands

```cmd
# Terminal 1 — Frontend
cd C:\Users\Mars\Documents\v0-eyestrain-main
pnpm install          ← only needed once
pnpm dev              ← starts frontend on :3000

# Terminal 2 — Backend
cd C:\Users\Mars\Documents\v0-eyestrain-main\backend
python -m venv venv   ← only needed once
venv\Scripts\activate ← every time you open a new terminal
pip install -r requirements.txt  ← only needed once
python app.py         ← starts Flask on :5000
```
