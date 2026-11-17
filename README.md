# Competition Lab — live multiplayer Prisoner's Dilemma

A runnable single-server classroom simulation for 9–10 Global Business students. Students connect on their phones by QR code and play 12 rounds in three phases. The facilitator controls the game and can export a CSV of individual decisions and payoffs.

## Game design
- **Phase 1:** rounds 1–4, randomized pairs; no communication.
- **Phase 2:** rounds 5–8, fixed pairs; no communication (repeated game, possible tit-for-tat).
- **Phase 3:** rounds 9–12, same fixed pairs; private chat available.
- Actions: **Maintain price** (C/accommodative) and **Cut price** (D/aggressive).
- Payoffs per player, in fictional €000: C/C 50/50; C/D 0/60; D/C 60/0; D/D 30/30, as shown in Peng & Meyer (2023), Chapter 13, Fig. 13.1.
- A fixed set of pairs is generated for Phase 2 and reused unchanged for Phase 3.
- An odd number of players causes a scripted **Study Bot** to fill the final pairing. Its moves are random in phase 1 and tit-for-tat in phases 2 and 3. For best comparability, **have a facilitator join as the 10th player** when there are nine students.
- The host can force completion if someone disconnects: missing decisions default to Cut and are flagged in exported data.
- Chat transcripts are visible to the host for debriefing; do not project private messages without consent.
- Single-instance in-memory storage: **game progress is lost if the server restarts**. Export before shutdown and don't run multiple replicas.

## Local run
Requires Python 3.11+.

```sh
pip install -r requirements.txt
HOST_PIN=ChooseAStrongPrivatePIN python server.py
```

Then open `http://localhost:3000/host` on the facilitator laptop. Create a room, then display the QR code to students. **To have phones join locally, your server must be reachable over the network and you must open the host page using that reachable address, not localhost.**

## Deploy on Render.com

1. Create a GitHub repository containing this folder's contents (server.py, requirements.txt, render.yaml, public/).
2. In Render, create a **Blueprint** from the repository. Render uses the included `render.yaml` to set up a Python web service.
3. Set the `HOST_PIN` environment variable to a private strong PIN (do not expose this PIN on the slides).
4. After deployment, open `https://YOUR-RENDER-URL/host` and create a room.
5. Project the QR code; students join through their phone browsers.
6. Consider using an always-on hosting plan for a live class. Free services can sleep, restart, or lose the in-memory game if the service restarts.

Alternatively, any platform that runs a single Python web service with WebSockets and persistent HTTP connections can host it.

## Facilitator workflow
1. Create room; share QR.
2. Wait for 10 players when possible (or allow Study Bot for 9 players).
3. Start Round 1; players choose without knowing opponent decisions. Results auto-reveal when everyone submits.
4. After discussing results, click Start Next Round; repeat 12 rounds.
5. At phase boundaries, show charts only after the phase ends.
6. Export CSV and use its individual scores, choice history, phase means, and cooperation rates for your comparison.

## Important interpretation safeguards
- Phase 1 varies partners, Phase 2 adds repetition, Phase 3 adds chat: this supports discussion but **does not prove causality** in a small sample.
- Repetition can promote tacit accommodation without an explicit agreement; **express price fixing and market allocation can violate Article 101 TFEU**. This game simulates incentives for educational purposes only.
- Results with a bot, forced submissions, or disconnections should be disclosed in debrief.
- All data is held on the server process while running; nicknames need not be real names. Do not use student emails or collect sensitive personal information.

## Links to course material
Peng & Meyer, *International Business*, 4th edition, Chapter 13, Figure 13.1, Prisoner's Dilemma and repeated games.
