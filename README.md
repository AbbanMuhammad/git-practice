# Git Practice Repo

# A Comprehensive Notes and Industry Standard/Good Practice

## What is Version Control? Why Git?
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
## Standard — Conventional Commits
The industry uses a format called Conventional Commits. Your message tells what type of change this is:
__Working Directory —__ This is your project folder. The files you open in Acode and edit. Git knows about these files but isn't recording them yet.
__Staging Area (Index) —__ This is a preparation zone. You explicitly tell Git "include this file in my next commit" using git add. This is what makes Git powerful — you can change 10 files but only commit 3 of them, precisely.
__Repository (.git folder) —__ This is the permanent record. Every commit lives here. When you run git commit, the staged snapshot moves here and becomes part of history forever. This is the hidden .git folder inside your project — the one you should never manually edit.

>__Industry insight:__ The staging area is what beginners skip. They do git add . (add everything) every time without thinking. Senior developers stage selectively — committing related changes together and leaving unrelated ones out. You'll learn this properly on Day 3.

--
## What is GitHub? (Git ≠ GitHub)
This confuses almost everyone starting out.
Git is the tool. It runs on your phone/computer. It tracks changes locally. It has no idea the internet exists.
GitHub is a website that hosts Git repositories online. It adds a social/collaboration layer on top — pull requests, issues, teams, CI/CD. It's owned by Microsoft.
Think of it this way:
- Git is like Microsoft Word's "Track Changes" feature
- GitHub is like Google Drive — a place to store and share those Word documents
You could use Git forever without GitHub. But GitHub without Git is nothing — it's just a file host.

>Alternatives to GitHub: GitLab (popular in enterprises, has built-in CI/CD), Bitbucket (popular with Atlassian/Jira teams). All of them work with the same Git commands — only the website changes.
---

|Prefix|Used For|
|------|--------|
|feat: |A new feature|
|fix:  |A bug fix|
|docs: |Documentation only|
|style:|formatting, no logic change|
|refactor:|Restructuring code, no new feature|
|chore:|setup, config, tooling|

# Good commit messages
git commit -m "feat: add dark mode toggle"
git commit -m "fix: correct login redirect bug"
git commit -m "docs: update README with setup instructions"

# Bad commit messages (never do these)
git commit -m "update"
git commit -m "stuff"
git commit -m "asd"

Edited directly on GitHub ✍️ 
