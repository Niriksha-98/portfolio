# github portfolio setup guide

## what's in this folder

| file | goes where |
|------|-----------|
| `index.html` | portfolio repo → `docs/index.html` (GitHub Pages) |
| `profile-README.md` | special repo named exactly your GitHub username |
| `distributed-log-analytics-README.md` | `distributed-log-analytics` repo → `README.md` |
| `text-summarization-README.md` | `text-summarization-streaming` repo → `README.md` |

---

## step 1 — profile README

this shows up on your GitHub profile page automatically.

```bash
# create a repo named exactly your github username
gh repo create niriksha-narendra --public
cd niriksha-narendra
cp /path/to/profile-README.md README.md
git add . && git commit -m "add profile readme"
git push
```

visit github.com/niriksha-narendra — the README now shows on your profile.

---

## step 2 — portfolio site (GitHub Pages)

```bash
gh repo create portfolio --public
cd portfolio
mkdir docs
cp /path/to/index.html docs/index.html
git add . && git commit -m "add portfolio site"
git push
```

then in the repo settings on github.com:
- go to **Settings → Pages**
- source: **Deploy from a branch**
- branch: `main`, folder: `/docs`
- save

your site will be live at: `https://niriksha-narendra.github.io/portfolio`

---

## step 3 — project READMEs

```bash
# for distributed log analytics
cd distributed-log-analytics   # your existing repo
cp /path/to/distributed-log-analytics-README.md README.md
git add README.md && git commit -m "add readme"
git push

# for text summarization
cd text-summarization-streaming
cp /path/to/text-summarization-README.md README.md
git add README.md && git commit -m "add readme"
git push
```

---

## step 4 — hockey-analytics-notes

already has a README. just make sure it's pushed:

```bash
cd hockey_mentor
gh repo create hockey-analytics-notes --public --source=. --push
```

---

## final result

- **github.com/niriksha-narendra** — profile page with bio and project table
- **niriksha-narendra.github.io/portfolio** — visual portfolio site
- **github.com/niriksha-narendra/hockey-analytics-notes** — main project
- **github.com/niriksha-narendra/distributed-log-analytics** — project with README
- **github.com/niriksha-narendra/text-summarization-streaming** — project with README
