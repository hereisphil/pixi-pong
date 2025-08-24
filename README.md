# 🎾 PIXI Pong!

A lightweight browser game built with **PixiJS**. Press **Play** and watch a ball bounce around the canvas. Each time it hits a side, that border turns red. When **all four sides** have been hit, the game ends with a retro **Game Over** screen and a **Play** button to restart.

---

## ✨ Features

- **PixiJS renderer** with animated tiling background + brightness filter
- **Bouncing ball** with simple physics (velocity reversal on bounds)
- **Hit tracking**: left/top/right/bottom borders light up red when struck
- **Win/End state** using a Promise (`checkBounce`) that resolves once all sides were hit
- Simple **UI** (`Play` button, status text, GIF)
- Zero build tools—just open and play

---

## 📸 Screenshot

Here's a GIF of the app running:

![PIXI Pong demo — ball bounces and walls turn red when hit](https://i.ibb.co/1GddgNtd/pixipong-ezgif-com-optimize.gif)

---

## 🧰 Tech Stack

- **PixiJS** via CDN: `https://pixijs.download/release/pixi.min.js`
- **HTML / CSS / ES Modules**
- No bundler required

---

## 📁 Project Structure

```tree
├─ index.html          # App shell, loads PixiJS + script module
├─ style.css           # Page layout & theming
├─ script.js           # Game logic (Pixi setup, physics, end state)
├─ background.jpg      # Scrolling background image (Pixabay credit)
└─ gameover.gif        # “Game Over” GIF
```

---

## ▶️ Run Locally

You can open `index.html` directly, but using a local server is recommended (avoids CORS issues and mirrors production behavior).

### Option A: VS Code Live Server

1. Install the **Live Server** extension.
2. Right-click `index.html` → **Open with Live Server**.
3. Visit the shown URL (e.g., `http://127.0.0.1:5500`).

### Option B: Any Static Server

```bash
# Python 3
python3 -m http.server 5500

# or Node (if installed globally)
npx serve .
```

Then open http://localhost:5500

---

## 🕹️ How to Play

1. Click Play.
2. The ball moves and bounces off the four edges.
3. Each edge turns red the first time it’s hit.
4. When all four have been hit, the game ends:
   - “All 4 sides have been hit!” text shows
   - The Game Over GIF appears
   - The Play button returns so you can try again

## 🧠 How It Works (Quick Tour)

### Pixi App

```javascript
const app = new PIXI.Application();
await app.init({ backgroundColor: "#3398b9", width: 800, height: 500 });
pongTable.appendChild(app.canvas);
```

### Background + Filter + Scroll

```javascript
const bgTexture = await PIXI.Assets.load("./background.jpg");
const tiling = new PIXI.TilingSprite({
  texture: bgTexture,
  width: 800,
  height: 800,
});
tiling.filters = [new PIXI.ColorMatrixFilter()];
tiling.filters[0].brightness(0.5);
app.stage.addChild(tiling);
app.ticker.add(() => (tiling.tilePosition.x -= 1));
```

### Edges + Ball

- Four PIXI.Graphics() rectangles form the borders.
- A PIXI.Graphics().circle() is positioned randomly and moves via simple x/y velocity.
- On collision with canvas bounds, velocity is flipped (xv = -xv or yv = -yv) and the - struck border is redrawn in red.

### Game End (Promise)

```javascript
const checkBounce = () =>
  new Promise((resolve) => {
    if (leftBounce && topBounce && rightBounce && bottomBounce) {
      // Show end UI, stop ticker, reset flags, etc.
      resolve();
    }
  });
```

### Start / Restart

- The Play button hides UI, starts a PIXI.Ticker, and resumes motion.
- When the Promise resolves, ticker stops and “Game Over” UI shows.

---

## When the Promise resolves, ticker stops and “Game Over” UI shows.

- No canvas / images? Serve the project over http:// (Live Server) rather than opening with file://.

- Nothing happens when clicking Play? Check the browser console for errors and ensure PixiJS is loaded (CDN reachable).

## 🙏 Credits

- [PixiJS](https://pixijs.com/)
- Background image by prawny on Pixabay
- Game Over GIF: from Giphy (“game over” category)

## 👋 About the Author

Hi! I’m Phillip Cantu, an American digital nomad, a Full Sail University web development student, and a [4Geeks Academy Full Stack Development with AI Bootcamp](https://4geeksacademy.com/us/apply?ref=REFERRALQEZPTJCK-17696). I’m passionate about creating modern, user-friendly, and practical applications that help people learn and build faster.

You can find me here:

- **GitHub:** [hereisphil](https://github.com/hereisphil)
- **LinkedIn:** [PhillipCantu](https://www.linkedin.com/in/phillipcantu/)
- **Instagram:** [@philtheotaku](https://www.instagram.com/philtheotaku/)
