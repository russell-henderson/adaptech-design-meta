Understood. You’ve got the **web remote URL**:

* `https://github.com/russell-henderson/adaptech-design-web`

And you’re seeing **another branch named `aadaptech-design`** that “will not commit.”

That symptom almost always comes from one of these states:

* you’re in a **detached HEAD** (not actually on a branch), or
* the branch is in a weird state (or you’re trying to commit in the wrong repo), or
* you created a branch name you did not intend and you’re currently on a different branch than you think.

Below is the clean fix path, step-by-step, with exact commands.

---

# Step 1: Fix the root repo branch so it can commit normally

Run this in **root** (`F:\adaptech-design`):

```bash
git -C F:/adaptech-design status
git -C F:/adaptech-design branch -vv
git -C F:/adaptech-design rev-parse --abbrev-ref HEAD
```

Now apply this **without guessing**:

## If `rev-parse` prints `HEAD` (detached)

Attach to a real branch:

```bash
git -C F:/adaptech-design switch -c main
```

## If it prints `aadaptech-design` (you’re on that branch)

Rename it to `main`:

```bash
git -C F:/adaptech-design branch -M main
```

## If it prints something else (like `master` or `adaptech-design`)

Rename it to `main`:

```bash
git -C F:/adaptech-design branch -M main
```

At this point, committing should work.

If `SIDE.md` is modified and you want it included:

```bash
git -C F:/adaptech-design add SIDE.md
git -C F:/adaptech-design commit -m "docs: update SIDE instructions"
```

(If you do not want to commit it, revert it instead: `git -C F:/adaptech-design restore SIDE.md`)

---

# Step 2: Set up and push the web repo remote

Run these in **web** (`F:\adaptech-design\web`):

```bash
git -c safe.directory=F:/adaptech-design/web -C F:/adaptech-design/web remote -v
git -c safe.directory=F:/adaptech-design/web -C F:/adaptech-design/web branch -vv
```

If no origin exists, add it:

```bash
git -c safe.directory=F:/adaptech-design/web -C F:/adaptech-design/web remote add origin https://github.com/russell-henderson/adaptech-design-web.git
```

Make sure branch is `main`:

```bash
git -c safe.directory=F:/adaptech-design/web -C F:/adaptech-design/web branch -M main
```

Push:

```bash
git -c safe.directory=F:/adaptech-design/web -C F:/adaptech-design/web push -u origin main
```

---

# Step 3: Root repo remote (you did not give this URL yet)

You still need a **root repo URL** (separate from the web repo) if you want the **submodule workflow** to work correctly.

Typical root repo URL would be something like:

* `https://github.com/russell-henderson/adaptech-design` (root docs repo)

Once you have it, you will run:

```bash
git -C F:/adaptech-design remote add origin <ROOT_REPO_URL>.git
git -C F:/adaptech-design push -u origin main
```

---

# Step 4: Point the root submodule to the web remote

After both repos have origins and are pushed:

```bash
git -C F:/adaptech-design config -f .gitmodules submodule.web.url https://github.com/russell-henderson/adaptech-design-web.git
git -C F:/adaptech-design add .gitmodules web
git -C F:/adaptech-design commit -m "chore(submodule): point web to remote origin"
git -C F:/adaptech-design push
```

---

## What I need from you now

Paste **the root repo URL** you want to use (the docs + submodule pointer repo). It will look like:

* `https://github.com/russell-henderson/<something>`

Once you paste it, I’ll give you the exact final command block with no placeholders.
