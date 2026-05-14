# `hearthworks/.github` — GitHub organisation profile

This directory is the source for the **organisation landing page** that
will render on <https://github.com/hearthworks>.

GitHub displays the file at `profile/README.md` of the special repo
**`<org>/.github`** as the org's public profile.

## How to deploy

```bash
# 1. From this folder, init a fresh repo
cd ~/works/HEARTH/hearthworks-dot-github
git init -b main
git add .
git commit -s -m "org: initial GitHub profile README"

# 2. Create the special repo on GitHub (must be named exactly ".github")
gh repo create hearthworks/.github --public --source . --remote origin --push
# or via the web UI:
#   https://github.com/organizations/hearthworks/repositories/new
#   name: .github   visibility: Public

# 3. Push
git push -u origin main
```

Within ~30 seconds the new content shows up at
<https://github.com/hearthworks>.

## What to fill in / replace

The README contains a few obvious placeholders — search for them before
the first push:

| Placeholder | Meaning |
|-------------|---------|
| `banner.png` | Optional 1280×320 hero image; drop a real PNG into `profile/banner.png` or remove the `<img>` block |
| GitHub Sponsors link `github.com/sponsors/ysun` | Replace once your Sponsors account is approved (or remove the bullet) |
| Ko-fi / crypto bullets | Remove if you don't actually offer those |
| `syqust@gmail.com` | Replace if you create a project alias like `hello@hearthworks.dev` |
| Sponsor "Your logo here" placeholder row | Remove the whole `<tr>` when you have at least one sponsor |

## Optional but recommended

- Add `profile/banner.png` (logo + tagline). PNG, transparent background,
  ≤300 KB.
- Add an org-wide `FUNDING.yml` at repo root (`./FUNDING.yml`) so the
  *Sponsor* button shows on every repo:

  ```yaml
  github: [ysun]
  custom: ['https://ko-fi.com/yisun']
  ```

- Add `CODE_OF_CONDUCT.md` and `SECURITY.md` at the repo root — GitHub
  surfaces them automatically across the org.

## Maintaining

When the README needs an update later, just edit `profile/README.md` and
push to `main`. There is no build step — GitHub renders the markdown
verbatim.
