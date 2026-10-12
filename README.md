# 🧠 Hunch — How Well Do You Really Know Your Rankings?

**A blind ranking game where every decision counts.**

Think you know the biggest planets, best games, most popular brands, or greatest athletes? Hunch puts your intuition to the test. Rank each item as it appears, lock in your choice, and see how close you came to the real order.

<p align="center">
  <a href="https://shekharsanskar9.github.io/Hunch/">
    🎮 Play Hunch
  </a>
  &nbsp; | &nbsp;
  <a href="https://github.com/shekharsanskar9/Hunch">
    GitHub Repository
  </a>
</p>

---

## 🎮 How to Play

1. **Pick a ranking** — Choose a topic or create your own.
2. **Trust your instincts** — Items appear one at a time, and you must assign each one a rank without seeing what's coming next.
3. **Lock it in** — Every placement is final. No changing your mind!
4. **Reveal your score** — Compare your guesses against the actual ranking and find out whether you're a Rookie, Challenger, Expert, Master, or Legend.

**The challenge?** You need to think ahead when you don't know what comes next.

## ✨ Features

| Feature                       | Description                                                                                                  |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------ |
| 🕶️ **Blind Ranking**         | Place items one at a time without knowing the upcoming items.                                                |
| 🗂️ **Ready-to-Play Topics**  | Explore 41 preset rankings across 23 categories, from science and sports to coding, games, brands, and more. |
| 🛠️ **Create Your Own**       | Build custom Top 3–10 rankings and challenge friends using shareable links.                                  |
| 🎯 **Scoring System**         | Earn 100 points for an exact placement, 60 for one position away, and 30 for two positions away.             |
| 🏆 **Leaderboards**           | Compete for your best score on individual rankings and climb the all-time player leaderboard.                |
| 🌐 **Community Rankings**     | Sign in with Google to publish rankings and explore creations from other players.                            |
| 🧠 **Mind-Reader Mode**       | Exchange guesses with a friend and predict how they ranked each item.                                        |
| 💾 **Local-First Gameplay**   | Play without Firebase using browser-based storage and shareable ranking links.                               |
| 🎉 **Interactive Experience** | Enjoy animated feedback, sound effects, confetti, and score reveals.                                         |

## 🧠 The Scoring System

Your score depends on how close each placement is to the correct position.

| Placement          | Points |
| ------------------ | -----: |
| Exact position     |    100 |
| Off by 1 position  |     60 |
| Off by 2 positions |     30 |
| Off by 3 or more   |      0 |

Your final percentage determines your title:

- 🐣 **Rookie** — Getting started
- ⚔️ **Challenger** — 40% and above
- 🎓 **Expert** — 60% and above
- 👑 **Master** — 80% and above
- 🏆 **Legend** — 100%

*Can you earn the Legend title?*

## 🛠️ Tech Stack

Hunch is built as a lightweight, dependency-free frontend application.

- **Frontend:** HTML5, CSS3, Vanilla JavaScript
- **Backend / Cloud:** Firebase Authentication and Cloud Firestore (optional)
- **Data persistence:** Browser `localStorage`
- **Sharing:** URL hash routing with JSON and Base64 encoding
- **Deployment:** GitHub Pages

### Why Vanilla JavaScript?

The entire application lives in a single `index.html` file. There is no framework, bundler, build step, or npm dependency. This keeps the application lightweight and easy to run, inspect, and extend.

## 🏗️ Architecture

```text
Hunch
│
├── index.html          # UI, styles, game logic and Firebase integration
├── firestore.rules     # Firestore security and validation rules
├── firebase.json       # Firebase CLI configuration
├── .firebaserc         # Default Firebase project
├── README.md           # Project overview and setup
├── DOCUMENTATION.md    # Technical architecture and implementation details
└── FIREBASE_SETUP.md   # Firebase configuration guide
```

**Two ways to play:**

- **Local mode:** Rankings, gameplay, scoring, and shareable links work without a Firebase backend. Locally created rankings and statistics use browser storage.
- **Online mode:** Firebase adds Google sign-in, community rankings, and leaderboards. Publishing is limited to five rankings per UTC day, enforced by Firestore security rules.

For the complete implementation details, see [DOCUMENTATION.md](DOCUMENTATION.md).

## 🚀 Getting Started

### Prerequisites

A modern web browser. Python 3 is optional and only needed if you prefer running a local HTTP server.

### Run locally

**Option 1: Open directly**

Clone the repository and open `index.html` in your browser.

```bash
git clone https://github.com/shekharsanskar9/Hunch.git
cd Hunch
```

**Option 2: Use a local server**

```bash
python3 -m http.server 8000
```

Open [http://localhost:8000](http://localhost:8000) in your browser.

### Enable Firebase (optional)

To enable Google sign-in, community rankings, and online leaderboards:

1. Follow the instructions in [FIREBASE_SETUP.md](FIREBASE_SETUP.md).
2. Configure the Firebase project and its authentication settings.
3. Add the Firebase configuration to the application.
4. Deploy the Firestore security rules.

Without Firebase configuration, the game remains playable in local mode.

## 🔒 Security Notes

- Firestore security rules validate published rankings, enforce ownership, and apply the daily publishing limit.
- Ranking data is immutable after publication, except that the creator can delete their own ranking.
- Leaderboard entries are restricted to the authenticated user's own record and best-score updates.
- Scores are submitted by the client, so server-side gameplay verification is not implemented. A determined user could submit a fabricated score within the permitted bounds.

See [FIREBASE_SETUP.md](FIREBASE_SETUP.md) for configuration and [DOCUMENTATION.md](DOCUMENTATION.md) for the security model.

## 🗺️ Roadmap Ideas

- [ ] Add more ranking topics and community-created presets.
- [ ] Improve mobile gameplay and accessibility.
- [ ] Introduce additional competitive game modes.
- [ ] Explore stronger server-side score validation.

## 👨‍💻 Author

**Sanskar Shekhar**
Computer Science and Engineering · BIT Mesra

[GitHub](https://github.com/shekharsanskar9) · [Play Hunch](https://shekharsanskar9.github.io/Hunch/)

---

\<p align="center">
&#x20; \<strong>Built for curious minds and confident guesses.\</strong>\<br>
&#x20; Play a round. Trust your hunch. 🧠
\</p>
