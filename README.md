# MavBin: Smart Trash Sorter

**Team MavBuds** · CSE 4316 Senior Design I · UTA · Fall 2026

A camera on a Raspberry Pi identifies each item dropped into the bin, and a rotating carousel (ADS Option B) turns it to the right bin. Later phases add a backend, a touch screen on the bin, and student and staff apps.

**Sprint 4 goal (review Dec 1):** first end-to-end run. The camera sees an item, the Pi classifies it, and the motor turns the platter to the correct bin.

---

## Team

| Role | Member | Owns (ADS subsystem) |
|---|---|---|
| Product Owner | Shramika | Backlog, priorities, acceptance criteria |
| Scrum Master | Prajita (@gyawaliprajita50) | Reviews and merges into `dev` and `main`, repo settings, sprint records |
| Software | Masma (@masma108) | D2 Vision: dataset, model, accuracy tests |
| Software | Salsabeel (@salsabeelhossain) | D3 Sort Controller, D5 Event Uploader, Pi setup, model deployment |
| Hardware | Sudip (@sudippaudel01) | H1 Imaging Chamber, H2 Sorting Mechanism, H3 Fill Sensors |
| Hardware | J’son (@24kwchappell) | D1 Motion and Safety Controller (ESP32), H4 Safety and Power |

---

## Branches

```
main        demo-ready only. Updated from dev at the end of each sprint.
 └── dev    integration. Every pull request targets dev.
      ├── hw/platter-drive
      ├── hw/sensors
      ├── hw/esp32-control
      ├── sw/vision
      ├── sw/pi-controller
      ├── sw/backend
      └── sw/apps
```

| Branch | What goes here | Owner | Folder |
|---|---|---|---|
| `hw/platter-drive` | Concave platter, flywheel and gear ring, side-mounted motor (CAD, drawings, test logs) | @sudippaudel01 | `hardware/platter/` |
| `hw/sensors` | Camera mount, presence sensor, fill sensors, pin map and wiring diagrams | @sudippaudel01 | `hardware/sensors/` |
| `hw/esp32-control` | ESP32 firmware, serial commands (ADS Table 3), flap/door interlocks, E-stop | @24kwchappell | `firmware/` |
| `sw/vision` | Labels, Edge Impulse model, inference on the Pi, accuracy results | @masma108 | `vision/` |
| `sw/pi-controller` | Main Pi program, sort logic, ESP32 simulator, event store and upload | @salsabeelhossain | `pi/` |
| `sw/backend` | API, rewards engine, database (S1 to S3) | Software team, owner TBD | `backend/` |
| `sw/apps` | Bin touch screen, student app, staff app (D4, C1, C2). Starts after the core sorter is accepted (SRS §3). | Owner TBD | `apps/` |

Docs, test results, and the dataset link go in `docs/`. Anyone can open a small PR for those.

---

## How to work

**First time**

```bash
git clone <repo-url>
cd MavBin
git checkout <your-branch>
```

**Every time you start**

```bash
git checkout <your-branch>
git pull origin dev
```

**When something works**

```bash
git add <the files you changed>
git commit -m "esp32: add STOP command for the flap interlock"
git push
```

Then open a pull request on GitHub with base `dev`, ask one teammate to review it, and @gyawaliprajita50 merges it.

---

## Do

- **Pull `dev` before you start working.** Merge conflicts stay small that way.
- **Stay in your own folder.** If you need to change someone else's, tell them first.
- **Make small commits with clear messages.** Start with the area, like `vision: retrain on 200 chamber photos`, not `update` or `fix stuff`.
- **Open a PR for each working piece**, and link the backlog story (`Closes #12`).
- **Agree on the serial commands before you code them.** @24kwchappell and @salsabeelhossain both approve any change to ADS Table 3. It is the one place where hardware and software meet.
- **Keep pins, ports, and thresholds in one config file.** Don't scatter magic numbers.
- **Write down what you measured** (rotation time, stop time, accuracy, power) in `docs/tests/` with the date. Sprint Review slides come from these results.
- **Commit wiring diagrams and pin maps** together with the code that uses them.
- **Say so at Daily Scrum (Tue/Thu/Fri)** if you have been blocked for more than a day.

## Don't

- **Don't push to `main` or `dev` directly.** Always go through a PR.
- **Don't merge your own PR.**
- **Don't force-push** (`git push --force`) to a shared branch.
- **Don't commit secrets:** Wi-Fi passwords, API keys, Edge Impulse keys. Put them in `.env`, which git ignores, and add the variable names without values to `.env.example`.
- **Don't commit the photo dataset, videos, or files over 50 MB.** Keep them in Edge Impulse or the shared drive and put the link in `docs/`.
- **Don't copy code from `gpt-lab-summer-2026/trash-sorter`.** It has no license. Check the TrashNet and TACO licenses before you train on them, and list them in `docs/`.
- **Don't run new motor code on the real build first.** Test it with the simulator or with motor power off, and keep the E-stop within reach.
- **Don't commit junk** like `__pycache__/`, `.DS_Store`, `venv/`, or build folders. `.gitignore` already covers them.

---

## Scrum Master setup (@gyawaliprajita50)

1. **Settings → Collaborators:** invite all five teammates.
2. **Settings → Branches:** add rules for `main` and `dev`: require a pull request, require 1 approval, and block force pushes.
3. **Settings → General:** set the default branch to `dev`.
4. **Projects:** create a board and add the backlog stories as issues so PRs can close them.
