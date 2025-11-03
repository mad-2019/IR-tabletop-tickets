# 🧠 IR Tabletop Tickets

A lightweight, **automated incident response tabletop simulation tool** using **GitHub Actions**.  
It periodically creates fake incident "alerts" as GitHub Issues — simulating a real SOC alert flow for distributed tabletop exercises.

This version uses **multiple inject files** (e.g., `inject1.txt`, `inject2.txt`) and runs each inject **line by line**, generating issues sequentially and automatically posting a **summary** when an inject completes.

---

## 🚀 Features

- Sequential inject delivery (each line = one issue)
- Supports multiple inject files (e.g., `inject1.txt`, `inject2.txt`)
- Random inject file selection per new run
- Automatic summary with start/end/duration tracking
- Works with GitHub Free plan (using `cron` every 6 minutes)
- Simple text-based injects — easy to edit, no external dependencies

---

## 🧱 Repository Structure

IR-tabletop-tickets/
├── .github/
│ └── workflows/
│ └── scheduled-issues.yml # Main automation workflow
├── inject1.txt # Example inject set
├── inject2.txt # Example inject set
├── current_index.txt # Tracks current inject line
├── current_inject.txt # Tracks current inject file
├── inject_start_time.txt # Tracks start timestamp of inject
└── README.md

1. **Clone or fork this repository**
   ```bash
   git clone https://github.com/mad-2019/IR-tabletop-tickets

2. Create inject files
Each inject file must contain one inject per line, example:
Suspicious login detected
Firewall rule changed unexpectedly
Privilege escalation on web server

3. Configure the workflow
File: .github/workflows/scheduled-issues.yml
Adjust the schedule if needed:
schedule:
  - cron: "*/6 * * * *"  # Every 6 minutes

4. Commit and push changes
This automatically enables the scheduled GitHub Action.

**Running the Simulation**
Option 1 – Automatic (Scheduled)
The workflow runs automatically every 6 minutes.
Each run posts the next inject from the current inject file.

Option 2 – Manual Trigger
You can manually start it:
1. Go to Actions → Tabletop Incident Injects → Run workflow
2. Choose:
reset: false → continue current inject
reset: true → start a new random inject file

Output
Each inject line creates a new GitHub Issue like:

🚨 [inject2.txt] Security Alert - 2025-10-30 21:12:01 UTC

When the inject file finishes, a summary issue is created automatically:
✅ Completed Inject File: inject2.txt
Start: 2025-10-30 21:12:01 UTC
End:   2025-10-30 21:24:31 UTC
Duration: 12 minutes

Resetting
To restart from scratch (inject #1):
Delete or reset the following files:
current_index.txt
current_inject.txt
inject_start_time.txt

Commit and push.
Run the workflow manually.

Troubleshooting
Issue	Possible Cause	Fix
Only one issue appears	GitHub Actions free accounts limit cron frequency (min 5–6 mins)	Wait 6+ mins or trigger manually
Workflow fails to commit index	Make sure current_index.txt is in the repository root, not inside .github/	
No injects picked	Ensure inject1.txt, inject2.txt, etc. exist in root	
Workflow doesn’t run	Verify Actions are enabled in the repo → “Settings → Actions → General → Allow all actions”	

Author
Created by Mauricio Díaz
Designed for cybersecurity tabletop simulation and training.
