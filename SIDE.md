Choose **Path 1 (two repos)**.

Reason: you already have a working submodule setup and a clean `web` history. Path 2 would force a structural rewrite (submodule removal + subtree import) and increases risk for zero immediate benefit.

What you do next is entirely deterministic once you have the URLs.

## You paste (exact labels)

* `ROOT_REPO_URL=...`
* `WEB_REPO_URL=...`

## Then run these commands in this order

### 0) Make sure nothing is using `web/` (close dev server, VS Code file watchers if possible)

### 1) Configure and push the **web** repo first

Because the root submodule pointer should reference a commit that exists on the remote.

```bash
git -c safe.directory=F:/adaptech-design/web -C F:/adaptech-design/web remote add origin <WEB_REPO_URL>
git -c safe.directory=F:/adaptech-design/web -C F:/adaptech-design/web branch -M main
git -c safe.directory=F:/adaptech-design/web -C F:/adaptech-design/web push -u origin main
```

### 2) Configure and push the **root** repo

```bash
git -C F:/adaptech-design remote add origin <ROOT_REPO_URL>
git -C F:/adaptech-design branch -M main
git -C F:/adaptech-design push -u origin main
```

### 3) Make the root repo’s submodule point at the **web remote**

Right now the submodule likely points to a local path. Set it to the remote URL:

```bash
git -C F:/adaptech-design config -f .gitmodules submodule.web.url <WEB_REPO_URL>
git -C F:/adaptech-design add .gitmodules web
git -C F:/adaptech-design commit -m "chore(submodule): point web to remote origin"
git -C F:/adaptech-design push
```

### 4) Verify a fresh clone would work (quick local check)

In any temp folder:

```bash
git clone <ROOT_REPO_URL> adaptech-design-clone-test
cd adaptech-design-clone-test
git submodule update --init --recursive
```

If that completes without prompts/errors, you’re done.

---

## Important note about SIDE.md being modified

Before pushing root, either:

* commit the latest SIDE.md change, or
* revert it so the root push is deterministic.

To commit it:

```bash
git -C F:/adaptech-design add SIDE.md
git -C F:/adaptech-design commit -m "docs: update SIDE instructions"
```

Then continue with the root push steps above.

---

Paste the two URLs and I’ll respond with the final command block with the placeholders replaced.
