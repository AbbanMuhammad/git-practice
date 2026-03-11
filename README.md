# What is Version Control? Why Git? 

Let's build the mental model first. The best Git users aren't the ones who memorized commands — they're the ones who understand what Git is actually doing. Today is all brain, no typing yet.

---
## The Problem Git Solves
Imagine you're building ExplainIt. You add a feature, it breaks everything. You want to go back — but you already saved over your old code.

Now imagine you're working with 3 other developers on the same file. You make changes, they make changes. Whose version wins?

This is the problem version control solves. It tracks every change ever made to your code, by who, when, and why — and lets you go back to any point in time.

---

## A Brief History
Before Git, developers used tools like CVS and SVN (Subversion). These were centralized — one server held the "real" code, everyone else had a copy. If the server died, everything was gone. Collaboration was painful.
In 2005, Linus Torvalds (the creator of Linux) got frustrated with the existing tools and built Git in about 10 days for himself. His goals were simple:
- Fast — handle massive codebases
- Distributed — everyone has the full history, not just a copy
- Reliable — data integrity built-in (nothing gets silently corrupted)

Today, Git is used by essentially every software company on earth. GitHub, GitLab, and Bitbucket are all built on top of it.

---
## How Git Thinks About Files
This is the most important mental model. Most people assume Git tracks changes to files (like "line 5 changed from X to Y"). That's actually how SVN worked.
Git thinks differently. Git takes snapshots.
Every time you commit, Git takes a photo of what all your files look like at that moment and stores a reference to that snapshot. If a file didn't change, it doesn't store it again — it just links to the previous identical version.

```
Commit 1 → 📸 Snapshot of: index.html, style.css, app.js
Commit 2 → 📸 Snapshot of: index.html (changed), style.css (same→link), app.js (same→link)
Commit 3 → 📸 Snapshot of: index.html (same→link), style.css (changed), app.js (changed)
```
This is why Git is so fast and reliable — it's a stream of snapshots, not a stream of differences.

## The 3 Areas of Git — The Most Important Concept Today

Every file in a Git project lives in one of 3 areas at any given time. Understanding this is what separates people who use Git from people who understand Git.

---
__Standard — Conventional Commits__
The industry uses a format called Conventional Commits. Your message tells what type of change this is:
- __Working Directory —__ This is your project folder. The files you open in Acode and edit. Git knows about these files but isn't recording them yet.
- __Staging Area (Index) —__ This is a preparation zone. You explicitly tell Git "include this file in my next commit" using git add. This is what makes Git powerful — you can change 10 files but only commit 3 of them, precisely.
- __Repository (.git folder) —__ This is the permanent record. Every commit lives here. When you run git commit, the staged snapshot moves here and becomes part of history forever. This is the hidden .git folder inside your project — the one you should never manually edit.
- 
> __Industry insight:__ The staging area is what beginners skip. They do git add . (add everything) every time without thinking. Senior developers stage selectively — committing related changes together and leaving unrelated ones out. You'll learn this properly on Day 3.
---
## What is GitHub? (Git ≠ GitHub)
This confuses almost everyone starting out. Git is the tool. It runs on your phone/computer. It tracks changes locally. It has no idea the internet exists. GitHub is a website that hosts Git repositories online. It adds a social/collaboration layer on top — pull requests, issues, teams, CI/CD. It's owned by Microsoft.
Think of it this way:
- Git is like Microsoft Word's "Track Changes" feature
- GitHub is like Google Drive — a place to store and share those Word documents
You could use Git forever without GitHub. But GitHub without Git is nothing — it's just a file host.

> Alternatives to GitHub: GitLab (popular in enterprises, has built-in CI/CD), Bitbucket (popular with Atlassian/Jira teams). All of them work with the same Git commands — only the website changes.
---

## Your First Repository
Today you go from reader to practitioner. By the end of this lesson you'll have a real Git repo with a real commit history, connected to GitHub.

---
## Part 1 — The .git Folder
When you run git init inside a folder, Git creates a hidden .git folder. This folder is your repository. Everything — every commit, every branch, every piece of history — lives inside it.
```
my-project/
├── .git/          ← The entire Git database. Don't touch this manually.
│   ├── HEAD       ← Points to your current branch
│   ├── config     ← Repo-specific settings
│   ├── objects/   ← Every file/commit ever stored
│   └── refs/      ← Branch and tag pointers
├── index.html
└── style.css
```

> If you delete the .git folder, Git forgets everything. Your files remain but all history is gone. This is why the .git folder is the most important folder in your project.
On Termux, hidden folders (starting with .) don't show with ls. Use ls -la to see them.
---

## Part 2 — SSH Key Setup (Do This Once, Forever)

Before touching code, let's connect Termux to GitHub securely. SSH keys let GitHub verify it's really you — no typing passwords on every push.
- __How it works:__ SSH generates two keys — a private key (stays on your phone, never share it) and a public key (you give this to GitHub). When you push, they handshake and authenticate.
Run these commands one by one in Termux:

```
# Step 1 — Generate your SSH key pair
ssh-keygen -t ed25519 -C "your-github-email@example.com"
# Press Enter 3 times (accept default location, no passphrase for now)

# Step 2 — View your PUBLIC key (this is what you give GitHub)
cat ~/.ssh/id_ed25519.pub
# Copy the entire output — it starts with "ssh-ed25519"

# Step 3 — Test the connection after adding to GitHub
ssh -T git@github.com
# You should see: "Hi AbbanMuhammad! You've successfully authenticated"
```

__Adding to GitHub:__
1. Go to github.com → Settings → SSH and GPG keys
2. Click "New SSH key"
3. Paste what you copied from Step 2
4. Title it something like "Termux - Android"

> __On a PC:__ Exact same commands in Git Bash (Windows) or Terminal (Mac/Linux). The only difference is the key saves to C:\Users\YourName\.ssh\ on Windows instead of ~/.ssh/.

## Part 3 — Your First Repository, Step by Step

Now let's build something real. You'll create a project called git-practice — this will be your sandbox for the entire course.

```
# Navigate to your home folder in Termux
cd ~

# Create a new project folder
mkdir git-practice
cd git-practice

# Initialize Git — this creates the .git folder
git init

# Verify the .git folder exists
ls -la
```
You should see .git listed. Git is now watching this folder.
```
# Create your first file
echo "# Git Practice Repo" > README.md
echo "This is where I learn Git properly." >> README.md

# Check what Git sees right now
git status
```
You'll see something like this:
```
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md
```
__"Untracked"__ means Git sees the file exists but isn't tracking it yet. It's in your Working Directory only.
```
# Move README.md to the Staging Area
git add README.md

# Check status again — notice the difference
git status
```
Now it says:
```
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file: README.md
```
The file is now staged — ready to be committed.
```
# Create the commit — take the snapshot
git commit -m "docs: add README
```
with project description"
```
# View your commit
git log --oneline
```
You've just made your first commit. That snapshot now lives permanently in your .git folder.

---
## Part 4 — Pushing to GitHub
```
# On GitHub: create a NEW empty repository called "git-practice"
# Don't add README or .gitignore — leave it completely empty

# Back in Termux — connect your local repo to GitHub
git remote add origin git@github.com:AbbanMuhammad/git-practice.git

# Verify the connection
git remote -v

# Push your commit to GitHub
git push -u origin main
```
The -u flag sets origin main as the default, so next time you just type git push.

> What is origin? It's just a nickname for the remote URL. You could call it anything, but origin is the universal convention. Every developer on earth names their main remote origin.

---

The Workflow You'll Use Every Day
```
1. Edit files in Acode
       ↓
2. git status          (what changed?)
       ↓
3. git add <file>      (stage what's ready)
       ↓
4. git commit -m "..."  (snapshot it)
       ↓
5. git push            (sync to GitHub)
```
Memorise this loop. It's the heartbeat of every developer's day


|Prefix | Used For|
|:------ | :--------|
|feat: |A new feature|
|fix:|A bug fix|
|docs:|Documentation only|
|style:|formatting, no logic change|
|refactor:|Restructuring code, no new feature|
|chore:|setup, config, tooling|

# Good commit messages
git commit -m "feat: add dark mode toggle"
git commit -m "fix: correct login redirect bug"
git commit -m "docs: update README with setup instructions"

# Bad commit messages (never do these)
1. git commit -m "update"
2. git commit -m "stuff"
3. git commit -m "asd"