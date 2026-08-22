# NUI 27 Gallery — deployment guide

Three files, one repo, no server code:

- `index.html` — the public viewer (this is what you share with your audience)
- `studio.html` — your private posting dashboard (keep this link to yourself)
- `feed.json` — the data file that connects them; Studio publishes to it, the viewer reads it

## 1. Create the repo
1. Go to github.com → **New repository**. Make it **public** (GitHub Pages needs a public repo on the free plan). Name it whatever you like, e.g. `nui27-gallery`.
2. Upload `index.html`, `studio.html`, and `feed.json` to the root of the repo (drag-and-drop on the repo's "Add file → Upload files" page works fine).

## 2. Turn on GitHub Pages
1. In the repo: **Settings → Pages**.
2. Under "Build and deployment", set **Source: Deploy from a branch**, **Branch: main**, folder **/ (root)**. Save.
3. GitHub gives you a URL like `https://your-username.github.io/nui27-gallery/`. That's your public viewer — it may take a minute to go live the first time.

## 3. Create a token so Studio can publish
1. Go to **github.com/settings/tokens → Fine-grained tokens → Generate new token**.
2. Give it a name, set expiration however you like.
3. Under **Repository access**, choose **Only select repositories** and pick this one repo.
4. Under **Permissions → Repository permissions**, set **Contents: Read and write**. Leave everything else as-is.
5. Generate, then copy the token (starts with `github_pat_…`) — GitHub only shows it once.

## 4. Connect Studio
1. Open `studio.html` — either the copy on your computer, or `https://your-username.github.io/nui27-gallery/studio.html` once Pages is live.
2. Click **⚙ Connect**, fill in your GitHub username, repo name, branch (`main`), and the token from step 3.
3. Click **Save & publish now**. It'll confirm the connection and write the current board to `feed.json`.

That's it — from now on, every post, reorder, or column change in Studio commits straight to your repo, and the viewer polls for updates every few seconds. A new post typically shows up on the live site within 10–30 seconds (a few seconds for the commit, plus however long GitHub Pages takes to redeploy).

## Notes & trade-offs
- **Keep the Studio link private.** Anyone who has your token could publish through it, and while the token is scoped to just this one repo, it's still worth not posting the `studio.html` URL publicly. Bookmark it for yourself instead.
- **Images are embedded directly in `feed.json`** as compressed JPEGs (resized to ~1400px, so a repo stays lean, but a large gallery will grow the file over time). If you ever hit GitHub's soft repo-size warnings, it's easy to switch to storing images as separate files instead — just say the word.
- **This isn't instant like a live socket** — it's "commit, redeploy, poll," so expect a short lag (seconds, not minutes) rather than truly real-time.
- **Export/Import feed** still works in Studio as a manual backup — use it before big changes if you want a safety net.
