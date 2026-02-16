Good recovery. Given the lock contention, **submodule is an acceptable stopgap** and it keeps you moving without fighting Windows file locks. You can always convert to subtree later when nothing has `web/` open.

### Do next (in order)

## 1) Commit the two untracked root files

From root:

```bash
git -C F:/adaptech-design add .markdownlint-cli2.yaml SIDE.md
git -C F:/adaptech-design commit -m "chore: add markdownlint config and SIDE instructions"
```

## 2) Verify submodule is properly recorded

Run:

```bash
git -C F:/adaptech-design submodule status
```

You should see one line for `web` with a commit hash.

Also check root status is clean:

```bash
git -C F:/adaptech-design status --short
```

## 3) Add origin and push (once you have the URL)

When you have the GitHub URL for the **root** repo:

```bash
git -C F:/adaptech-design remote add origin <ROOT_REPO_URL>
git -C F:/adaptech-design branch -M main
git -C F:/adaptech-design push -u origin main
```

### Important: pushing submodules

A root push only records the submodule pointer. The **web repo must also have its own origin + push**, otherwise collaborators won’t be able to fetch the submodule content.

So after root origin is set, also set `web` origin:

```bash
git -c safe.directory=F:/adaptech-design/web -C F:/adaptech-design/web remote add origin <WEB_REPO_URL>
git -c safe.directory=F:/adaptech-design/web -C F:/adaptech-design/web branch -M main
git -c safe.directory=F:/adaptech-design/web -C F:/adaptech-design/web push -u origin main
```

If you intend for **one single GitHub repo** only, don’t push yet, because submodules require a separate repo for `web`.

## 4) Document the repo structure clearly (quick, 2 lines)

Add a short note to `PLANS.md` (root) under “Repo Structure”:

* Root repo holds docs and tracks `web/` as a submodule (temporary due to file-lock contention).
* `web/` is the Next.js app repo with its own history.

---

### What I need from you

Paste either:

* the **single GitHub URL** you want to use (if you plan to keep only `web` on GitHub and root local-only), or
* **two URLs**: one for root, one for web (best practice with submodules).

Once you paste the URL(s), I’ll give you the exact remote commands with no ambiguity.
