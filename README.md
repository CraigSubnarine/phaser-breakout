# phaser-breakout

Breakout-style game in Phaser 3. One `index.html`, no build step, no asset files (shapes and sounds are made in code).

- **Play:** https://craigsubnarine.github.io/phaser-breakout/
- **Repo:** https://github.com/CraigSubnarine/phaser-breakout
- **Run locally:** open `index.html` in a browser.
- **Controls:** mouse or arrow keys / A-D to move; click or Space to launch. On a phone: slide your finger anywhere (the strip below the game works well) and tap to launch.
- **Pause:** the button in the top-right corner, or Esc / P. The game also pauses itself if you switch tabs or apps.
- **Status:** v1 playable on desktop and phones. Opens on a main menu (Play, Settings, Back to Arcade). Settings: difficulty (Easy: 5 lives, wide paddle, slower ball; Normal; Hard: small paddle, faster ball) and sound on/off, both remembered. Best score is kept per difficulty. Levels add a row and speed up. Phones held upright get a taller playfield.

## Publish

Hosted on GitHub Pages from the `main` branch (root folder). Push to `main` and the site updates in about a minute.

```bash
git add -A && git commit -m "describe the change" && git push
```

Check the deploy: `gh api repos/{owner}/{repo}/pages/builds/latest --jq .status` (run inside this folder).

## Lessons

- Phaser Arcade Physics: `setBounce(1)` + `setCollideWorldBounds` gives a ball for free; turning off the bottom wall (`setBoundsCollision(true, true, true, false)`) is how a ball gets lost.
- Steering off the paddle: set the ball's angle from where it hit the paddle instead of letting physics bounce it. Keeps speed constant and gives the player control.
- Bug found in testing: when a level was cleared, the ball froze where the last brick had been and hit a brick of the next level on its first frame. Fix: snap the ball back to the paddle when resetting.
- Mobile: a fixed 800x600 game shrinks to under half its size on an upright phone, so the playfield switches to 600x900 when the screen is taller than wide. Listening for touches on the whole page (not just the game canvas) lets your thumb steer from below the game. Phones only allow sound after a tap, so audio starts on the first touch.
- Menus: plain HTML buttons over the canvas were much less code than drawing menus in Phaser, and they're tappable and keyboard-friendly for free. The game tracks one `state` ('menu', 'playing', 'paused', 'over') and only accepts game input while 'playing'.
- Bugs found in testing: `this.scene.pause()`/`resume()` in Phaser 3.90 wait until the next frame, so pause then Main menu in quick succession left the game frozen. Fixed with `this.sys.pause()`/`resume()`, which act immediately. Also, quitting right as a level was cleared could still load the next level; fixed by cancelling that timer directly.
