# 🧠 Hunch — How Well Do You Really Know Your Rankings?

\<p align="center">
&#x20; \<strong>Think you know what's best? Prove it without seeing what's next.\</strong>
&#x20; \<br>\<br>
&#x20; A blind ranking game where every decision counts.
&#x20; \<br>\<br>
&#x20; \<a href="[https://shekharsanskar9.github.io/Hunch/](https://shekharsanskar9.github.io/Hunch/)">\<strong>🎮 Play Hunch\</strong>\</a>
&#x20; &nbsp; · &nbsp;
&#x20; \<a href="[https://github.com/shekharsanskar9/Hunch](https://github.com/shekharsanskar9/Hunch)">⭐ GitHub Repository\</a>
\</p>

---

## 🎮 About the Game

What is the biggest planet? Which game deserves the top spot? Who is the greatest athlete?

You probably have an opinion. But can you rank them correctly when you don't know what's coming next?

**Hunch** is a blind ranking game that tests your knowledge, intuition, and decision-making.

Items appear one at a time. Your task is to assign each item a position without knowing what comes next. Every placement is final, so think carefully before locking it in!

\<details>
\<summary>\<strong>👀 See how it works\</strong>\</summary>

Imagine ranking planets by size:

1. Jupiter appears. You place it at #1.
2. Mars appears. You place it at #3.
3. Saturn appears. You must decide whether it belongs at #1 or #2.
4. Earth appears. You have to work with the remaining positions.

Once all items are placed, your ranking is compared with the actual order.

**Remember: You cannot change your previous decisions!**

\</details>

## 🚀 Game Modes

| Mode                  | Description                                                      |
| --------------------- | ---------------------------------------------------------------- |
| 🎯 Ready-to-Play      | Explore 41 preset rankings across 23 categories.                 |
| 🛠️ Custom Rankings   | Create your own Top 3–10 challenges and share them with friends. |
| 🌐 Community Rankings | Discover and play rankings created by other players.             |
| 🧠 Mind-Reader Mode   | Predict how your friend ranked each item.                        |

\<p align="center">
&#x20; \<a href="[https://shekharsanskar9.github.io/Hunch/](https://shekharsanskar9.github.io/Hunch/)">\<strong>🧠 START PLAYING →\</strong>\</a>
\</p>

---

## ✨ Features

- 🕶️ **Blind Ranking:** Place items one at a time without knowing what's coming next.
- 🗂️ **41 Preset Rankings:** Explore 23 categories, including science, sports, coding, games, and brands.
- 🛠️ **Custom Rankings:** Build and share your own Top 3–10 challenges.
- 🎯 **Scoring System:** Earn points based on the accuracy of your placements.
- 🏆 **Leaderboards:** Track personal bests and compete with other players.
- 🌐 **Community Rankings:** Sign in with Google to publish and explore rankings.
- 🧠 **Mind-Reader Mode:** Exchange guesses and predict your friend's choices.
- 💾 **Local-First Gameplay:** Play without Firebase using browser storage and shareable links.
- 🎉 **Interactive Experience:** Enjoy animations, sound effects, confetti, and score reveals.

## 🏆 Scoring System

Your score depends on how close each placement is to its actual position.

| Placement Accuracy         |  Points |
| -------------------------- | ------: |
| Exact position             | **100** |
| Off by 1 position          |      60 |
| Off by 2 positions         |      30 |
| Off by 3 or more positions |       0 |

### Your Hunch Level

Your final percentage determines your rank.

| Rank          |         Score | Description             |
| ------------- | ------------: | ----------------------- |
| 🐣 Rookie     |     Below 40% | Just getting started    |
| ⚔️ Challenger | 40% and above | Building your instincts |
| 🎓 Expert     | 60% and above | Knows their rankings    |
| 👑 Master     | 80% and above | Almost unbeatable       |
| 🏆 Legend     |          100% | Perfect ranking!        |

**Can you earn the Legend title?**

---

## 🛠️ Tech Stack

Hunch is built as a lightweight, dependency-free frontend application.

| Technology               | Purpose                             |
| ------------------------ | ----------------------------------- |
| HTML5                    | Application structure               |
| CSS3                     | Styling and animations              |
| Vanilla JavaScript       | Game logic and interactions         |
| Firebase Authentication  | Optional Google sign-in             |
| Cloud Firestore          | Community rankings and leaderboards |
| Browser localStorage     | Local data persistence              |
| URL Hash + JSON + Base64 | Shareable custom rankings           |
| GitHub Pages             | Deployment                          |

### Why Vanilla JavaScript?

The entire application lives in a single `index.html` file.

- No frontend framework.
- No bundler or build process.
- No npm dependencies.
- Easy to run, inspect, and extend.

This keeps Hunch lightweight and straightforward to maintain.

## 🏗️ Project Structure

```text
Hunch/
│
├── index.html          # UI, styling, game logic and Firebase integration
├── firestore.rules     # Firestore security and validation rules
├── firebase.json       # Firebase CLI configuration
├── .firebaserc         # Default Firebase project
├── README.md           # Project overview and setup
├── DOCUMENTATION.md    # Technical architecture and implementation
└── FIREBASE_SETUP.md   # Firebase configuration guide
```

## 💾 Local Mode vs Online Mode

Hunch supports two gameplay environments.

### Local Mode

No Firebase configuration or account is required.

- Play preset and custom rankings.
- Save statistics using browser storage.
- Generate and share custom ranking links.
- Access core gameplay without an internet-connected backend.

### Online Mode

Firebase enables additional community features:

- Google authentication.
- Community ranking publication.
- Online leaderboards.

Publishing is limited to five rankings per UTC day, enforced through Firestore security rules.

---

## 💻 Getting Started

Want to run Hunch locally or explore the source code?

### Prerequisites

- A modern web browser.
- Git.
- Python 3 (optional, for running a local HTTP server).

### 1. Clone the repository

```bash
git clone https://github.com/shekharsanskar9/Hunch.git
cd Hunch
```

### 2. Run the application

**Option 1: Open directly**

Open `index.html` in your browser.

**Option 2: Start a local server**

```bash
python3 -m http.server 8000
```

Open `http://localhost:8000` in your browser.

### 3. Configure Firebase (Optional)

To enable Google sign-in, community rankings, and online leaderboards:

1. Follow the instructions in `FIREBASE_SETUP.md`.
2. Configure your Firebase project.
3. Enable Google Authentication.
4. Add your Firebase configuration to the application.
5. Deploy the Firestore security rules.

Local gameplay works without Firebase.

---

## 🔒 Security & Limitations

- Firestore rules validate published rankings and enforce ownership.
- Users can delete their own published rankings.
- Leaderboard entries are restricted to authenticated users' own records and best-score updates.
- Daily publishing limits are enforced through Firestore security rules.
- Scores are submitted by the client. Server-side gameplay verification is not implemented, so users could submit fabricated scores within the permitted bounds.

For more details, refer to:

- [Technical Documentation](DOCUMENTATION.md)
- [Firebase Setup Guide](FIREBASE_SETUP.md)

## 🗺️ Future Improvements

- [ ] Expand preset rankings and community topics.
- [ ] Improve mobile gameplay and accessibility.
- [ ] Introduce additional competitive game modes.
- [ ] Explore stronger server-side score validation.

## 👨‍💻 Developer

**Sanskar Shekhar**
Computer Science and Engineering · BIT Mesra

- [GitHub Profile](https://github.com/shekharsanskar9)
- [Project Repository](https://github.com/shekharsanskar9/Hunch)
- [Live Application](https://shekharsanskar9.github.io/Hunch/)

---

\<p align="center">   \<strong>Built for curious minds and confident guesses.\</strong>   \<br>   Play a round. Trust your hunch. 🧠   \<br>\<br>   \<a href="[https://shekharsanskar9.github.io/Hunch/](https://shekharsanskar9.github.io/Hunch/)">\<strong>🎮 PLAY HUNCH\</strong>\</a> \</p>
