🫧 Bubble
Raise it. Back it. Pop it.
Bubble is a suggestion-and-escalation board for teams. Anyone raises a problem they hit in their own area of work, suggests a fix, and rates how badly it hurts. Every colleague who backs it makes the bubble bigger. The ops team opens a field of bubbles with the biggest one in the centre — and can only pop a bubble by writing an answer, so nothing disappears without a record.
Single HTML file. No build step, no server, no dependencies. Data lives in the browser's `localStorage`.
---
Run it
Locally — download `index.html` and double-click it. That's the whole install.
Locally over http (recommended if you want fonts and refresh behaviour identical to a hosted copy):
```bash
git clone https://github.com/<you>/bubble.git
cd bubble
python3 -m http.server 8080
# open http://localhost:8080
```
On GitHub Pages — push `index.html` to the repo root, then Settings → Pages → Deploy from branch → `main` / `root`. Live in about a minute.
---
The two sides
I raise bubbles (the user side)
Set your name once, top right.
My bubbles — everything you've raised and where it stands.
Everyone's bubbles — back anyone else's with one click; each backer grows it.
Popped & archived — the answer ops gave, kept so you can circle back.
Raise a bubble captures: name/ID, area of work, process, problem, your fix, severity 1–5, and the timestamp.
Ops team (the admin side) — access code `1234`, changeable in Settings
Lands straight on the bubble field: biggest bubble dead centre, smaller ones radiating outward in every direction, colour by severity.
List toggles to a ranked table, biggest first; sort by size, severity, area of work, age, or backers. Filter by area or internal/customer-facing, and search.
Click a bubble or a row to expand it.
Pop this bubble stays disabled until an answer is written. Popped bubbles move to the Archive with the answer attached.
---
How a bubble gets its size
```
radius = (26 + √backers × 17 + severity × 4.5) × (1 + neglect × 0.85)
```
Backers — the person who raised it counts as one; every "Back this" adds another.
Severity — the 1–5 rating, also the bubble's colour: 1 green, 2 blue, 3 violet, 4 amber, 5 red.
Neglect — customer-facing bubbles hold steady for 30 days. From day 31 they swell, hitting maximum size on day 50. Untouched on day 50 they expire into the Archive, marked expired unanswered — still on the record. Internal bubbles don't expire; they grow only through backing.
Customer-facing bubbles also capture time lost each week, time spent raising the request, and whether the fix wins that time back. The detail view turns those into a payback multiple.
---
Demoing it at work
Settings has a demo clock slider. Drag it forward and the field re-renders as if days had passed — neglected customer bubbles visibly swell past day 30 and expire at day 50. It's the fastest way to show the whole lifecycle in ten seconds.
Ten seeded bubbles load on first run so the field isn't empty in front of an audience. Reload demo bubbles resets them; Erase everything clears the field for a live run-through. Export / Import moves data between machines as JSON.
A four-minute walkthrough that lands well:
Raise a bubble as yourself, severity 4, customer-facing.
Change your name, back it — watch it grow in the field.
Switch to Ops, open the field, show biggest-in-centre and the list view sorted by area.
Drag the demo clock to day 45 and show what neglect does to the field.
Open the biggest bubble, try to pop it without an answer, then write one and pop it.
Show it in the Archive with the answer attached.
---
If you take it past a demo
Everything is per-browser today, which is right for a laptop demo and wrong for a real team. To share a field across people you need a backend: the app state is a single JSON object (`{ bubbles, me, pin, offset, theme }`), so the smallest real version is a `GET /bubbles` and `POST /bubbles` against SQLite, swapped in where `load()` and `save()` sit at the top of the script. Worth adding at the same time: real sign-in so `backers` can't be gamed, one bubble per person per problem, and email to the raiser when their bubble is popped.
---
MIT licensed. Built to be argued with — fork it and change the growth curve if your floor works differently.
