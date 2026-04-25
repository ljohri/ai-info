---
layout: default
title: How to Join
---

## Browsing the site (no sign-up)

This **community site is public** on the web. You do **not** need an account, password, or any extra credentials to **read** it—bookmarks and links work for everyone the same way.

## Contributing ideas (no special access)

- **Watch** or **star** the repository on GitHub if you want updates.
- **Open an issue** to suggest changes, new links, or fixes.
- **Submit a pull request** with edits, even as a one-off, if you are comfortable with GitHub’s fork-and-PR flow.

## Joining as an editor (write access to the repo)

If you want to **edit the site and push changes** to the repository (not just suggest them in issues), you need **write access** to the project: either ask a maintainer to add you as a **collaborator** after you’ve agreed how you’ll help, or work via your **fork** and pull requests (you do not need direct push access for that path).

**1. Clone the repository over SSH** (use your own GitHub username and a key you’ve [added to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)):

```bash
git clone git@github.com:ljohri/ai-info.git
cd ai-info
```

(HTTPS alternative: `git clone https://github.com/ljohri/ai-info.git` — same project, no SSH required for read-only `clone` and fork workflows.)

**2. Make a branch, edit (usually under `docs/`), commit, and push** — or open a **pull request** from your fork if you do not have direct push access yet.

**3. If you are invited as a collaborator**, use the same `git clone` URL; your SSH key and GitHub account will be checked when you `git push` to `main` or a feature branch, depending on how the project uses branches and reviews.

*Nothing on this page grants access by itself* — repository permissions are always managed in **GitHub** (Settings → Collaborators, or your fork/PR from there).
