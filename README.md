# 🌐 LLHub — Language Learning Hub Website

A full-stack landing page for **LLHub**, generated from the PowerPoint presentation. Includes a beautiful animated frontend and a lightweight Node.js/Express backend with a file-based data store — no external database required.

---

## 📁 Project Structure

```
llhub/
├── public/
│   ├── index.html        ← Main landing page
│   ├── css/style.css     ← All styles
│   └── js/main.js        ← Interactions & API calls
├── data/
│   └── signups.json      ← Auto-created; stores form submissions
├── server.js             ← Express backend
├── package.json
├── .env.example          ← Copy to .env and edit
├── Procfile              ← For Railway/Heroku
└── README.md
```

---

## 🚀 Running Locally

### Prerequisites
- Node.js 18+ ([download](https://nodejs.org))

### Steps

```bash
# 1. Install dependencies
npm install

# 2. Copy and configure environment variables
cp .env.example .env
# Edit .env and set a strong ADMIN_KEY

# 3. Start the server
npm start

# 4. Open in browser
open http://localhost:3000
```

For development with auto-reload:
```bash
npm run dev   # uses nodemon
```

---

## 🔌 API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/api/signup` | Submit signup form |
| `GET` | `/api/stats` | Public stats |
| `GET` | `/api/signups` | All signups (admin, requires `x-admin-key` header) |
| `GET` | `/api/health` | Health check |

### Example: View Signups (Admin)
```bash
curl http://localhost:3000/api/signups \
  -H "x-admin-key: your-admin-key"
```

---

## ☁️ Hosting Options

### Option 1 — Railway (Recommended, Free Tier Available)

1. Create an account at [railway.app](https://railway.app)
2. Click **New Project → Deploy from GitHub repo**
3. Push this folder to a GitHub repo first:
   ```bash
   git init && git add . && git commit -m "Initial commit"
   # Create a repo on GitHub, then:
   git remote add origin https://github.com/YOUR_USER/llhub.git
   git push -u origin main
   ```
4. In Railway, connect your repo
5. Set environment variables in Railway dashboard:
   - `PORT` = `3000`
   - `ADMIN_KEY` = your secret key
6. Railway auto-detects Node.js and deploys. Done! ✅

---

### Option 2 — Render (Free Tier)

1. Go to [render.com](https://render.com) → New → Web Service
2. Connect your GitHub repo
3. Set:
   - **Build command**: `npm install`
   - **Start command**: `node server.js`
   - **Environment**: Node
4. Add environment variables (`ADMIN_KEY`, etc.)
5. Deploy — Render gives you a `.onrender.com` URL for free ✅

---

### Option 3 — Heroku

```bash
# Install Heroku CLI first: https://devcenter.heroku.com/articles/heroku-cli
heroku login
heroku create llhub-app
heroku config:set ADMIN_KEY=your-secret-key
git push heroku main
heroku open
```

---

### Option 4 — VPS (DigitalOcean / Linode / Hetzner)

```bash
# On your server (Ubuntu):
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs

# Upload project (scp or git clone), then:
cd llhub
npm install
cp .env.example .env && nano .env   # set your ADMIN_KEY

# Run with PM2 (keeps it alive after disconnect)
sudo npm install -g pm2
pm2 start server.js --name llhub
pm2 save && pm2 startup

# Optional: Nginx reverse proxy on port 80/443
# sudo apt install nginx
# Configure /etc/nginx/sites-available/llhub to proxy to localhost:3000
```

---

### Option 5 — Static-only (No Backend)

If you only need the frontend (no signup form backend), you can host `public/` directly on:
- **Netlify**: Drag-and-drop the `public/` folder at [netlify.com/drop](https://netlify.com/drop)
- **Vercel**: `npx vercel public/`
- **GitHub Pages**: Push `public/` to a `gh-pages` branch

> ⚠️ The signup form will fail without the backend. You can remove the fetch call in `js/main.js` or replace it with a third-party form service like Formspree.

---

## 🔒 Security Notes

- Change `ADMIN_KEY` to a strong random string before deploying
- The `data/signups.json` file should NOT be committed to git (it's in `.gitignore`)
- For production with many users, consider replacing the file store with PostgreSQL or MongoDB

---

## 📄 License
MIT — free to use and modify.
