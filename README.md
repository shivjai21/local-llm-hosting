# Local LLM Hosting with Ollama, Open WebUI, and Cloudflare

This repository documents the complete workflow for downloading, running, and securely exposing Large Language Models (LLMs) locally using Ollama, Open WebUI, and Cloudflare Tunnels. It also includes the necessary steps to securely version control this project using Git and SSH.

## 🚀 Features
* **Local AI:** Run models completely offline using Ollama.
* **ChatGPT-like Interface:** Host a clean, user-friendly UI using Open WebUI within an isolated Conda environment.
* **Zero-Install Public URL:** Expose the local interface to the internet instantly using native SSH reverse port forwarding.
* **Secure Access:** Expose the local interface to the internet securely via Cloudflare Tunnels without opening router ports.


## 📋 Prerequisites
Before you begin, ensure you have the following installed:
* [Miniconda](https://docs.conda.io/en/latest/miniconda.html) or Anaconda
* [Ollama](https://ollama.com/)
* A system with a built-in SSH client (macOS/Linux have this natively)
* [Cloudflared](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/) (Cloudflare Tunnel Daemon)


---

## 🛠️ Step-by-Step Setup

### 1. Pull and Run a Model with Ollama
First, start the Ollama service and pull a model of your choice (e.g., Llama 3).
\`\`\`bash
ollama run llama3
\`\`\`
*Leave this terminal running or ensure the Ollama app is active in your background.*

### 2. Setup Open WebUI using Conda
We use Conda to create an isolated environment to keep dependencies clean and prevent conflicts.

Open a new terminal and run:
\`\`\`bash
# Create a new Conda environment named 'open-webui' with Python 3.11
conda create -n open-webui python=3.11 -y

# Activate the Conda environment
conda activate open-webui

# Install Open WebUI
pip install open-webui

# Start the Open WebUI server
open-webui serve
\`\`\`
*The UI should now be accessible locally at `http://localhost:8080`.*

### 3. Expose the UI using an SSH Reverse Tunnel
To access your WebUI from anywhere in the world, we will use native SSH to forward your local port (8080) to a public forwarding service (like localhost.run).

Open a third terminal window and run:
\`\`\`bash
ssh -R 80:localhost:8080 nokey@localhost.run
\`\`\`

**How this works:**
* `ssh`: Calls your system's built-in SSH tool.
* `-R 80:localhost:8080`: Tells the remote server to forward public traffic on its port 80 down to your machine's port 8080.
* `nokey@localhost.run`: Connects to a free, public SSH tunneling service.

The terminal will output a unique, secure URL (e.g., `https://your-random-string.lhr.life`). Copy this link and paste it into your browser on any device to access your local AI globally!

### 4. Expose the UI using Cloudflare Tunnels
To access your WebUI from anywhere in the world securely, tunnel port 8080.

Open a third terminal window and run:
\`\`\`bash
cloudflared tunnel --url http://localhost:8080
\`\`\`
Cloudflare will generate a unique `.trycloudflare.com` URL. Copy this link and paste it into your browser on any device to access your local AI.
