# Voice Agent — Complete Setup Guide

Your own AI voice agent that answers phone calls, uses your knowledge base, and records every conversation.

---

## What You Need Before Starting

1. **A computer** (Mac, Windows, or Linux)
2. **Docker Desktop** — download it free from [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/)
3. **4 free API keys** (we'll get these in Step 2)

---

## Step 1: Install Docker Desktop

1. Go to [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/)
2. Click the download button for your computer (Mac or Windows)
3. Open the downloaded file and follow the installer
4. Once installed, **open Docker Desktop** — you'll see a whale icon in your taskbar/menu bar
5. Wait until it says **"Docker Desktop is running"** (green icon)

> If Docker asks you to create an account, you can skip it. You don't need one.

---

## Step 2: Get Your API Keys (4 keys, ~10 minutes)

You need 4 keys. Open each link below in a new tab, sign up, and copy your key.

### Key 1: Google AI (Gemini) — FREE
1. Go to [aistudio.google.com/apikey](https://aistudio.google.com/apikey)
2. Sign in with your Google account
3. Click **"Create API Key"**
4. Copy the key (starts with `AIza...`)
5. Save it somewhere — you'll need it in Step 4

### Key 2: Deepgram (Speech-to-Text) — FREE $200 credit
1. Go to [console.deepgram.com](https://console.deepgram.com)
2. Sign up for a free account
3. Go to **Settings → API Keys**
4. Click **"Create Key"**
5. Copy the key
6. Save it somewhere — you'll need it in Step 4

### Key 3: Telnyx (Phone Number) — ~$1.15/month
1. Go to [telnyx.com](https://telnyx.com) and create an account
2. Add a payment method (credit card)
3. Go to **Numbers → Search & Buy** and buy a phone number (~$1.15/month)
4. Go to **Account → Keys & Credentials**
5. Copy your **API Key**
6. Your **SIP URI** will look like: `sip:+1XXXXXXXXXX@sip.telnyx.com` (replace X's with your phone number)
7. Save both — you'll need them in Step 4

### Key 4: Cloudflare R2 (Call Recordings) — FREE 10GB
1. Go to [dash.cloudflare.com](https://dash.cloudflare.com) and create an account
2. In the left sidebar, click **R2 Object Storage**
3. Click **"Create Bucket"**, name it `voice-agent-recordings`, click **Create**
4. Go to **R2 → Manage R2 API Tokens** → **Create API Token**
5. Give it **Object Read & Write** permissions for your bucket
6. After creating, you'll see:
   - **Access Key ID** — copy it
   - **Secret Access Key** — copy it (you can only see this once!)
7. Your **Account ID** is in the URL: `dash.cloudflare.com/ACCOUNT_ID_HERE`
8. For **Public URL**: go to your bucket → **Settings** → **Public Access** → Enable and copy the URL
9. Save all of these — you'll need them in Step 4

---

## Step 3: Download This Project

### Option A: Using Antigravity (recommended)
1. Open [Google Antigravity](https://antigravity.google/)
2. Click **"Open from GitHub"**
3. Paste the repo URL your instructor gave you
4. It will open the project in Antigravity

### Option B: Manual download
1. Click the green **"Code"** button on the GitHub page
2. Click **"Download ZIP"**
3. Unzip the downloaded file to a folder you can find (like your Desktop)

---

## Step 4: Add Your API Keys

1. In the project folder, find the file called **`.env.example`**
2. Make a copy of it and rename the copy to **`.env`** (just `.env`, no "example")
   - **Mac:** In Terminal, type: `cp .env.example .env`
   - **Windows:** In Command Prompt, type: `copy .env.example .env`
   - **Antigravity:** Tell the AI: *"Copy .env.example to .env"*
3. Open the **`.env`** file in any text editor
4. Replace each placeholder with your real keys:

```
GOOGLE_API_KEY=paste-your-google-key-here
DEEPGRAM_API_KEY=paste-your-deepgram-key-here
TELNYX_API_KEY=paste-your-telnyx-key-here
TELNYX_SIP_URI=sip:+1XXXXXXXXXX@sip.telnyx.com
R2_ACCOUNT_ID=paste-your-cloudflare-account-id
R2_ACCESS_KEY_ID=paste-your-r2-access-key
R2_SECRET_ACCESS_KEY=paste-your-r2-secret-key
R2_PUBLIC_URL=https://your-bucket-url.r2.dev
```

5. **Save the file**

> IMPORTANT: Do NOT add quotes around the keys. Just paste them directly after the `=` sign.

---

## Step 5: Start the Voice Agent

1. Open **Terminal** (Mac) or **Command Prompt** (Windows)
2. Navigate to the project folder:
   ```bash
   cd path/to/voice-agent-starter
   ```
3. Run this command:
   ```bash
   docker compose up -d
   ```
4. Wait 2-3 minutes the first time — it downloads everything automatically
5. You'll see all services starting up (database, phone bridge, AI agent, etc.)

> Using Antigravity? Just tell the AI: *"Run docker compose up -d"*

---

## Step 6: Open the Dashboard

1. Open your web browser
2. Go to: **http://localhost:3003**
3. Login with password: **changeme**
   (You can change this in your `.env` file later)

---

## What You Can Do Now

| Action | How |
|--------|-----|
| **Add a client** | Click "Add Client" → enter their name and business details |
| **Upload knowledge** | Click on a client → Upload PDF, TXT, or DOCX files |
| **Set the agent personality** | Click on a client → Edit the system prompt and greeting |
| **Change the voice** | Edit `TTS_VOICE` in `.env` (options: alba, marius, javert, jean, fantine, cosette, eponine, azelma) |
| **Test in browser** | Click "Call Agent" to talk to it through your mic |
| **Receive real phone calls** | Once Telnyx is configured, calls to your number go to the agent |
| **Listen to recordings** | Call recordings appear in the dashboard automatically |

---

## How to Stop / Restart

Open Terminal or Command Prompt in the project folder:

```bash
# Stop everything
docker compose down

# Start again
docker compose up -d

# See what's happening (live logs)
docker compose logs -f
```

---

## Troubleshooting

| Problem | What To Do |
|---------|-----------|
| "Cannot connect to Docker" | Open Docker Desktop first. Wait for the green "running" icon. |
| Page won't load at localhost:3003 | Wait 30-60 seconds. The database needs time to start up. |
| Agent doesn't answer calls | Open Terminal, run `docker compose logs agent` and check for errors. |
| "API key not set" error | Open your `.env` file. Make sure keys are pasted without quotes. |
| Port 3003 already in use | In `docker-compose.yml`, change `"3003:3000"` to `"3004:3000"`. Then open `localhost:3004`. |
| Everything was working, now it's not | Run `docker compose down` then `docker compose up -d` to restart. |
| Need to update to a newer version | Run `docker compose pull` then `docker compose up -d`. |
