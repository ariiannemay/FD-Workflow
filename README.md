# 🤖 FD Workflow Bot

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python)
![Discord](https://img.shields.io/badge/Discord.py-2.0%2B-5865F2?style=for-the-badge&logo=discord)
![Status](https://img.shields.io/badge/Status-Production-green?style=for-the-badge)

A specialized Discord bot designed to streamline operations for remote editing teams. This tool automates the assignment queue, calculates strict Turnaround Times (TAT), and manages communication between Coordinators and Editors.

> **Note:** This repository contains the source code for the logic and interface. Sensitive configuration data (tokens, specific role IDs) is managed via environment variables.

---

## ✨ Key Features

### 🔄 Smart Queue Management
* **FIFO Logic:** Automatically manages a waiting list of editors based on arrival time.
* **Persistence:** Uses JSON-based storage (`queue.json`) to ensure the queue order survives bot restarts or downtime.
* **Concurrency Handling:** Prevents users from double-joining and handles disconnects gracefully.

### 🛡️ Role-Based Access Control (RBAC)
* **Invisible Administration:** Sensitive commands (Assigning, Removing, Resetting) are hidden from standard users using `default_permissions` and explicit Role ID checks.
* **Security:** Implements a dual-layer security check (Discord Permissions + Internal ID Verification) to prevent unauthorized usage via API or Emojis.

### ⏱️ Automated Workflow Logic
* **TAT Calculator:** Integrated logic to calculate project deadlines based on audio duration and service type (e.g., Verbatim vs. Non-Verbatim).
* **Assignment Protocol:**
    * Removes editor from queue.
    * Sends a private Direct Message (DM) with instructions.
    * Logs the assignment publicly for accountability.
    * *Fail-safe:* Detects if an editor has DMs blocked and alerts the coordinator publicly.

### 📊 Activity Logging
* **Embed Logging:** All administrative actions (Force removes, priority assignments, discipline notices) are logged to a designated channel using clean, non-intrusive Discord Embeds.

---

## 🛠️ Tech Stack

* **Language:** Python 3.x
* **Library:** `discord.py` (Interactions/Slash Commands)
* **Web Server:** `Flask` (Implemented for keep-alive monitoring)
* **Data Storage:** Local JSON (Lightweight persistence)
* **Formatting:** `datetime` & Discord Magic Timestamps

---

## 🚀 Installation & Setup

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/yourusername/workflow-coordinator-bot.git](https://github.com/yourusername/workflow-coordinator-bot.git)
    cd workflow-coordinator-bot
    ```

2.  **Install dependencies:**
    ```bash
    pip install discord.py flask
    ```

3.  **Configuration:**
    Create a `.env` file in the root directory (do not upload this to GitHub):
    ```env
    DISCORD_TOKEN=your_bot_token_here
    ```

4.  **Role Configuration:**
    Open `main.py` and update the `SWC_ROLE_IDS` list with the Role IDs of your coordinators:
    ```python
    # main.py
    SWC_ROLE_IDS = [123456789012345678, 987654321098765432]
    ```

5.  **Run the Bot:**
    ```bash
    python main.py
    ```

---

## 📖 Command Reference

### 🟢 Public Commands (For Editors)
| Command | Description | Visibility |
| :--- | :--- | :--- |
| `/available` | Adds user to the work queue. | Public + DM |
| `/optout` | Removes user from the queue. | Ephemeral (Private) |
| `/tattimer` | Opens the Deadline Calculator. | Ephemeral (Private) |
| `/tatdelay` | Form to request a deadline extension. | Public Log |
| `/fileupdate` | Form to submit file status (e.g., "Uploading"). | Public Log |
| `/plannedavailability` | Submit scheduled leave/absence. | Public Log |
| `/unplannedavailability`| Submit emergency absence report. | Public Log |

### ⛔ Admin Commands (Coordinator Only)
*These commands are invisible to standard users.*

| Command | Description |
| :--- | :--- |
| `/queue` | View the backend waitlist (with timestamps). |
| `/assign` | Assign a file to a user (triggers DM notification). |
| `/remove` | Forcefully remove a user from the queue. |
| `/resetqueue` | Wipe the entire list (End of Shift). |
| `/reassign_notif` | Send a disciplinary reassignment DM. |
| `/askfileupdate` | Publicly ping an editor for a status report. |
| `/setlogchannel` | Configure where logs are sent. |

---

## 📸 Usage Snippets

**1. The Assignment Flow:**
> Coordinator uses `/assign` or Right-Click Context Menu $\rightarrow$ Bot calculates timestamps $\rightarrow$ Bot DMs Editor $\rightarrow$ Bot updates public log.

**2. TAT Calculator:**
> User inputs `01:30:00` audio length $\rightarrow$ Bot applies specific multipliers based on file type (e.g., Client A vs. Client B) $\rightarrow$ Returns exact deadline timestamps.

---

## 🔒 Confidentiality Notice

This repository contains sanitized code. All proprietary client names, specific pricing logic, and private webhook URLs have been removed or replaced with generic placeholders to protect business confidentiality.

---

## 📝 License

This project is open-source and available under the [MIT License](LICENSE).
