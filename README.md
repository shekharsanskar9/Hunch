# Hunch

A blind top-ranking game. Someone sets a secret ranking; you see the items one at a time and lock each into a rank before you know what comes next.

**Play it live: https://shekharsanskar9.github.io/Hunch/**

## Features
- **Blind ranking**: items are revealed one by one and every placement is final.
- **23 topics, 41 ready-made rankings**: sports, science, education, coding, databases, apps, social media, brands, fashion, cars, bikes, travel, colors, drinks, chocolates, pens, video games, retro games and more.
- **Set your own**: build a Top 3–10 ranking and share it with a link.
- **Scoring**: 100 / 60 / 30 points for exact / off by one / off by two, with titles from Rookie to Legend.
- **Community rankings and leaderboards**: sign in with Google to publish (5 per day) and to appear on the per-ranking and all-time leaderboards. Backed by Firebase on the free plan; see [FIREBASE_SETUP.md](FIREBASE_SETUP.md).
- **Mind-Reader mode**: your friend sends back their guess, and you predict where they placed every item.

Without Firebase configured, everything runs in the browser and saves to local storage.

## Run
Open `index.html`, or serve the folder:

```
python3 -m http.server 8000
```

Then visit http://localhost:8000.

## Author
Sanskar Shekhar · CSE, BIT Mesra · [@shekharsanskar9](https://github.com/shekharsanskar9)
