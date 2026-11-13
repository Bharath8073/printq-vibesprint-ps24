# PrintQ — Campus Print-Shop Token & Queue Simulator (PS-24)

**Event:** OWASP NIE VibeSprint 1.0 (Nov 13, 2026 · AI-ML Lab 4)  
**Problem Card:** PS-24 (`PrintQ` — Useful & Practical Track)  
**Prototype Type:** 100% Browser-First Static Web Application (Zero external APIs, zero `.env` keys, fictional campus sample data)

## 1. Problem Statement & Solution Overview

Students submitting print jobs at campus print shops face unpredictable wait times and crowded counters, while shop operators struggle to track job states and urgent requests transparently.

**PrintQ** solves this with a dual-view browser simulator:
1. **Student Kiosk & Live Token Pass:** Students enter their document name, page count, copy count, and colour preference (`Black & White` or `Full Colour`). They immediately receive a sequential token (`PQ-104`, etc.), transparent cost breakdown, queue position, and dynamic pickup ETA.
2. **Shop Operator Queue Console:** The shop sees a live ordered queue with `Pending`, `Printing`, and `Ready` status toggles, `↑`/`↓` manual queue reorder controls, and a `↺ Reset Queue` button.
3. **Minute-40 Paired Twist (Rush-Hour Queue & Urgent Override Confirmation):**
   - **Dynamic Rush-Hour Wait Calculation:** Estimates wait times from page count, copy count, and print mode (`B&W` vs `Colour`), with a 1-click **Rush-Hour Toggle (`1.5x` print duration + `1.0 min` batch buffer per job)**.
   - **Single Urgent Job Override with Mandatory Delay Confirmation:** Operators can mark one job `⚡ Urgent` only after reviewing and confirming a modal that calculates and displays the exact delay impact (`+X.X mins`) on all other pending jobs in the queue.

## 2. Visible Wait-Time & Cost Formula

| Parameter | Black & White (B&W) | Full Colour | Notes |
| :--- | :--- | :--- | :--- |
| **Print Speed** | `15 pages / min` (`4 sec/page`) | `5 pages / min` (`12 sec/page`) | Base hardware speed |
| **Per-Page Rate** | `₹1.50 / page` | `₹5.00 / page` | Fictional student tariff |
| **Job Setup Buffer** | `+0.5 min` (`30 sec`) | `+0.5 min` (`30 sec`) | Binding & tray loading |
| **Rush-Hour Twist** | `1.5x` + `1.0 min` buffer | `1.5x` + `1.0 min` buffer | Toggleable in top bar |

**Formula per Job:**
$$\text{Job Time (min)} = \begin{cases} (\text{Pages} \times \text{Copies} \times \text{Rate}) + 0.5 & \text{(Normal Hours)} \\ \left((\text{Pages} \times \text{Copies} \times \text{Rate}) + 0.5\right) \times 1.5 + 1.0 & \text{(Rush-Hour Twist)} \end{cases}$$

**Cumulative Queue Wait:** Sum of remaining print durations for all active jobs (`Printing` at 60% remaining + `Pending` jobs ahead in queue).

## 3. Judge 3-Step Verification Path

| Step | Judge Action | Expected Outcome in PrintQ |
| :--- | :--- | :--- |
| **STEP 01** | Submit a print job and show its token and wait estimate | Fill the Student Kiosk form (or click `+ Lab Record`) and click **Generate Print Token & Join Queue**. A new token (`PQ-104`) appears in the Student Token Pass and Shop Queue with exact wait minutes and ETA clock time. |
| **STEP 02** | Change the job status in the shop queue | In the Shop Operator Console, click **Printing** or **Ready** on any job card. The status pill, queue counters, and student token tracker update immediately. |
| **STEP 03** | Mark a job urgent and show the required confirmation | Click **⚡ Mark Urgent** on any pending job. A modal displays the exact `+X.X min` delay impact on all other pending jobs. Clicking **Confirm Urgent & Delay Other Jobs** moves the job to the front of the pending queue with an `⚡ URGENT PRIORITY` badge. |

## 4. How to Run Locally or Deploy on GitHub Pages

1. Clone or download this repository containing `index.html` and `README.md`.
2. Double-click `index.html` to open it directly in any modern browser (Chrome, Edge, Firefox, Safari) — no `npm install`, backend server, or `.env` configuration needed.
3. For a shareable live URL, enable **GitHub Pages** (`Settings` → `Pages` → Deploy from `main` branch).
