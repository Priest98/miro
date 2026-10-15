# Deployment: Vercel frontend + backend on your own PC

The frontend is hosted on Vercel. The backend runs on your Windows PC and is
exposed to the internet through a free Cloudflare tunnel. The hosted site works
only while your PC, the backend and the tunnel are running.

## 1. One-time setup on your PC

1. Install Node.js (LTS) from https://nodejs.org and Git from https://git-scm.com.
2. Install uv in PowerShell:
   ```powershell
   powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
   ```
3. Download the Cloudflare tunnel tool: get `cloudflared-windows-amd64.exe` from
   https://github.com/cloudflare/cloudflared/releases/latest, rename it to
   `cloudflared.exe`, and put it in your MiroFish folder.
4. Clone and configure:
   ```powershell
   git clone https://github.com/Priest98/miro.git
   cd miro
   Copy-Item .env.example .env
   notepad .env
   ```
   Fill in `LLM_API_KEY`, `LLM_BASE_URL`, `LLM_MODEL_NAME` and `ZEP_API_KEY`,
   and delete the three `LLM_BOOST_*` lines. For the free Gemini tier use:
   ```
   LLM_BASE_URL=https://generativelanguage.googleapis.com/v1beta/openai/
   LLM_MODEL_NAME=gemini-3.5-flash-lite
   ```
5. Install the backend:
   ```powershell
   npm run setup:backend
   ```

## 2. Every time you want to use the site

Open two PowerShell windows in the `miro` folder.

Window 1, the backend:
```powershell
npm run backend
```

Window 2, the tunnel:
```powershell
.\cloudflared.exe tunnel --url http://localhost:5001
```
It prints an address like `https://random-words.trycloudflare.com`.

Then open the Vercel site with that address added once:
```
https://<your-vercel-site>/?api=https://random-words.trycloudflare.com
```
The site remembers the address in your browser. The tunnel address changes
every time you restart cloudflared, so repeat the `?api=` step after a restart.

## Running fully locally instead

```powershell
npm run setup:all
npm run dev
```
Then open http://localhost:3000. No tunnel or `?api=` is needed.
