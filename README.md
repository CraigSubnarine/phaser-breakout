# phaser-breakout

Breakout-style game in Phaser 3. One `index.html`, no build step, no asset files (shapes and sounds are made in code).

- **Play:** _(GitHub Pages link goes here once published)_
- **Run locally:** open `index.html` in a browser.
- **Controls:** mouse/touch or arrow keys / A-D to move; click, tap or Space to launch.
- **Status:** v1 playable. 3 lives, levels add a row and speed up, best score saved in the browser.

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
